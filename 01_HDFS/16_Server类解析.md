
### org.apache.hadoop.ipc.Server类解析

#### 背景

```
采用了Master/Slave结构，其中Master是整个系统的单点，如NameNode或JobTracker，这是制约系统性能和可扩展性的最关键因素之一
Master通过ipc.Server接收并处理所有Slave发送的请求，要求ipc.Server具备高并发和可扩展性
采用提高并发处理能力的技术：线程池、事件驱动和Reactor设计模式
```

#### reactor设计模式

```
Reactor是并发编程中的一种基于事件驱动的设计模式
特点1：通过派发/分离I/O操作事件提高系统的并发性能；
特点2：提供了粗粒度的并发控制，使用单线程实现，避免了复杂的同步处理
```

待插入图片 16001Reactor多线程模型.png

#### 主要角色划分
<li>Reactor
<br>I/O事件的派发者
<li>Acceptor
<br>接受来自Client的连接，建立与Client对应的Handler，并向Reactor注册此Handler
<li>Handler
<br>与一个Client通信的实体，并按一定的过程实现业务的处理
<br>Handler内部往往会有更进一步的层次划分，用来抽象诸如read、decode、compute、encode和send等过程
<br>在Reactor模式中，业务逻辑被分散的I/O事件所打破
<br>Handler需要有适当的机制在所需的信息还不全（读到一半）的时候保存上下文，并在下一次I/O事件到来的时候（另一半可读）能继续上次中断的处理
<li>Reader/Sender
<br>Reactor模式往往构建一个存放数据处理线程的线程池
<br>数据读出后，立即扔到线程池中等待后续处理即可
<br>Reactor模式一般分离Handler中的读和写两个过程，分别注册成单独的读事件和写事件，并由对应的Reader和Sender线程处理
         
#### server处理流程
<br>接收来自客户端的RPC请求，经过调用相应的函数获取结果后，返回给对应的客户端

<li>接收请求-阶段

```
接收来自各个客户端的RPC请求，并将它们封装成固定的格式（Call类）放到一个共享队列（callQueue）中
分为建立连接和接收请求两个子阶段，分别由Listener和Reader两种线程完成

整个Server只有一个Listener线程，统一负责监听来自客户端的连接请求，一旦有新的请求到达，它会采用轮询的方式从线程池中选择一个Reader线程进行处理
Reader线程可同时存在多个，它们分别负责接收一部分客户端连接的RPC请求
至于每个Reader线程负责哪些客户端连接，完全由Listener决定
当前Listener只是采用了简单的轮询分配机制

Listener和Reader线程内部各自包含一个Selector对象，分别用于监听SelectionKey.OP_ACCEPT和SelectionKey.OP_READ事件
对于Listener线程，主循环的实现体是监听是否有新的连接请求到达，并采用轮询策略选择一个Reader线程处理新连接
对于Reader线程，主循环的实现体是监听（它负责的那部分）客户端连接中是否有新的RPC请求到达，并将新的RPC请求封装成Call对象，放到共享队列callQueue中
```

<li>处理请求-阶段

```
从共享队列callQueue中获取Call对象，执行对应的函数调用，并将结果返回给客户端，这全部由Handler线程完成
Server端可同时存在多个Handler线程，它们并行从共享队列中读取Call对象，经执行对应的函数调用后，将尝试着直接将结果返回给对应的客户端
考虑到某些函数调用返回结果很大或者网络速度过慢，可能难以将结果一次性发送到客户端，此时Handler将尝试着将后续发送任务交给Responder线程
```

<li>返回结果-阶段

```
Server端仅存在一个Responder线程，它的内部包含一个Selector对象，用于监听SelectionKey.OP_WRITE事件
当Handler没能将结果一次性发送到客户端时，会向该Selector对象注册SelectionKey.OP_WRITE事件，进而由Responder线程采用异步方式继续发送未发送完成的结果
```

```
--Reactor延伸
http://www.cnblogs.com/duanxz/p/3696849.html
```
