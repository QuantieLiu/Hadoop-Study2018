
#### 配置1-启动成功输出

``` 
[11899517@bigdata4 flume]$ ./bin/flume-ng agent --conf conf --conf-file conf/flume-conf-netsrc.properties --name agent -Dflume.root.logger=INFO,console
Info: Sourcing environment configuration script /mnt/home/11899517/flume/conf/flume-env.sh
Info: Including Hadoop libraries found via (/home/hadoop/hadoop-current/bin/hadoop) for HDFS access
Info: Including Hive libraries found via () for Hive access
+ exec /mnt/home/11899517/soft/jdk1.8.0_11/bin/java -Xmx20m -Dflume.root.logger=INFO,console -cp '/mnt/home/11899517/flume/conf:/mnt/home/11899517/flume/lib/*:/home/hadoop/hadoop-2.7.6/etc/hadoop:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/common/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/*:/home/hadoop/hadoop-current/contrib/capacity-scheduler/*.jar:/lib/*' -Djava.library.path=:/home/hadoop/hadoop-2.7.6/lib/native org.apache.flume.node.Application --conf-file conf/flume-conf-netsrc.properties --name agent
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/mnt/home/11899517/flume/lib/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
2018-12-30 22:14:51,191 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider.start(PollingPropertiesFileConfigurationProvider.java:62)] Configuration provider starting
2018-12-30 22:14:51,197 (conf-file-poller-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider$FileWatcherRunnable.run(PollingPropertiesFileConfigurationProvider.java:134)] Reloading configuration file:conf/flume-conf-netsrc.properties
2018-12-30 22:14:51,204 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:loggerSink
2018-12-30 22:14:51,204 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:loggerSink
2018-12-30 22:14:51,204 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:930)] Added sinks: loggerSink Agent: agent
2018-12-30 22:14:51,215 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration.validateConfiguration(FlumeConfiguration.java:140)] Post-validation flume configuration contains configuration for agents: [agent]
2018-12-30 22:14:51,215 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:147)] Creating channels
2018-12-30 22:14:51,221 (conf-file-poller-0) [INFO - org.apache.flume.channel.DefaultChannelFactory.create(DefaultChannelFactory.java:42)] Creating instance of channel memoryChannel type memory
2018-12-30 22:14:51,230 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:201)] Created channel memoryChannel
2018-12-30 22:14:51,231 (conf-file-poller-0) [INFO - org.apache.flume.source.DefaultSourceFactory.create(DefaultSourceFactory.java:41)] Creating instance of source netSrc, type netcat
2018-12-30 22:14:51,240 (conf-file-poller-0) [INFO - org.apache.flume.sink.DefaultSinkFactory.create(DefaultSinkFactory.java:42)] Creating instance of sink: loggerSink, type: logger
2018-12-30 22:14:51,242 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:116)] Channel memoryChannel connected to [netSrc, loggerSink]
2018-12-30 22:14:51,250 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:137)] Starting new configuration:{ sourceRunners:{netSrc=EventDrivenSourceRunner: { source:org.apache.flume.source.NetcatSource{name:netSrc,state:IDLE} }} sinkRunners:{loggerSink=SinkRunner: { policy:org.apache.flume.sink.DefaultSinkProcessor@722d34f1 counterGroup:{ name:null counters:{} } }} channels:{memoryChannel=org.apache.flume.channel.MemoryChannel{name: memoryChannel}} }
2018-12-30 22:14:51,261 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:144)] Starting Channel memoryChannel
2018-12-30 22:14:51,375 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.register(MonitoredCounterGroup.java:119)] Monitored counter group for type: CHANNEL, name: memoryChannel: Successfully registered new MBean.
2018-12-30 22:14:51,375 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.start(MonitoredCounterGroup.java:95)] Component type: CHANNEL, name: memoryChannel started
2018-12-30 22:14:51,377 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:171)] Starting Sink loggerSink
2018-12-30 22:14:51,382 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:182)] Starting Source netSrc
2018-12-30 22:14:51,382 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:155)] Source starting
2018-12-30 22:14:51,400 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:166)] Created serverSocket:sun.nio.ch.ServerSocketChannelImpl[/0:0:0:0:0:0:0:0:12345]
2018-12-30 22:15:47,100 (SinkRunner-PollingRunner-DefaultSinkProcessor) [INFO - org.apache.flume.sink.LoggerSink.process(LoggerSink.java:95)] Event: { headers:{} body: 68 65 6C 6C 6F 20 71 6C 79 0D                   hello qly. }
4:51,240 (conf-file-poller-0) [INFO - org.apache.flume.sink.DefaultSinkFactory.create(DefaultSinkFactory.java:42)] Creating instance of sink: loggerSink, type: logger
2018-12-30 22:14:51,242 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:116)] Channel memoryChannel connected to [netSrc, loggerSink]
2018-12-30 22:14:51,250 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:137)] Starting new configuration:{ sourceRunners:{netSrc=EventDrivenSourceRunner: { source:org.apache.flume.source.NetcatSource{name:netSrc,state:IDLE} }} sinkRunners:{loggerSink=SinkRunner: { policy:org.apache.flume.sink.DefaultSinkProcessor@722d34f1 counterGroup:{ name:null counters:{} } }} channels:{memoryChannel=org.apache.flume.channel.MemoryChannel{name: memoryChannel}} }
2018-12-30 22:14:51,261 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:144)] Starting Channel memoryChannel
2018-12-30 22:14:51,375 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.register(MonitoredCounterGroup.java:119)] Monitored counter group for type: CHANNEL, name: memoryChannel: Successfully registered new MBean.
2018-12-30 22:14:51,375 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.start(MonitoredCounterGroup.java:95)] Component type: CHANNEL, name: memoryChannel started
2018-12-30 22:14:51,377 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:171)] Starting Sink loggerSink
2018-12-30 22:14:51,382 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:182)] Starting Source netSrc
2018-12-30 22:14:51,382 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:155)] Source starting
2018-12-30 22:14:51,400 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:166)] Created serverSocket:sun.nio.ch.ServerSocketChannelImpl[/0:0:0:0:0:0:0:0:12345]
``` 

