<li>hive版本过高

```
[11899517@bigdata4 bin]$ hive
Logging initialized using configuration in jar:file:/mnt/home/11899517/hive/lib/hive-common-1.2.2.jar!/hive-log4j.properties
Exception in thread "main" java.lang.RuntimeException: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient
	at org.apache.hadoop.hive.ql.session.SessionState.start(SessionState.java:522)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:677)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:621)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
Caused by: java.lang.RuntimeException: Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient
	at org.apache.hadoop.hive.metastore.MetaStoreUtils.newInstance(MetaStoreUtils.java:1523)
	at org.apache.hadoop.hive.metastore.RetryingMetaStoreClient.<init>(RetryingMetaStoreClient.java:86)
	at org.apache.hadoop.hive.metastore.RetryingMetaStoreClient.getProxy(RetryingMetaStoreClient.java:132)
	at org.apache.hadoop.hive.metastore.RetryingMetaStoreClient.getProxy(RetryingMetaStoreClient.java:104)
	at org.apache.hadoop.hive.ql.metadata.Hive.createMetaStoreClient(Hive.java:3005)
	at org.apache.hadoop.hive.ql.metadata.Hive.getMSC(Hive.java:3024)
	at org.apache.hadoop.hive.ql.session.SessionState.start(SessionState.java:503)
	... 8 more
Caused by: java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at org.apache.hadoop.hive.metastore.MetaStoreUtils.newInstance(MetaStoreUtils.java:1521)
	... 14 more
Caused by: javax.jdo.JDOFatalDataS
```

<li>关键字占用报错

```
--到hive-site.xml的hive.support.sql11.reserved.keywords改为false，默认true
  <property>
    <name>hive.support.sql11.reserved.keywords</name>
    <value>false</value>
    <description>
      This flag should be set to true to enable support for SQL2011 reserved keywords.
      The default value is true.
    </description>
  </property>
--更改配置文件后不一定能立刻生效，在hive窗口输入
SET hive.support.sql11.reserved.keywords=false; 即可
```

```
hive> desc order;
FailedPredicateException(identifier,{useSQL11ReservedKeywordsForIdentifier()}?)
	at org.apache.hadoop.hive.ql.parse.HiveParser_IdentifiersParser.identifier(HiveParser_IdentifiersParser.java:10924)
	at org.apache.hadoop.hive.ql.parse.HiveParser.identifier(HiveParser.java:45918)
	at org.apache.hadoop.hive.ql.parse.HiveParser.tabTypeExpr(HiveParser.java:15941)
	at org.apache.hadoop.hive.ql.parse.HiveParser.partTypeExpr(HiveParser.java:16279)
	at org.apache.hadoop.hive.ql.parse.HiveParser.descStatement(HiveParser.java:16818)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:2703)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:1653)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1112)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:202)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:166)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:397)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:309)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1145)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1193)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1082)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1072)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:213)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:165)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:376)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:736)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:681)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:621)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
FAILED: ParseException line 1:13 Failed to recognize predicate 'order'. Failed rule: 'identifier' in specifying table types
hive> 
```

#### truncate 不能删除外部表

```
hive> truncate table a_weblog;
FAILED: SemanticException [Error 10146]: Cannot truncate non-managed table a_weblog.
hive> 
```

#### 建库报错
原因：HADOOP_HOME用老师的，但是存放表数据的hdfs路径却使用了自己的
解决方案：把hive安装目录的conf的hive-site.xml的hive.metastore.warehouse.dir改为hdfs:///user/11899517/f_test/hive/warehouse，换到老师的hdfs配置即可

```
[11899517@bigdata4 hive]$ hive
Logging initialized using configuration in jar:file:/mnt/home/11899517/hive/lib/hive-common-1.2.2.jar!/hive-log4j.properties
hive> show databases;
OK
bigdata
default
Time taken: 1.231 seconds, Fetched: 2 row(s)
hive> create database bbt;
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: java.net.ConnectException Call From bigdata4.novalocal/127.0.0.1 to bigdata0:33601 failed on connection exception: java.net.ConnectException: 拒绝连接; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused)
hive> 
```

#### NoViableAltException

``` 
NoViableAltException(313@[192:1: tableName : (db= identifier DOT tab= identifier -> ^( TOK_TABNAME $db $tab) |tab= identifier -> ^( TOK_TABNAME $tab) );])
	at org.antlr.runtime.DFA.noViableAlt(DFA.java:158)
	at org.antlr.runtime.DFA.predict(DFA.java:144)
	at org.apache.hadoop.hive.ql.parse.HiveParser_FromClauseParser.tableName(HiveParser_FromClauseParser.java:4747)
	at org.apache.hadoop.hive.ql.parse.HiveParser.tableName(HiveParser.java:45945)
	at org.apache.hadoop.hive.ql.parse.HiveParser.createTableStatement(HiveParser.java:5032)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:2643)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:1653)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1112)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:202)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:166)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:397)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:309)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1145)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1193)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1082)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1072)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:213)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:165)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:376)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:736)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:681)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:621)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
FAILED: ParseException line 1:36 cannot recognize input near ''bigdata.weblog'' '(' ''time_tag'' in table name
``` 


#### 权限问题只能建bigdata数据库

``` 
hive> create database if not exists bigdataqly;
FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: org.apache.hadoop.security.AccessControlException Permission denied: user=11899517, access=WRITE, inode="/user/hive/warehouse":hadoop:supergroup:drwxr-xr-x
	at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:308)
	at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:214)
	at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:190)
	at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1752)
	at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1736)
	at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkAncestorAccess(FSDirectory.java:1719)
	at org.apache.hadoop.hdfs.server.namenode.FSDirMkdirOp.mkdirs(FSDirMkdirOp.java:69)
	at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:3872)
	at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:984)
	at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:634)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:616)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2217)
	at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2213)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:422)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1758)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2213)
)
``` 
#### 注意 ` 和 ' 的使用, 数据库名/表名/分区名用`,中文/包路径/数据路径用'

