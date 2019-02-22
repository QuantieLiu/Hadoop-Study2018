### 前期准备

```

```

## cmd01为例

### job文件
<li>新建空白文档，以.job后缀，内容如下
<li>window系统压缩,只能是zip格式

```
# cmd01.job
# distinctUserId
type=command
command=/home/hadoop/hadoop-current/bin/hadoop jar /mnt/home/11899517/had1205-0222.jar com.bigdata.etl.job.ParseLogJob /user/11899517/input /user/11899517/azkoutput
```

### 把MR任务的jar包复制到集群

```
qly@qlyComputer:~$ sudo scp -i /home/qly/.ssh/11899517 /home/qly/workspace/had05/had1205/target/had1205-0222.jar 11899517@10.173.32.6:/mnt/home/11899517/
had1205-0222.jar                              100%  431KB 430.7KB/s   00:00    
qly@qlyComputer:~$ ssh 11899517@10.173.32.6 -i /home/qly/.ssh/11899517
Last login: Fri Feb 22 14:11:21 2019 from 10.173.16.14
[11899517@bigdata4 ~]$ ls
000000_0          hive
17monipdb.dat     hivefile
all_tar           indata.txt
azkaban           json-serde-1.3.6-jar-with-dependencies.jar
azkaban-exec      log0605-output
azkaban-plugins   log0605.txt
azkaban-web       metastore_db
derby.log         mysql-connector-java-5.1.46.jar
flume             mysql-connector-java-5.1.47.jar
flumefile         part00000
flume_log         soft
had1205-0222.jar  sqoop
[11899517@bigdata4 ~]$ 
```

### 登陆网页，上传job文件
<li>实操参考链接：

```
--以command形式操作MapReduce任务，附带zip目录的结构（推荐）
https://www.cnblogs.com/qingyunzong/p/8810610.html
--Azkaban各种类型的Job编写
https://www.jianshu.com/p/f2310a5c38c6
--作业参数使用介绍
http://www.mamicode.com/info-detail-2106795.html
```

### execute

```
[11899517@bigdata4 ~]$ hadoop fs -ls /user/11899517
Found 10 items
drwxr-x---   - 11899517 supergroup          0 2019-02-22 15:08 /user/11899517/azkoutput
drwxr-x---   - 11899517 supergroup          0 2018-12-31 00:59 /user/11899517/flume_data
drwxr-x---   - 11899517 supergroup          0 2019-01-26 10:30 /user/11899517/hive
drwxr-x---   - 11899517 supergroup          0 2019-01-06 16:46 /user/11899517/hivetable
drwxr-x---   - 11899517 supergroup          0 2018-12-11 12:57 /user/11899517/input
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:29 /user/11899517/mrlogs
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:15 /user/11899517/output
drwxr-x---   - 11899517 supergroup          0 2018-12-16 18:03 /user/11899517/resource
drwxr-x---   - 11899517 supergroup          0 2019-01-25 23:02 /user/11899517/sqoop_test
drwxr-x---   - 11899517 supergroup          0 2019-01-06 20:31 /user/11899517/weblog
[11899517@bigdata4 ~]$ hadoop fs -ls /user/11899517/azkoutput
Found 2 items
-rw-r-----   3 11899517 supergroup          0 2019-02-22 15:08 /user/11899517/azkoutput/_SUCCESS
-rw-r-----   3 11899517 supergroup  158180009 2019-02-22 15:08 /user/11899517/azkoutput/part00000
[11899517@bigdata4 ~]$ hadoop fs -copyToLocal /user/11899517/azkoutput ~
[11899517@bigdata4 ~]$ ls
000000_0       azkaban-plugins  flumefile         indata.txt                                  mysql-connector-java-5.1.46.jar
17monipdb.dat  azkaban-web      flume_log         json-serde-1.3.6-jar-with-dependencies.jar  mysql-connector-java-5.1.47.jar
all_tar        azkoutput        had1205-0222.jar  log0605-output                              part00000
azkaban        derby.log        hive              log0605.txt                                 soft
azkaban-exec   flume            hivefile          metastore_db          
[11899517@bigdata4 ~]$ tail -100f part00000
{"user_id":"9891528166064966","action_path":["http://www.bigdataclass.com/my/9891528166064966","http://www.bigdataclass.com/product/1527235438748929","http://www.bigdataclass.com/my/9891528166064966","http://www.bigdataclass.com/my/9891528166064966","http://www.bigdataclass.com/my/9891528166064966","http://www.bigdataclass.com","http://www.bigdataclass.com/product/1527235438749743","http://www.bigdataclass.com/category","http://www.bigdataclass.com","http://www.bigdataclass.com/category","http://www.bigdataclass.com/category","http://www.bigdataclass.com/category","http://www.bigdataclass.com/my/9891528166064966","http://www.bigdataclass.com/category","http://www.bigdataclass.com/product/1527235438749476","order","pay"]}
{"user_id":"9891528166071869","action_path":["http://www.bigdataclass.com","http://www.bigdataclass.com/my/9891528166071869","http://www.bigdataclass.com/product/1527235438750625","http://www.bigdataclass.com/product/1527235438748767","http://www.bigdataclass.com","http://www.bigdataclass.com/product/1527235438747497","http://www.bigdataclass.com/category","http://www.bigdataclass.com/product/1527235438746838","http://www.bigdataclass.com/my/9891528166071869","http://www.bigdataclass.com/product/1527235438750851","http://www.bigdataclass.com/product/1527235438751468","http://www.bigdataclass.com/my/9891528166071869","http://www.bigdataclass.com/product/1527235438750851","http://www.bigdataclass.com/product/1527235438749509","http://www.bigdataclass.com/product/1527235438749547","http://www.bigdataclass.com/my/9891528166071869","http://www.bigdataclass.com/category","http://www.bigdataclass.com/product/1527235438750543","order","pay"]}
省略很多行输出
[11899517@bigdata4 ~]$ 
```