#### 报错信息
<li>flume未正常启动或防火墙设置
  
```  
[11899517@bigdata4 ~]$ telnet localhost 12345
Trying ::1...
Connected to localhost.
Escape character is '^]'.
hello qly
OK
```  

<li>启动命令有误导致启动失败
  
```  
//agent和-Dflume之间有空格
[11899517@bigdata4 flume]$ ./bin/flume-ng agent --conf conf --conf-file conf/flume-conf-netsrc.properties --name agent -Dflume.root.logger = INFO,console
Info: Sourcing environment configuration script /mnt/home/11899517/flume/conf/flume-env.sh
Info: Including Hadoop libraries found via (/home/hadoop/hadoop-current/bin/hadoop) for HDFS access
Info: Including Hive libraries found via () for Hive access
+ exec /mnt/home/11899517/soft/jdk1.8.0_11/bin/java -Xmx20m -cp '/mnt/home/11899517/flume/conf:/mnt/home/11899517/flume/lib/*:/home/hadoop/hadoop-2.7.6/etc/hadoop:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/common/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/*:/home/hadoop/hadoop-current/contrib/capacity-scheduler/*.jar:/lib/*' -Djava.library.path=:/home/hadoop/hadoop-2.7.6/lib/native org.apache.flume.node.Application --conf-file conf/flume-conf-netsrc.properties --name agent-Dflume.root.logger = INFO,console
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/mnt/home/11899517/flume/lib/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
```  
<li>fileSink配置有误,导致启动正常但是无法往目录文件路径写入数据
<li>Sink fileSink has been removed due to an error during configuration

