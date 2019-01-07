
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

#### missing ) at 'as' near '<EOF>'

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
``` 


#### 

``` 

``` 
