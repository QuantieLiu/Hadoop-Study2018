<li>Output directory /sqoop already exists
删除相应目录即可 
hadoop fs -rmr  /sqoop
	
```
13/06/26 00:49:54 ERROR security.UserGroupInformation: PriviledgedActionException as:hadoop cause:org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory /data/ehm_hosts already exists
13/06/26 00:49:54 ERROR tool.ImportTool: Encountered IOException running import job: org.apache.hadoop.mapred.FileAlreadyExistsException: Output directory /data/ehm_hosts already exists
```

<li>Hive exited with status 1

```
[11899517@bigdata4 bin]$ ./sqoop import-all-tables --connect jdbc:mysql://10.173.32.6:3306/sqoop --username root --password Gg/ru,.#5 -m 1 --hive-import
Warning: /mnt/home/11899517/sqoop/bin/../../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /mnt/home/11899517/sqoop/bin/../../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /mnt/home/11899517/sqoop/bin/../../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /mnt/home/11899517/sqoop/bin/../../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
19/02/27 15:16:01 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
19/02/27 15:16:01 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/02/27 15:16:01 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
19/02/27 15:16:01 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
19/02/27 15:16:01 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
Wed Feb 27 15:16:01 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 15:16:02 INFO tool.CodeGenTool: Beginning code generation
19/02/27 15:16:02 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 15:16:02 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 15:16:02 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /home/hadoop/hadoop-current
注: /tmp/sqoop-11899517/compile/3272ebac37488d7d07547d9ea3b53c6a/analysis.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
19/02/27 15:16:04 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-11899517/compile/3272ebac37488d7d07547d9ea3b53c6a/analysis.jar
19/02/27 15:16:04 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/02/27 15:16:04 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/02/27 15:16:04 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/02/27 15:16:04 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/02/27 15:16:04 INFO mapreduce.ImportJobBase: Beginning import of analysis
19/02/27 15:16:04 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/02/27 15:16:04 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/02/27 15:16:05 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
Wed Feb 27 15:16:06 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 15:16:06 INFO db.DBInputFormat: Using read commited transaction isolation
19/02/27 15:16:06 INFO mapreduce.JobSubmitter: number of splits:1
19/02/27 15:16:07 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1550731843098_0815
19/02/27 15:16:07 INFO impl.YarnClientImpl: Submitted application application_1550731843098_0815
19/02/27 15:16:07 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1550731843098_0815/
19/02/27 15:16:07 INFO mapreduce.Job: Running job: job_1550731843098_0815
19/02/27 15:16:14 INFO mapreduce.Job: Job job_1550731843098_0815 running in uber mode : false
19/02/27 15:16:14 INFO mapreduce.Job:  map 0% reduce 0%
19/02/27 15:16:20 INFO mapreduce.Job:  map 100% reduce 0%
19/02/27 15:16:21 INFO mapreduce.Job: Job job_1550731843098_0815 completed successfully
19/02/27 15:16:21 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=144692
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=37989
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=4113
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=4113
		Total vcore-milliseconds taken by all map tasks=4113
		Total megabyte-milliseconds taken by all map tasks=4211712
	Map-Reduce Framework
		Map input records=1111
		Map output records=1111
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=62
		CPU time spent (ms)=2270
		Physical memory (bytes) snapshot=175230976
		Virtual memory (bytes) snapshot=2160984064
		Total committed heap usage (bytes)=146276352
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=37989
19/02/27 15:16:21 INFO mapreduce.ImportJobBase: Transferred 37.0986 KB in 16.7231 seconds (2.2184 KB/sec)
19/02/27 15:16:21 INFO mapreduce.ImportJobBase: Retrieved 1111 records.
19/02/27 15:16:21 INFO mapreduce.ImportJobBase: Publishing Hive/Hcat import job data to Listeners for table analysis
Wed Feb 27 15:16:21 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 15:16:21 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 15:16:21 WARN hive.TableDefWriter: Column times had to be cast to a less precise type in Hive
19/02/27 15:16:21 INFO hive.HiveImport: Loading uploaded data into Hive
19/02/27 15:16:23 INFO hive.HiveImport: 
19/02/27 15:16:23 INFO hive.HiveImport: Logging initialized using configuration in jar:file:/mnt/home/11899517/sqoop/lib/hive-common-1.2.2.jar!/hive-log4j.properties
19/02/27 15:16:31 INFO hive.HiveImport: FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:java.lang.IllegalArgumentException: java.net.URISyntaxException: Relative path in absolute URI: hdfs://bigdata0:50000./user/11899517/hive/warehouse)
19/02/27 15:16:31 ERROR tool.ImportAllTablesTool: Encountered IOException running import job: java.io.IOException: Hive exited with status 1
[11899517@bigdata4 bin]$ 
```