```  
[11899517@bigdata4 flume]$ ./bin/flume-ng agent --conf conf --conf-file conf/flume-conf-netsrc2localhost.properties --name agent -Dflume.root.logger=INFO,console
Info: Sourcing environment configuration script /mnt/home/11899517/flume/conf/flume-env.sh
Info: Including Hadoop libraries found via (/home/hadoop/hadoop-current/bin/hadoop) for HDFS access
Info: Including Hive libraries found via () for Hive access
+ exec /mnt/home/11899517/soft/jdk1.8.0_11/bin/java -Xmx20m -Dflume.root.logger=INFO,console -cp '/mnt/home/11899517/flume/conf:/mnt/home/11899517/flume/lib/*:/home/hadoop/hadoop-2.7.6/etc/hadoop:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/common/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/hdfs/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/yarn/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/lib/*:/home/hadoop/hadoop-2.7.6/share/hadoop/mapreduce/*:/home/hadoop/hadoop-current/contrib/capacity-scheduler/*.jar:/lib/*' -Djava.library.path=:/home/hadoop/hadoop-2.7.6/lib/native org.apache.flume.node.Application --conf-file conf/flume-conf-netsrc2localhost.properties --name agent
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/mnt/home/11899517/flume/lib/slf4j-log4j12-1.6.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/home/hadoop/hadoop-2.7.6/share/hadoop/common/lib/slf4j-log4j12-1.7.10.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
2018-12-30 23:39:16,488 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider.start(PollingPropertiesFileConfigurationProvider.java:62)] Configuration provider starting
2018-12-30 23:39:16,494 (conf-file-poller-0) [INFO - org.apache.flume.node.PollingPropertiesFileConfigurationProvider$FileWatcherRunnable.run(PollingPropertiesFileConfigurationProvider.java:134)] Reloading configuration file:conf/flume-conf-netsrc2localhost.properties
2018-12-30 23:39:16,500 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:fileSink
2018-12-30 23:39:16,501 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:fileSink
2018-12-30 23:39:16,501 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:fileSink
2018-12-30 23:39:16,501 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:1016)] Processing:fileSink
2018-12-30 23:39:16,501 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration$AgentConfiguration.addProperty(FlumeConfiguration.java:930)] Added sinks: fileSink Agent: agent
2018-12-30 23:39:16,513 (conf-file-poller-0) [INFO - org.apache.flume.conf.FlumeConfiguration.validateConfiguration(FlumeConfiguration.java:140)] Post-validation flume configuration contains configuration for agents: [agent]
2018-12-30 23:39:16,513 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:147)] Creating channels
2018-12-30 23:39:16,521 (conf-file-poller-0) [INFO - org.apache.flume.channel.DefaultChannelFactory.create(DefaultChannelFactory.java:42)] Creating instance of channel memoryChannel type memory
2018-12-30 23:39:16,525 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.loadChannels(AbstractConfigurationProvider.java:201)] Created channel memoryChannel
2018-12-30 23:39:16,526 (conf-file-poller-0) [INFO - org.apache.flume.source.DefaultSourceFactory.create(DefaultSourceFactory.java:41)] Creating instance of source netSrc, type netcat
2018-12-30 23:39:16,535 (conf-file-poller-0) [INFO - org.apache.flume.sink.DefaultSinkFactory.create(DefaultSinkFactory.java:42)] Creating instance of sink: fileSink, type: file_roll
2018-12-30 23:39:16,540 (conf-file-poller-0) [ERROR - org.apache.flume.node.AbstractConfigurationProvider.loadSinks(AbstractConfigurationProvider.java:426)] Sink fileSink has been removed due to an error during configuration
java.lang.IllegalArgumentException: Directory may not be null
	at com.google.common.base.Preconditions.checkArgument(Preconditions.java:88)
	at org.apache.flume.sink.RollingFileSink.configure(RollingFileSink.java:90)
	at org.apache.flume.conf.Configurables.configure(Configurables.java:41)
	at org.apache.flume.node.AbstractConfigurationProvider.loadSinks(AbstractConfigurationProvider.java:411)
	at org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:102)
	at org.apache.flume.node.PollingPropertiesFileConfigurationProvider$FileWatcherRunnable.run(PollingPropertiesFileConfigurationProvider.java:141)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
2018-12-30 23:39:16,542 (conf-file-poller-0) [INFO - org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:116)] Channel memoryChannel connected to [netSrc]
2018-12-30 23:39:16,549 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:137)] Starting new configuration:{ sourceRunners:{netSrc=EventDrivenSourceRunner: { source:org.apache.flume.source.NetcatSource{name:netSrc,state:IDLE} }} sinkRunners:{} channels:{memoryChannel=org.apache.flume.channel.MemoryChannel{name: memoryChannel}} }
2018-12-30 23:39:16,559 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:144)] Starting Channel memoryChannel
2018-12-30 23:39:16,613 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.register(MonitoredCounterGroup.java:119)] Monitored counter group for type: CHANNEL, name: memoryChannel: Successfully registered new MBean.
2018-12-30 23:39:16,613 (lifecycleSupervisor-1-0) [INFO - org.apache.flume.instrumentation.MonitoredCounterGroup.start(MonitoredCounterGroup.java:95)] Component type: CHANNEL, name: memoryChannel started
2018-12-30 23:39:16,614 (conf-file-poller-0) [INFO - org.apache.flume.node.Application.startAllComponents(Application.java:182)] Starting Source netSrc
2018-12-30 23:39:16,614 (lifecycleSupervisor-1-1) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:155)] Source starting
2018-12-30 23:39:16,622 (lifecycleSupervisor-1-1) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:166)] Created serverSocket:sun.nio.ch.ServerSocketChannelImpl[/0:0:0:0:0:0:0:0:12345]
```  

