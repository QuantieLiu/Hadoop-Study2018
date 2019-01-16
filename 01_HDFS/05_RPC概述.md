
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
  

<li>URL
```
代码-hadoop RPC功能demo
http://blog.51cto.com/sbp810050504/1925405
代码-hadoop RPC应用示例
https://www.cnblogs.com/edisonchou/p/4285817.html
```
