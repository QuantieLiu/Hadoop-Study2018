



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
