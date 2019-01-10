
<li>BeanFactory not initialized or already closed - call 'refresh' before accessing beans via the ApplicationContext 
存在重复的类，简称是否存在冲突文件

```
一月 10, 2019 11:27:07 上午 org.apache.catalina.core.StandardContext listenerStop
严重: Exception sending context destroyed event to listener instance of class org.springframework.web.context.ContextLoaderListener
java.lang.IllegalStateException: BeanFactory not initialized or already closed - call 'refresh' before accessing beans via the ApplicationContext
	at org.springframework.context.support.AbstractRefreshableApplicationContext.getBeanFactory(AbstractRefreshableApplicationContext.java:170)
	at org.springframework.context.support.AbstractApplicationContext.destroyBeans(AbstractApplicationContext.java:1033)
	at org.springframework.context.support.AbstractApplicationContext.doClose(AbstractApplicationContext.java:1009)
	at org.springframework.context.support.AbstractApplicationContext.close(AbstractApplicationContext.java:961)
	at org.springframework.web.context.ContextLoader.closeWebApplicationContext(ContextLoader.java:581)
	at org.springframework.web.context.ContextLoaderListener.contextDestroyed(ContextLoaderListener.java:116)
	at org.apache.catalina.core.StandardContext.listenerStop(StandardContext.java:5146)
	at org.apache.catalina.core.StandardContext.stopInternal(StandardContext.java:5810)
	at org.apache.catalina.util.LifecycleBase.stop(LifecycleBase.java:224)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:159)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1571)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1561)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)

一月 10, 2019 11:27:07 上午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["http-bio-8080"]
一月 10, 2019 11:27:07 上午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["ajp-bio-8009"]
```