``` 
hive> CREATE EXTERNAL TABLE IF NOT EXISTS 'bigdata.weblog1' ( 'active_name' string COMMNENT '事件名称','order_id' string COMMNENT '订单id' )
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.oapach.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/hadoop/weblog/weblog';
NoViableAltException(313@[192:1: tableName : (db= identifier DOT tab= identifier -> ^( TOK_TABNAME $db $tab) |tab= identifier -> ^( TOK_TABNAME $tab) );])
	at org.antlr.runtime.DFA.noViableAlt(DFA.java:158)
	at org.antlr.runtime.DFA.predict(DFA.java:144)
	at org.apache.hadoop.hive.ql.parse.HiveParser_FromClauseParser.tableName(HiveParser_FromClauseParser.java:4747)
	at org.apache.hadoop.hive.ql.parse.HiveParser.tableName(HiveParser.java:45945)
	at org.apache.hadoop.hive.ql.parse.HiveParser.createTableStatement(HiveParser.java:5032)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:2643)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:1653)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1112)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:202)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:166)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:397)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:309)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1145)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1193)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1082)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1072)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:213)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:165)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:376)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:736)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:681)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:621)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
FAILED: ParseException line 1:36 cannot recognize input near ''bigdata.weblog1'' '(' ''active_name'' in table name
``` 

#### OUTPUTFORMAT路径有误

``` 
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.weblog1` ( 
    > `active_name` string COMMENT '事件名称',
    > `order_id` string COMMENT '订单id' )
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.oapach.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/weblog/weblog';
FAILED: SemanticException Cannot find class 'org.oapach.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
``` 

#### missing ) at 'as' near '<EOF>'------)数量不对
<li> map('key','register_day','value',from_unixtime(cast(register_time / 1000) as bigint),'yyyy-MM-dd'))处的register_time / 1000 后面直接as 不需要 ）

``` 
hive> SELECT user_id,mp['key'],mp['value'] FROM
    > (SELECT
    > user_id,array(map('key','gender','value',gender),
    > map('key','age','value',cast(2018-form_unixtime(cast(birthday/1000 as bigint),'yyyy') as string)),
    > map('key','device_type','value',device_type),
    > map('key','register_day','value',from_unixtime(cast(register_time / 1000) as bigint),'yyyy-MM-dd'))
    > as arr 
    > FROM bigdata.menber) s lateral view explode(arr) arrtable as mp;
FAILED: ParseException line 6:72 extraneous input ')' expecting AS near '<EOF>'
line 7:0 missing ) at 'as' near '<EOF>'
hive> SELECT user_id,mp['key'],mp['value'] FROM
    > (SELECT user_id,array(map('key','gender','value',gender),
    > map('key','age','value',cast(2018-form_unixtime(cast(birthday/1000 as bigint),'yyyy') as string)),
    > map('key','device_type','value',device_type),
    > map('key','register_day','value',from_unixtime(cast(register_time / 1000) as bigint),'yyyy-MM-dd')) as arr 
    > FROM bigdata.menber) s lateral view explode(arr) arrtable as mp;
FAILED: ParseException line 5:72 extraneous input ')' expecting AS near '<EOF>'
line 5:100 missing ) at 'as' near '<EOF>'

hive> SELECT user_id,mp['key'],mp['value'] FROM
    > (SELECT 
    > user_id,
    > array(map('key','gender','value',gender),
    > map('key','age','value',cast(2018-form_unixtime(cast(birthday/1000 as bigint),'yyyy') as string)),
    > map('key','device_type','value',device_type),
    > map('key','register_day','value',from_unixtime(cast(register_time / 1000) as bigint),'yyyy-MM-dd'))
    > as arr 
    > FROM bigdata.menber ) s lateral view explode(arr) arrtable as mp;
FAILED: ParseException line 7:72 extraneous input ')' expecting AS near '<EOF>'
line 8:0 missing ) at 'as' near '<EOF>'
hive> INSERT overwrite table bigdata.user_tag_value partition(module = 'basic_info')
    > SELECT user_id,mp['key'],mp['value'] FROM
    > (SELECT 
    > user_id,
    > array(map('key','gender','value',gender),
    > map('key','age','value',cast(2018-form_unixtime(cast(birthday/1000 as bigint),'yyyy') as string)),
    > map('key','device_type','value',device_type),
    > map('key','register_day','value',from_unixtime(cast(register_time / 1000) as bigint),'yyyy-MM-dd'))
    > )as arr 
    > FROM bigdata.menber ) s lateral view explode(arr) arrtable as mp;
FAILED: ParseException line 8:72 extraneous input ')' expecting AS near '<EOF>'
``` 


#### Invalid table alias or column reference 'key':

``` 
hive> SELECT user_id,mp[`key`],mp[`value`]
    > FROM
    > (SELECT user_id,
    > array(map('key','first_order_time','value',min(order_time)),
    > map('key','last_order_time','value',max(order_time)),
    > map('key','order_count','value',count(1)),
    > map('key','order_sum','value',sum(pay_amount)))
    > as arr 
    > FROM
    > bigdata.orders
    > GROUP BY
    > user_id
    > ) s lateral view explode(arr) arrtable as mp;
FAILED: SemanticException [Error 10004]: Line 1:18 Invalid table alias or column reference 'key': (possible column names are: s.user_id, s.arr, arrtable.mp)
``` 
