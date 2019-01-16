
#### 概述
<li>Remote Procedure Call Protocol
<li>远程过程调用协议

```
一种通过网络从远程计算机程序上请求服务，允许一台计算机程序远程调用另外一台计算机的子程序,而不需要了解底层网络技术的协议
RPC协议假定某些传输协议的存在，如TCP或UDP，为通信程序之间携带信息数据。
在OSI网络通信模型中，RPC跨越了传输层和应用层。RPC使得开发包括网络分布式多程序在内的应用程序更加容易
```

#### 主要组成

```
通信模块：实现请求应该协议。主要分为同步方式和异步方式
stub程序：客户端和服务器均包含stub程序，可以看做代理程序。使得远程函数表现的跟本地调用一样，对用户程序完全透明。
调度程序：接受来自通信模块的请求消息，根据标识选择stub程序处理。并发量大一般采用线程池处理。
客户程序/服务过程：请求发出者和请求的处理者
```

#### 总体架构

<li>序列化层

```
支持多种框架实现序列化与反序列化，
将结构化对象转换为字节流以便通过网络进行传输或者写入持久存储
在RPC框架中，它主要用于将用户请求中的参数或者应答转换成字节流以便跨机传输
```

<li>函数调用层

```
定位要调用的函数，并执行该函数
利用java反射与动态代理实现
```  
  
<li>网络传输层

```
  描述了Client和Server之间消息的传输方式
  基于TCP/IP的Socket机制
```

<li>服务端处理框架

```
描述了客户端和服务器端信息交互的方式，直接决定了服务器端的并发处理能力
基于Reactor模式的事件驱动IO模型，常见的网络I/O模型有阻塞式I/O，非阻塞式I/O、事件驱动式I/O等
```

#### 显著特点

```
透明性：远程调用其他机器上的程序，对用户来说就像是调用本地方法一样；
高性能：RPC Server能够并发处理多个来自Client的请求；
可控性：jdk中已经提供了一个RPC框架—RMI，但是该PRC框架过于重量级并且可控之处比较少，所以Hadoop RPC实现了自定义的PRC框架。
```

#### 使用步骤

<li>1 定义RPC协议
<br>RPC协议是客户端和服务器端之间的通信接口，它定义了服务器端对外提供的服务接口
<li>2 实现RPC协议
<br>Hadoop RPC协议通常是一个Java接口，用户需要实现该接口
<li>3 构造和启动RPC SERVER
<br>直接使用静态类Builder构造一个RPC Server，并调用函数start()启动该Server
<li>4 构造RPC Client并发送请求
<br>使用静态方法getProxy构造客户端代理对象，直接通过代理对象调用远程端的方法
  
```
--为某个协议（java接口）实例构造服务器对象，用于客户端处理发送的请求
public static <T> Server RPC.Builder(Configuration).build();
--用于构造客户端代理对象（该对象实现了某个协议），用于向服务器端发送RPC请求
public static <T> T getProxy/waitForProxy(Class<T> protocol,long clientVersion,InetSocketAddress addr, Configuration conf,SocketFactory factory) throws IOException;
```
  
#### org.apache.hadoop.ipc.RPC类解析

```
ClientCache(成员变量)：根据用户提供的SocketFactory来缓存Client对象，以便重用Client对象。
Server(内部类)：继承Server抽象类，利用反射实现了call方法，即客户端请求的方法和对应参数完成方法调用。
Invocation(内部类)：将要调用的方法名和参数打包成可序列化的对象，方便客户端和服务器之间传递。
```

#### 客户端和服务器端的关系

```
Client-NameNode之间，其中NameNode是服务器
Client-DataNode之间，其中DataNode是服务器
DataNode-NameNode之间，其中NameNode是服务器
DataNode-DateNode之间，其中某一个DateNode是服务器，另一个是客户端
```

#### org.apache.hadoop.ipc.Client类解析

```
Call(内部类)：封装了一个RPC请求，包含5个成员变量，唯一表示id、函数调用信息param、函数返回值value、函数异常信息error、函数完成标识done。Hadoop rpc server采用异步方式处理客户端请求，使得远程过程调用的发生顺序和返回顺序无直接关系，而客户端正是通过id识别不同的函数调用。当客户端向服务器发送请求，只需填充id和param两个变量，其余3个变量由服务器端根据函数执行情况填充。
Connection(内部类，一个线程)：是client和server之间的一个通信连接，封装了连接先关的基本信息和操作。基本信息包括：通信连接唯一标识remoteId(ConnectionId)、与Server端通信的scoket、网络输入输出流in/out、保存RPC请求的哈希表calls(Hashtable<Integer, Call>)。操作包括：addCall将一个Call对象添加到哈希表中；sendParam想服务器端发送RPC请求；receiveResponse从服务器端接收已经处理完成的RPC请求；run调用receiveResponse方法，等待返回结果。
ConnectionId(内部类)：连接的标记（包括server地址，协议，其他一些连接的配置项信息）
ParallelCall(内部类)：实现并行调用的请求
ParallelResults(内部类)：并行调用的执行结果
```

##### Client类中主要对外通过两个接口，分别用于单个远程调用和批量远程调用

```
public Writable call(Writable param, ConnectionId remoteId)  throws InterruptedException, IOException
public Writable call(Writable param, InetSocketAddress addr,  Class<?> protocol, UserGroupInformation ticket,
                       int rpcTimeout, Configuration conf)  throws InterruptedException, IOException
```

##### 调用流程分析，当调用call函数执行某个远程方法

```
1）创建一个Connection对象，并将远程方法调用信息封装成Call对象，放到Connection对象中的哈希表中；
2）调用Connection类中的sendRpcRequest()方法将当前Call对象发送给Server端；
3）Server端处理完RPC请求后，将结果通过网络返回给Client端，Client端通过receiveRpcResponse()函数获取结果；
4）Client检查结果处理状态（成功还是失败），并将对应Call对象从哈希表中删除。
```
##### 一个Client包含多个连接，private Hashtable<ConnectionId, Connection> connections = new Hashtable<ConnectionId, Connection>();


<li>URL
```
代码-hadoop RPC功能demo
http://blog.51cto.com/sbp810050504/1925405
代码-hadoop RPC应用示例
https://www.cnblogs.com/edisonchou/p/4285817.html
```