<li>Hive exited with status 88
<li>元数据冲突
也有人推荐分两步做
https://blog.csdn.net/wwg18895736195/article/details/84403610

```
[11899517@bigdata4 bin]$ ./sqoop import-all-tables --connect "jdbc:mysql://10.173.32.6:3306/sqoop?characterEncoding=UTF-8&useCursorFetch=true" --username root --password Gg/ru,.#5 -m 1 --hive-import --warehouse-dir /user/11899517/f_test/sqoop_all
Warning: /mnt/home/11899517/sqoop/bin/../../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /mnt/home/11899517/sqoop/bin/../../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /mnt/home/11899517/sqoop/bin/../../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /mnt/home/11899517/sqoop/bin/../../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
19/02/27 12:07:24 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
19/02/27 12:07:24 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/02/27 12:07:24 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
19/02/27 12:07:24 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
19/02/27 12:07:24 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
Wed Feb 27 12:07:24 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 12:07:25 INFO tool.CodeGenTool: Beginning code generation
19/02/27 12:07:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 12:07:25 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 12:07:25 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /home/hadoop/hadoop-current
注: /tmp/sqoop-11899517/compile/72716d0a1bf5559f98ed943079780a56/analysis.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
19/02/27 12:07:26 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-11899517/compile/72716d0a1bf5559f98ed943079780a56/analysis.jar
19/02/27 12:07:26 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/02/27 12:07:26 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/02/27 12:07:26 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/02/27 12:07:26 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/02/27 12:07:26 INFO mapreduce.ImportJobBase: Beginning import of analysis
19/02/27 12:07:26 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/02/27 12:07:27 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/02/27 12:07:27 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
Wed Feb 27 12:07:29 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 12:07:29 INFO db.DBInputFormat: Using read commited transaction isolation
19/02/27 12:07:29 INFO mapreduce.JobSubmitter: number of splits:1
19/02/27 12:07:29 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1550731843098_0787
19/02/27 12:07:30 INFO impl.YarnClientImpl: Submitted application application_1550731843098_0787
19/02/27 12:07:30 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1550731843098_0787/
19/02/27 12:07:30 INFO mapreduce.Job: Running job: job_1550731843098_0787
19/02/27 12:07:37 INFO mapreduce.Job: Job job_1550731843098_0787 running in uber mode : false
19/02/27 12:07:37 INFO mapreduce.Job:  map 0% reduce 0%
19/02/27 12:07:43 INFO mapreduce.Job:  map 100% reduce 0%
19/02/27 12:07:43 INFO mapreduce.Job: Job job_1550731843098_0787 completed successfully
19/02/27 12:07:43 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=144761
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=37989
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=4067
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=4067
		Total vcore-milliseconds taken by all map tasks=4067
		Total megabyte-milliseconds taken by all map tasks=4164608
	Map-Reduce Framework
		Map input records=1111
		Map output records=1111
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=65
		CPU time spent (ms)=2110
		Physical memory (bytes) snapshot=175374336
		Virtual memory (bytes) snapshot=2162507776
		Total committed heap usage (bytes)=145752064
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=37989
19/02/27 12:07:43 INFO mapreduce.ImportJobBase: Transferred 37.0986 KB in 15.7465 seconds (2.356 KB/sec)
19/02/27 12:07:43 INFO mapreduce.ImportJobBase: Retrieved 1111 records.
19/02/27 12:07:43 INFO mapreduce.ImportJobBase: Publishing Hive/Hcat import job data to Listeners for table analysis
Wed Feb 27 12:07:43 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 12:07:43 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 12:07:43 WARN hive.TableDefWriter: Column times had to be cast to a less precise type in Hive
19/02/27 12:07:43 INFO hive.HiveImport: Loading uploaded data into Hive
19/02/27 12:07:45 INFO hive.HiveImport: 
19/02/27 12:07:45 INFO hive.HiveImport: Logging initialized using configuration in jar:file:/mnt/home/11899517/sqoop/lib/hive-common-1.2.2.jar!/hive-log4j.properties
19/02/27 12:07:53 INFO hive.HiveImport: OK
19/02/27 12:07:53 INFO hive.HiveImport: Time taken: 2.0 seconds
19/02/27 12:07:53 INFO hive.HiveImport: Loading data to table default.analysis
19/02/27 12:07:53 INFO hive.HiveImport: Table default.analysis stats: [numFiles=1, totalSize=37989]
19/02/27 12:07:53 INFO hive.HiveImport: OK
19/02/27 12:07:53 INFO hive.HiveImport: Time taken: 0.575 seconds
19/02/27 12:07:54 INFO hive.HiveImport: Hive import complete.
19/02/27 12:07:54 INFO hive.HiveImport: Export directory is contains the _SUCCESS file only, removing the directory.
19/02/27 12:07:54 INFO tool.CodeGenTool: Beginning code generation
19/02/27 12:07:54 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `azkaban_1019435825.productcount` AS t LIMIT 1
19/02/27 12:07:54 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /home/hadoop/hadoop-current
注: /tmp/sqoop-11899517/compile/72716d0a1bf5559f98ed943079780a56/azkaban_1019435825_productcount.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
19/02/27 12:07:54 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-11899517/compile/72716d0a1bf5559f98ed943079780a56/azkaban_1019435825_productcount.jar
19/02/27 12:07:54 INFO mapreduce.ImportJobBase: Beginning import of azkaban_1019435825.productcount
19/02/27 12:07:54 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
Wed Feb 27 12:07:55 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 12:07:56 INFO db.DBInputFormat: Using read commited transaction isolation
19/02/27 12:07:56 INFO mapreduce.JobSubmitter: number of splits:1
19/02/27 12:07:56 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1550731843098_0788
19/02/27 12:07:56 INFO impl.YarnClientImpl: Submitted application application_1550731843098_0788
19/02/27 12:07:56 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1550731843098_0788/
19/02/27 12:07:56 INFO mapreduce.Job: Running job: job_1550731843098_0788
19/02/27 12:08:03 INFO mapreduce.Job: Job job_1550731843098_0788 running in uber mode : false
19/02/27 12:08:03 INFO mapreduce.Job:  map 0% reduce 0%
19/02/27 12:08:09 INFO mapreduce.Job:  map 100% reduce 0%
19/02/27 12:08:09 INFO mapreduce.Job: Job job_1550731843098_0788 completed successfully
19/02/27 12:08:09 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=221464
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=87
		HDFS: Number of bytes written=0
		HDFS: Number of read operations=4
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=3942
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=3942
		Total vcore-milliseconds taken by all map tasks=3942
		Total megabyte-milliseconds taken by all map tasks=4036608
	Map-Reduce Framework
		Map input records=0
		Map output records=0
		Input split bytes=87
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=61
		CPU time spent (ms)=1600
		Physical memory (bytes) snapshot=171511808
		Virtual memory (bytes) snapshot=2159673344
		Total committed heap usage (bytes)=148373504
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
19/02/27 12:08:09 INFO mapreduce.ImportJobBase: Transferred 0 bytes in 14.5988 seconds (0 bytes/sec)
19/02/27 12:08:09 INFO mapreduce.ImportJobBase: Retrieved 0 records.
19/02/27 12:08:09 INFO mapreduce.ImportJobBase: Publishing Hive/Hcat import job data to Listeners for table azkaban_1019435825.productcount
Wed Feb 27 12:08:09 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 12:08:09 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `azkaban_1019435825.productcount` AS t LIMIT 1
19/02/27 12:08:09 WARN hive.TableDefWriter: Column day had to be cast to a less precise type in Hive
19/02/27 12:08:09 INFO hive.HiveImport: Loading uploaded data into Hive
19/02/27 12:08:11 INFO hive.HiveImport: 
19/02/27 12:08:11 INFO hive.HiveImport: Logging initialized using configuration in jar:file:/mnt/home/11899517/sqoop/lib/hive-common-1.2.2.jar!/hive-log4j.properties
19/02/27 12:08:18 INFO hive.HiveImport: FAILED: SemanticException [Error 10072]: Database does not exist: azkaban_1019435825
19/02/27 12:08:18 ERROR tool.ImportAllTablesTool: Encountered IOException running import job: java.io.IOException: Hive exited with status 88
[11899517@bigdata4 bin]$ 
```

