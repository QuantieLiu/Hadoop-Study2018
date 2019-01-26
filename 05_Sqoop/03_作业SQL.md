#### 前期准备

```
--配置HIVE_HOME环境变量
--便于 使用 hive 命令来进入hive命令行界面
export FLUME_HOME=/mnt/home/11899517/flume
export HIVE_HOME=/mnt/home/11899517/hive
export PATH=$FLUME_HOME/bin:$PATH:$HIVE_HOME/bin
```

```
--把hive的hive_site.xml复制到sqoop的conf
--添加hive仓库的路径参数配置
<property>  
  <name>hive.metastore.warehouse.dir</name>  
  <value>hdfs://bigdata0:33601/user/11899517/hive/warehouse</value>  
</property>  
```

```
--把hive/lib的一下jar包copy到sqoop/lib
-rw-r--r-- 1 11899517 11899517  290835 1月  24 13:21 hive-common-1.2.2.jar
-rw-r--r-- 1 11899517 11899517   32242 1月  24 13:21 hive-shims-0.20S-1.2.2.jar
-rw-r--r-- 1 11899517 11899517   59903 1月  24 13:21 hive-shims-0.23-1.2.2.jar
-rw-r--r-- 1 11899517 11899517    8966 1月  24 13:21 hive-shims-1.2.2.jar
-rw-r--r-- 1 11899517 11899517  109178 1月  24 13:21 hive-shims-common-1.2.2.jar
-rw-r--r-- 1 11899517 11899517   13084 1月  24 13:21 hive-shims-scheduler-1.2.2.jar
```

#### 把mysql的product表通过sqoop导入到hive中
<li>在sqoop的bin目录启动命令，若没有提前建库，则会在default中建表

```
[11899517@bigdata4 bin]$ ./sqoop import --connect jdbc:mysql://10.173.32.6:3306/sqoop --username root --password Gg/ru,.#5 --table product -m 1 --hive-import  --create-hive-table --hive-table  hiv_product
Warning: /mnt/home/11899517/sqoop/bin/../../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /mnt/home/11899517/sqoop/bin/../../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /mnt/home/11899517/sqoop/bin/../../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /mnt/home/11899517/sqoop/bin/../../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
19/01/26 10:29:59 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
19/01/26 10:29:59 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/01/26 10:29:59 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
19/01/26 10:29:59 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
19/01/26 10:29:59 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/01/26 10:29:59 INFO tool.CodeGenTool: Beginning code generation
Sat Jan 26 10:29:59 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/01/26 10:30:00 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `product` AS t LIMIT 1
19/01/26 10:30:00 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `product` AS t LIMIT 1
19/01/26 10:30:00 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /home/hadoop/hadoop-current
注: /tmp/sqoop-11899517/compile/d074ac18a06893c8ec6ded14efc90aa1/product.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
19/01/26 10:30:01 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-11899517/compile/d074ac18a06893c8ec6ded14efc90aa1/product.jar
19/01/26 10:30:01 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/01/26 10:30:01 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/01/26 10:30:01 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/01/26 10:30:01 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/01/26 10:30:01 INFO mapreduce.ImportJobBase: Beginning import of product
19/01/26 10:30:01 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/01/26 10:30:02 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/01/26 10:30:02 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
Sat Jan 26 10:30:04 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/01/26 10:30:04 INFO db.DBInputFormat: Using read commited transaction isolation
19/01/26 10:30:04 INFO mapreduce.JobSubmitter: number of splits:1
19/01/26 10:30:04 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1547603700477_1108
19/01/26 10:30:05 INFO impl.YarnClientImpl: Submitted application application_1547603700477_1108
19/01/26 10:30:05 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1547603700477_1108/
19/01/26 10:30:05 INFO mapreduce.Job: Running job: job_1547603700477_1108
19/01/26 10:30:12 INFO mapreduce.Job: Job job_1547603700477_1108 running in uber mode : false
19/01/26 10:30:12 INFO mapreduce.Job:  map 0% reduce 0%
19/01/26 10:30:18 INFO mapreduce.Job:  map 100% reduce 0%
19/01/26 10:30:18 INFO mapreduce.Job: Job job_1547603700477_1108 completed successfully
19/01/26 10:30:18 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=144695
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=2903
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=3586
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=3586
		Total vcore-milliseconds taken by all map tasks=3586
		Total megabyte-milliseconds taken by all map tasks=3672064
	Map-Reduce Framework
		Map input records=100
		Map output records=100
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=69
		CPU time spent (ms)=1970
		Physical memory (bytes) snapshot=175673344
		Virtual memory (bytes) snapshot=2163785728
		Total committed heap usage (bytes)=144179200
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=2903
19/01/26 10:30:18 INFO mapreduce.ImportJobBase: Transferred 2.835 KB in 16.4693 seconds (176.2675 bytes/sec)
19/01/26 10:30:18 INFO mapreduce.ImportJobBase: Retrieved 100 records.
19/01/26 10:30:18 INFO mapreduce.ImportJobBase: Publishing Hive/Hcat import job data to Listeners for table product
Sat Jan 26 10:30:18 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/01/26 10:30:18 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `product` AS t LIMIT 1
19/01/26 10:30:18 INFO hive.HiveImport: Loading uploaded data into Hive
19/01/26 10:30:20 INFO hive.HiveImport: 
19/01/26 10:30:20 INFO hive.HiveImport: Logging initialized using configuration in jar:file:/mnt/home/11899517/sqoop/lib/hive-common-1.2.2.jar!/hive-log4j.properties
19/01/26 10:30:26 INFO hive.HiveImport: OK
19/01/26 10:30:26 INFO hive.HiveImport: Time taken: 1.711 seconds
19/01/26 10:30:27 INFO hive.HiveImport: Loading data to table default.hiv_product
19/01/26 10:30:27 INFO hive.HiveImport: Table default.hiv_product stats: [numFiles=1, totalSize=2903]
19/01/26 10:30:27 INFO hive.HiveImport: OK
19/01/26 10:30:27 INFO hive.HiveImport: Time taken: 0.492 seconds
19/01/26 10:30:27 INFO hive.HiveImport: Hive import complete.
19/01/26 10:30:27 INFO hive.HiveImport: Export directory is contains the _SUCCESS file only, removing the directory.
[11899517@bigdata4 bin]$ 
```

