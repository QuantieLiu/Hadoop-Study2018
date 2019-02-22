### 前期准备

```

```

### job文件
<li>新建空白文档，以.job后缀，内容如下
<li>window系统压缩,只能是zip格式

```
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





