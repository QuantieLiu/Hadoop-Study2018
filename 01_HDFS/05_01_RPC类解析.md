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

一个Client包含多个连接，private Hashtable<ConnectionId, Connection> connections = new Hashtable<ConnectionId, Connection>();