## cmd02为例

### job文件
<li>新建空白文档，以.job后缀，内容如下

```
# cmd02.job
# distinctUserId02
type=command
command=/home/hadoop/hadoop-current/bin/hadoop jar /mnt/home/11899517/had1205-distincUser.jar com.bigdata.etl.job.DistinctUserIdJob /user/11899517/input  /user/11899517/azkoutput02
```

<li>注意maven打包时，要把非执行的job类的main方法注释
  
```
public static void main(String[] args) throws Exception {
		int res=ToolRunner.run(new Configuration(), new ParseLogJob(),args);
		System.exit(res);
	}
```


### execute

```
[11899517@bigdata4 ~]$ hadoop fs -ls /user/11899517
Found 11 items
drwxr-x---   - 11899517 supergroup          0 2019-02-22 15:52 /user/11899517/azkinput
drwxr-x---   - 11899517 supergroup          0 2019-02-22 15:08 /user/11899517/azkoutput
drwxr-x---   - 11899517 supergroup          0 2018-12-31 00:59 /user/11899517/flume_data
drwxr-x---   - 11899517 supergroup          0 2019-01-26 10:30 /user/11899517/hive
drwxr-x---   - 11899517 supergroup          0 2019-01-06 16:46 /user/11899517/hivetable
drwxr-x---   - 11899517 supergroup          0 2018-12-11 12:57 /user/11899517/input
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:29 /user/11899517/mrlogs
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:15 /user/11899517/output
drwxr-x---   - 11899517 supergroup          0 2018-12-16 18:03 /user/11899517/resource
drwxr-x---   - 11899517 supergroup          0 2019-01-25 23:02 /user/11899517/sqoop_test
drwxr-x---   - 11899517 supergroup          0 2019-01-06 20:31 /user/11899517/weblog
[11899517@bigdata4 ~]$ hadoop fs -ls /user/11899517
Found 12 items
drwxr-x---   - 11899517 supergroup          0 2019-02-22 15:52 /user/11899517/azkinput
drwxr-x---   - 11899517 supergroup          0 2019-02-22 15:08 /user/11899517/azkoutput
drwxr-x---   - 11899517 supergroup          0 2019-02-22 16:24 /user/11899517/azkoutput02
drwxr-x---   - 11899517 supergroup          0 2018-12-31 00:59 /user/11899517/flume_data
drwxr-x---   - 11899517 supergroup          0 2019-01-26 10:30 /user/11899517/hive
drwxr-x---   - 11899517 supergroup          0 2019-01-06 16:46 /user/11899517/hivetable
drwxr-x---   - 11899517 supergroup          0 2018-12-11 12:57 /user/11899517/input
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:29 /user/11899517/mrlogs
drwxr-x---   - 11899517 supergroup          0 2019-01-08 13:15 /user/11899517/output
drwxr-x---   - 11899517 supergroup          0 2018-12-16 18:03 /user/11899517/resource
drwxr-x---   - 11899517 supergroup          0 2019-01-25 23:02 /user/11899517/sqoop_test
drwxr-x---   - 11899517 supergroup          0 2019-01-06 20:31 /user/11899517/weblog
[11899517@bigdata4 ~]$ rm -rf azkoutput02
[11899517@bigdata4 ~]$ ls
000000_0         azkaban-web  had1205-0222.jar                            log0605-output                   part00000
17monipdb.dat    azkoutput    had1205-distincUser.jar                     log0605.txt                      soft
all_tar          derby.log    hive                                        metastore_db                     sqoop
azkaban          flume        hivefile                                    mysql-connector-java-5.1.46.jar
azkaban-exec     flumefile    indata.txt                                  mysql-connector-java-5.1.47.jar
azkaban-plugins  flume_log    json-serde-1.3.6-jar-with-dependencies.jar  output
[11899517@bigdata4 ~]$ hadoop fs -copyToLocal /user/11899517/azkoutput02 ~
[11899517@bigdata4 ~]$ ls
000000_0         azkaban-web  flume_log                json-serde-1.3.6-jar-with-dependencies.jar  output
17monipdb.dat    azkoutput    had1205-0222.jar         log0605-output                              part00000
all_tar          azkoutput02  had1205-distincUser.jar  log0605.txt                                 soft
azkaban          derby.log    hive                     metastore_db                                sqoop
azkaban-exec     flume        hivefile                 mysql-connector-java-5.1.46.jar
azkaban-plugins  flumefile    indata.txt               mysql-connector-java-5.1.47.jar
[11899517@bigdata4 ~]$ cd azkoutput02
[11899517@bigdata4 azkoutput02]$ ls
part00000  _SUCCESS
[11899517@bigdata4 azkoutput02]$ tail -20f part00000
{"user_id":"9981528165836645","action_path":["http://www.bigdataclass.com","http://www.bigdataclass.com/category","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/category","http://www.bigdataclass.com","http://www.bigdataclass.com","http://www.bigdataclass.com/product/1527235438747966","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/category","http://www.bigdataclass.com","http://www.bigdataclass.com","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/product/1527235438751157","http://www.bigdataclass.com","http://www.bigdataclass.com/product/1527235438750963","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/product/1527235438748291","http://www.bigdataclass.com/my/9981528165836645","http://www.bigdataclass.com/my/9981528165836645"]}
省略很多行输出
[11899517@bigdata4 ~]$ 
```