<li>验证数据是否导入成功
	
```
hive> select * from hiv_product limit 5;
OK
103	1527235438751484	商品1
108	1527235438751468	商品2
28	1527235438751389	商品3
21	1527235438751248	商品4
25	1527235438751118	商品5
Time taken: 0.116 seconds, Fetched: 5 row(s)
hive> 
```

#### 查询表中有多少个课程，允许重复

```
hive> select count(*) from hiv_product;
Query ID = 11899517_20190126113509_74453119-903e-458f-bf34-f0a5462e59e7
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1547603700477_1122, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1547603700477_1122/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1547603700477_1122
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2019-01-26 11:35:16,525 Stage-1 map = 0%,  reduce = 0%
2019-01-26 11:35:21,712 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.62 sec
2019-01-26 11:35:27,964 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.74 sec
MapReduce Total cumulative CPU time: 3 seconds 740 msec
Ended Job = job_1547603700477_1122
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.74 sec   HDFS Read: 9934 HDFS Write: 4 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 740 msec
OK
100
Time taken: 19.74 seconds, Fetched: 1 row(s)
hive> select count(product_id) from hiv_product;
Query ID = 11899517_20190126113548_899ca988-9646-4917-b6c5-4aa9aa13832f
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1547603700477_1123, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1547603700477_1123/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1547603700477_1123
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2019-01-26 11:35:54,819 Stage-1 map = 0%,  reduce = 0%
2019-01-26 11:36:00,044 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.61 sec
2019-01-26 11:36:06,308 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.76 sec
MapReduce Total cumulative CPU time: 3 seconds 760 msec
Ended Job = job_1547603700477_1123
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.76 sec   HDFS Read: 10097 HDFS Write: 4 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 760 msec
OK
100
Time taken: 18.468 seconds, Fetched: 1 row(s)
hive> 
```



