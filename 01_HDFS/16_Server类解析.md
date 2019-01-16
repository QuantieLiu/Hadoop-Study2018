
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

<li>主要角色划分

```
Reactor：I/O事件的派发者。
Acceptor：接受来自Client的连接，建立与Client对应的Handler，并向Reactor注册此Handler。
Handler：与一个Client通信的实体，并按一定的过程实现业务的处理。Handler内部往往会有更进一步的层次划分，用来抽象诸如read、decode、compute、encode和send等过程。
         在Reactor模式中，业务逻辑被分散的I/O事件所打破，所以Handler需要有适当的机制在所需的信息还不全（读到一半）的时候保存上下文，并在下一次I/O事件到来的时候（另一半可读）能继续上次中断的处理。
Reader/Sender：为了加速处理速度，Reactor模式往往构建一个存放数据处理线程的线程池，这样数据读出后，立即扔到线程池中等待后续处理即可。
        为此，Reactor模式一般分离Handler中的读和写两个过程，分别注册成单独的读事件和写事件，并由对应的Reader和Sender线程处理。
```

<li>

```

```