<li>mysql无响应

```
[11899517@bigdata4 bin]$ ./sqoop import-all-tables --connect jdbc:mysql://localhost:3306/sqoop --username root --password Gg/ru,.#5 -m 1 --hive-import --warehouse-dir /user/11899517/f_test/sqoop_all
Warning: /mnt/home/11899517/sqoop/bin/../../hbase does not exist! HBase imports will fail.
Please set $HBASE_HOME to the root of your HBase installation.
Warning: /mnt/home/11899517/sqoop/bin/../../hcatalog does not exist! HCatalog jobs will fail.
Please set $HCAT_HOME to the root of your HCatalog installation.
Warning: /mnt/home/11899517/sqoop/bin/../../accumulo does not exist! Accumulo imports will fail.
Please set $ACCUMULO_HOME to the root of your Accumulo installation.
Warning: /mnt/home/11899517/sqoop/bin/../../zookeeper does not exist! Accumulo imports will fail.
Please set $ZOOKEEPER_HOME to the root of your Zookeeper installation.
19/02/27 11:42:26 INFO sqoop.Sqoop: Running Sqoop version: 1.4.7
19/02/27 11:42:26 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/02/27 11:42:26 INFO tool.BaseSqoopTool: Using Hive-specific delimiters for output. You can override
19/02/27 11:42:26 INFO tool.BaseSqoopTool: delimiters with --fields-terminated-by, etc.
19/02/27 11:42:26 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
Wed Feb 27 11:42:27 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 11:42:27 INFO tool.CodeGenTool: Beginning code generation
19/02/27 11:42:27 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 11:42:27 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `analysis` AS t LIMIT 1
19/02/27 11:42:27 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /home/hadoop/hadoop-current
注: /tmp/sqoop-11899517/compile/993cc57d86b76b4d928ef9f11219f131/analysis.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
19/02/27 11:42:30 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-11899517/compile/993cc57d86b76b4d928ef9f11219f131/analysis.jar
19/02/27 11:42:30 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/02/27 11:42:30 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/02/27 11:42:30 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/02/27 11:42:30 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/02/27 11:42:30 INFO mapreduce.ImportJobBase: Beginning import of analysis
19/02/27 11:42:30 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/02/27 11:42:31 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/02/27 11:42:31 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
Wed Feb 27 11:42:33 CST 2019 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
19/02/27 11:42:33 INFO db.DBInputFormat: Using read commited transaction isolation
19/02/27 11:42:33 INFO mapreduce.JobSubmitter: number of splits:1
19/02/27 11:42:34 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1550731843098_0783
19/02/27 11:42:34 INFO impl.YarnClientImpl: Submitted application application_1550731843098_0783
19/02/27 11:42:34 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1550731843098_0783/
19/02/27 11:42:34 INFO mapreduce.Job: Running job: job_1550731843098_0783
19/02/27 11:42:41 INFO mapreduce.Job: Job job_1550731843098_0783 running in uber mode : false
19/02/27 11:42:41 INFO mapreduce.Job:  map 0% reduce 0%
19/02/27 11:42:45 INFO mapreduce.Job: Task Id : attempt_1550731843098_0783_m_000000_0, Status : FAILED
Error: java.lang.RuntimeException: java.lang.RuntimeException: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
	at org.apache.sqoop.mapreduce.db.DBInputFormat.setDbConf(DBInputFormat.java:170)
	at org.apache.sqoop.mapreduce.db.DBInputFormat.setConf(DBInputFormat.java:161)
	at org.apache.hadoop.util.ReflectionUtils.setConf(ReflectionUtils.java:76)
	at org.apache.hadoop.util.ReflectionUtils.newInstance(ReflectionUtils.java:136)
	at org.apache.hadoop.mapred.MapTask.runNewMapper(MapTask.java:751)
	at org.apache.hadoop.mapred.MapTask.run(MapTask.java:341)
	at org.apache.hadoop.mapred.YarnChild$2.run(YarnChild.java:164)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:422)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1758)
	at org.apache.hadoop.mapred.YarnChild.main(YarnChild.java:158)
Caused by: java.lang.RuntimeException: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
	at org.apache.sqoop.mapreduce.db.DBInputFormat.getConnection(DBInputFormat.java:223)
	at org.apache.sqoop.mapreduce.db.DBInputFormat.setDbConf(DBInputFormat.java:168)
	... 10 more
Caused by: com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

The last packet sent successfully to the server was 0 milliseconds ago. The driver has not received any packets from the server.
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:425)
	at com.mysql.jdbc.SQLError.createCommunicationsException(SQLError.java:990)
	at com.mysql.jdbc.MysqlIO.<init>(MysqlIO.java:342)
	at com.mysql.jdbc.ConnectionImpl.coreConnect(ConnectionImpl.java:2188)
	at com.mysql.jdbc.ConnectionImpl.connectOneTryOnly(ConnectionImpl.java:2221)
	at com.mysql.jdbc.ConnectionImpl.createNewIO(ConnectionImpl.java:2016)
	at com.mysql.jdbc.ConnectionImpl.<init>(ConnectionImpl.java:776)
	at com.mysql.jdbc.JDBC4Connection.<init>(JDBC4Connection.java:47)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:425)
	at com.mysql.jdbc.ConnectionImpl.getInstance(ConnectionImpl.java:386)
	at com.mysql.jdbc.NonRegisteringDriver.connect(NonRegisteringDriver.java:330)
	at java.sql.DriverManager.getConnection(DriverManager.java:664)
	at java.sql.DriverManager.getConnection(DriverManager.java:247)
	at org.apache.sqoop.mapreduce.db.DBConfiguration.getConnection(DBConfiguration.java:302)
	at org.apache.sqoop.mapreduce.db.DBInputFormat.getConnection(DBInputFormat.java:216)
	... 11 more
Caused by: java.net.ConnectException: Connection refused (Connection refused)
	at java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:350)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:589)
	at com.mysql.jdbc.StandardSocketFactory.connect(StandardSocketFactory.java:211)
	at com.mysql.jdbc.MysqlIO.<init>(MysqlIO.java:301)
	... 27 more
```