<li>sink配置有误，核对

```  
2018-12-31 00:02:57,264 (conf-file-poller-0) [ERROR - org.apache.flume.node.AbstractConfigurationProvider.loadSinks(AbstractConfigurationProvider.java:426)] Sink fileSink has been removed due to an error during configuration
java.lang.IllegalArgumentException: Directory may not be null
	at com.google.common.base.Preconditions.checkArgument(Preconditions.java:88)
	at org.apache.flume.sink.RollingFileSink.configure(RollingFileSink.java:90)
	at org.apache.flume.conf.Configurables.configure(Configurables.java:41)
	at org.apache.flume.node.AbstractConfigurationProvider.loadSinks(AbstractConfigurationProvider.java:411)
	at org.apache.flume.node.AbstractConfigurationProvider.getConfiguration(AbstractConfigurationProvider.java:102)
	at org.apache.flume.node.PollingPropertiesFileConfigurationProvider$FileWatcherRunnable.run(PollingPropertiesFileConfigurationProvider.java:141)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
```

<li>同一个properties文件被重复启动报错
	
```  
2018-12-30 23:38:37,805 (lifecycleSupervisor-1-2) [INFO - org.apache.flume.source.NetcatSource.stop(NetcatSource.java:197)] Source stopping
2018-12-30 23:38:37,805 (lifecycleSupervisor-1-2) [ERROR - org.apache.flume.lifecycle.LifecycleSupervisor$MonitorRunnable.run(LifecycleSupervisor.java:251)] Unable to start EventDrivenSourceRunner: { source:org.apache.flume.source.NetcatSource{name:netSrc,state:STOP} } - Exception follows.
org.apache.flume.FlumeException: java.net.BindException: 地址已在使用
	at org.apache.flume.source.NetcatSource.start(NetcatSource.java:171)
	at org.apache.flume.source.EventDrivenSourceRunner.start(EventDrivenSourceRunner.java:44)
	at org.apache.flume.lifecycle.LifecycleSupervisor$MonitorRunnable.run(LifecycleSupervisor.java:249)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
Caused by: java.net.BindException: 地址已在使用
	at sun.nio.ch.Net.bind0(Native Method)
	at sun.nio.ch.Net.bind(Net.java:414)
	at sun.nio.ch.Net.bind(Net.java:406)
	at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:214)
	at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
	at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:67)
	at org.apache.flume.source.NetcatSource.start(NetcatSource.java:164)
	... 9 more
2018-12-30 23:38:40,806 (lifecycleSupervisor-1-3) [INFO - org.apache.flume.source.NetcatSource.start(NetcatSource.java:155)] Source starting
2018-12-30 23:38:40,806 (lifecycleSupervisor-1-3) [ERROR - org.apache.flume.source.NetcatSource.start(NetcatSource.java:169)] Unable to bind to socket. Exception follows.
java.net.BindException: 地址已在使用
	at sun.nio.ch.Net.bind0(Native Method)
	at sun.nio.ch.Net.bind(Net.java:414)
	at sun.nio.ch.Net.bind(Net.java:406)
	at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:214)
	at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
	at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:67)
	at org.apache.flume.source.NetcatSource.start(NetcatSource.java:164)
	at org.apache.flume.source.EventDrivenSourceRunner.start(EventDrivenSourceRunner.java:44)
	at org.apache.flume.lifecycle.LifecycleSupervisor$MonitorRunnable.run(LifecycleSupervisor.java:249)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
	at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1142)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
	at java.lang.Thread.run(Thread.java:745)
```  