<li>exec-web的Job Logs输出如下：
	
```
22-02-2019 16:24:24 CST cmd02 INFO - Starting job cmd02 at 1550823864414
22-02-2019 16:24:24 CST cmd02 INFO - azkaban.webserver.url property was not set
22-02-2019 16:24:24 CST cmd02 INFO - job JVM args: -Dazkaban.flowid=cmd02 -Dazkaban.execid=1647 -Dazkaban.jobid=cmd02
22-02-2019 16:24:24 CST cmd02 INFO - user.to.proxy property was not set, defaulting to submit user azkabanweb
22-02-2019 16:24:24 CST cmd02 INFO - Building command job executor. 
22-02-2019 16:24:24 CST cmd02 INFO - Memory granted for job cmd02
22-02-2019 16:24:24 CST cmd02 INFO - 1 commands to execute.
22-02-2019 16:24:24 CST cmd02 INFO - cwd=/mnt/home/11899517/azkaban-exec/executions/1647
22-02-2019 16:24:24 CST cmd02 INFO - effective user is: azkabanweb
22-02-2019 16:24:24 CST cmd02 INFO - Command: /home/hadoop/hadoop-current/bin/hadoop jar /mnt/home/11899517/had1205-distincUser.jar com.bigdata.etl.job.DistinctUserIdJob /user/11899517/input  /user/11899517/azkoutput02
22-02-2019 16:24:24 CST cmd02 INFO - Environment variables: {JOB_OUTPUT_PROP_FILE=/mnt/home/11899517/azkaban-exec/executions/1647/cmd02_output_453259436916266008_tmp, JOB_PROP_FILE=/mnt/home/11899517/azkaban-exec/executions/1647/cmd02_props_8638723723384121833_tmp, KRB5CCNAME=/tmp/krb5cc__cmd02fouth__cmd02__cmd02__1647__azkabanweb, JOB_NAME=cmd02}
22-02-2019 16:24:24 CST cmd02 INFO - Working directory: /mnt/home/11899517/azkaban-exec/executions/1647
22-02-2019 16:24:26 CST cmd02 INFO - 19/02/22 16:24:26 INFO client.RMProxy: Connecting to ResourceManager at bigdata0/10.173.32.5:8032
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO input.FileInputFormat: Total input paths to process : 1
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO lzo.GPLNativeCodeLoader: Loaded native gpl library from the embedded binaries
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO lzo.LzoCodec: Successfully loaded & initialized native-lzo library [hadoop-lzo rev 5e360bdefd8923280b1a0234b845448050bf0caa]
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO input.CombineFileInputFormat: DEBUG: Terminated node allocation with : CompletedNodes: 3, size left: 54625074
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO mapreduce.JobSubmitter: number of splits:1
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1550731843098_0020
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO impl.YarnClientImpl: Submitted application application_1550731843098_0020
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO mapreduce.Job: The url to track the job: http://bigdata0.novalocal:8088/proxy/application_1550731843098_0020/
22-02-2019 16:24:27 CST cmd02 INFO - 19/02/22 16:24:27 INFO mapreduce.Job: Running job: job_1550731843098_0020
22-02-2019 16:24:33 CST cmd02 INFO - 19/02/22 16:24:33 INFO mapreduce.Job: Job job_1550731843098_0020 running in uber mode : false
22-02-2019 16:24:33 CST cmd02 INFO - 19/02/22 16:24:33 INFO mapreduce.Job:  map 0% reduce 0%
22-02-2019 16:24:44 CST cmd02 INFO - 19/02/22 16:24:44 INFO mapreduce.Job:  map 67% reduce 0%
22-02-2019 16:24:45 CST cmd02 INFO - 19/02/22 16:24:45 INFO mapreduce.Job:  map 100% reduce 0%
22-02-2019 16:24:52 CST cmd02 INFO - 19/02/22 16:24:52 INFO mapreduce.Job:  map 100% reduce 100%
22-02-2019 16:24:52 CST cmd02 INFO - 19/02/22 16:24:52 INFO mapreduce.Job: Job job_1550731843098_0020 completed successfully
22-02-2019 16:24:52 CST cmd02 INFO - 19/02/22 16:24:52 INFO mapreduce.Job: Counters: 53
22-02-2019 16:24:52 CST cmd02 INFO - 	File System Counters
22-02-2019 16:24:52 CST cmd02 INFO - 		FILE: Number of bytes read=44455580
22-02-2019 16:24:52 CST cmd02 INFO - 		FILE: Number of bytes written=89161779
22-02-2019 16:24:52 CST cmd02 INFO - 		FILE: Number of read operations=0
22-02-2019 16:24:52 CST cmd02 INFO - 		FILE: Number of large read operations=0
22-02-2019 16:24:52 CST cmd02 INFO - 		FILE: Number of write operations=0
22-02-2019 16:24:52 CST cmd02 INFO - 		HDFS: Number of bytes read=54625219
22-02-2019 16:24:52 CST cmd02 INFO - 		HDFS: Number of bytes written=10153717
22-02-2019 16:24:52 CST cmd02 INFO - 		HDFS: Number of read operations=6
22-02-2019 16:24:52 CST cmd02 INFO - 		HDFS: Number of large read operations=0
22-02-2019 16:24:52 CST cmd02 INFO - 		HDFS: Number of write operations=2
22-02-2019 16:24:52 CST cmd02 INFO - 	Job Counters 
22-02-2019 16:24:52 CST cmd02 INFO - 		Launched map tasks=1
22-02-2019 16:24:52 CST cmd02 INFO - 		Launched reduce tasks=1
22-02-2019 16:24:52 CST cmd02 INFO - 		Other local map tasks=1
22-02-2019 16:24:52 CST cmd02 INFO - 		Total time spent by all maps in occupied slots (ms)=9887
22-02-2019 16:24:52 CST cmd02 INFO - 		Total time spent by all reduces in occupied slots (ms)=3938
22-02-2019 16:24:52 CST cmd02 INFO - 		Total time spent by all map tasks (ms)=9887
22-02-2019 16:24:52 CST cmd02 INFO - 		Total time spent by all reduce tasks (ms)=3938
22-02-2019 16:24:52 CST cmd02 INFO - 		Total vcore-milliseconds taken by all map tasks=9887
22-02-2019 16:24:52 CST cmd02 INFO - 		Total vcore-milliseconds taken by all reduce tasks=3938
22-02-2019 16:24:52 CST cmd02 INFO - 		Total megabyte-milliseconds taken by all map tasks=10124288
22-02-2019 16:24:52 CST cmd02 INFO - 		Total megabyte-milliseconds taken by all reduce tasks=4032512
22-02-2019 16:24:52 CST cmd02 INFO - 	Map-Reduce Framework
22-02-2019 16:24:52 CST cmd02 INFO - 		Map input records=227785
22-02-2019 16:24:52 CST cmd02 INFO - 		Map output records=227785
22-02-2019 16:24:52 CST cmd02 INFO - 		Map output bytes=43772219
22-02-2019 16:24:52 CST cmd02 INFO - 		Map output materialized bytes=44455580
22-02-2019 16:24:52 CST cmd02 INFO - 		Input split bytes=145
22-02-2019 16:24:52 CST cmd02 INFO - 		Combine input records=0
22-02-2019 16:24:52 CST cmd02 INFO - 		Combine output records=0
22-02-2019 16:24:52 CST cmd02 INFO - 		Reduce input groups=10082
22-02-2019 16:24:52 CST cmd02 INFO - 		Reduce shuffle bytes=44455580
22-02-2019 16:24:52 CST cmd02 INFO - 		Reduce input records=227785
22-02-2019 16:24:52 CST cmd02 INFO - 		Reduce output records=10082
22-02-2019 16:24:52 CST cmd02 INFO - 		Spilled Records=455570
22-02-2019 16:24:52 CST cmd02 INFO - 		Shuffled Maps =1
22-02-2019 16:24:52 CST cmd02 INFO - 		Failed Shuffles=0
22-02-2019 16:24:52 CST cmd02 INFO - 		Merged Map outputs=1
22-02-2019 16:24:52 CST cmd02 INFO - 		GC time elapsed (ms)=260
22-02-2019 16:24:52 CST cmd02 INFO - 		CPU time spent (ms)=15910
22-02-2019 16:24:52 CST cmd02 INFO - 		Physical memory (bytes) snapshot=486187008
22-02-2019 16:24:52 CST cmd02 INFO - 		Virtual memory (bytes) snapshot=4343554048
22-02-2019 16:24:52 CST cmd02 INFO - 		Total committed heap usage (bytes)=351272960
22-02-2019 16:24:52 CST cmd02 INFO - 	Log_error
22-02-2019 16:24:52 CST cmd02 INFO - 		Parse_error=0
22-02-2019 16:24:52 CST cmd02 INFO - 	Log_normal
22-02-2019 16:24:52 CST cmd02 INFO - 		Map_user_total=227785
22-02-2019 16:24:52 CST cmd02 INFO - 		reduce_output_count=10082
22-02-2019 16:24:52 CST cmd02 INFO - 		reduce_userId_count=10082
22-02-2019 16:24:52 CST cmd02 INFO - 	Shuffle Errors
22-02-2019 16:24:52 CST cmd02 INFO - 		BAD_ID=0
22-02-2019 16:24:52 CST cmd02 INFO - 		CONNECTION=0
22-02-2019 16:24:52 CST cmd02 INFO - 		IO_ERROR=0
22-02-2019 16:24:52 CST cmd02 INFO - 		WRONG_LENGTH=0
22-02-2019 16:24:52 CST cmd02 INFO - 		WRONG_MAP=0
22-02-2019 16:24:52 CST cmd02 INFO - 		WRONG_REDUCE=0
22-02-2019 16:24:52 CST cmd02 INFO - 	File Input Format Counters 
22-02-2019 16:24:52 CST cmd02 INFO - 		Bytes Read=0
22-02-2019 16:24:52 CST cmd02 INFO - 	File Output Format Counters 
22-02-2019 16:24:52 CST cmd02 INFO - 		Bytes Written=10153717
22-02-2019 16:24:53 CST cmd02 INFO - Process completed successfully in 28 seconds.
22-02-2019 16:24:53 CST cmd02 INFO - output properties file=/mnt/home/11899517/azkaban-exec/executions/1647/cmd02_output_453259436916266008_tmp
22-02-2019 16:24:53 CST cmd02 INFO - Finishing job cmd02 at 1550823893201 with status SUCCEEDED
```
