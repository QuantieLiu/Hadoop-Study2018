
#### 建数据库和表
<li>尤其注意 ` 和 ' 的使用, 数据库名/表名/分区名用`,中文/包路径/数据路径用'

``` 
hive> create database if not exists bigdata;
OK
Time taken: 0.124 seconds
hive> show databases;
OK
bigdata
default
Time taken: 0.011 seconds, Fetched: 2 row(s)
hive> 

hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.weblog1` ( 
    > `active_name` string COMMENT '事件名称',
    > `order_id` string COMMENT '订单id' )
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/weblog/weblog';
OK
Time taken: 0.294 seconds
hive> 
``` 

## 依据教学内容--开始
#### 建表&&分区

``` 
--日志表建表语句
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.weblog`(
    > `time_tag` bigint COMMENT '时间',
    > `active_name` string COMMENT '事件名称',
    > `device_id` string COMMENT '设备id',
    > `session_id` string COMMENT '会话id',
    > `user_id` string COMMENT '用户id',
    > `ip` string COMMENT 'ip地址',
    > `adress` map<string,string> COMMENT '地址',
    > `req_url` string COMMENT 'http请求地址',
    > `action_path` string COMMENT '访问路径',
    > `product_id` string COMMENT '商品id',
    > `order_id` string COMMENT '订单id')
    > PARTITIONED BY(
    > `day` string COMMENT '日期')
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/weblog/weblog';
OK
Time taken: 0.056 seconds

--查看表结构和信息
hive> desc bigdata.weblog;
OK
time_tag            	bigint              	时间                  
active_name         	string              	事件名称                
device_id           	string              	设备id                
session_id          	string              	会话id                
user_id             	string              	用户id                
ip                  	string              	ip地址                
adress              	map<string,string>  	地址                  
req_url             	string              	http请求地址            
action_path         	string              	访问路径                
product_id          	string              	商品id                
order_id            	string              	订单id                
day                 	string              	日期                  
	 	 
# Partition Information	 	 
# col_name            	data_type           	comment             
	 	 
day                 	string              	日期                  
Time taken: 0.67 seconds, Fetched: 17 row(s)

--手动建立分区
hive> ALTER TABLE bigdata.weblog ADD PARTITION (day='2018-05-30') location '/user/11899517/weblog/weblog/day=2018-05-30';
OK
Time taken: 0.158 seconds

--查看表分区
hive> show partitions bigdata.weblog;
OK
day=2018-05-29
day=2018-05-30
Time taken: 0.055 seconds, Fetched: 2 row(s)
``` 

#### 关联查询

``` 
--建立会员表
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.menber` (
    > `user_id` string COMMENT '用户id',
    > `nick_name` string COMMENT '昵称',
    > `name` string COMMENT '姓名',
    > `gender` string COMMENT '性别',
    > `register_time` bigint COMMENT '注册时间',
    > `birthday` bigint COMMENT '生日',
    > `device_type` string COMMENT '设备类型')
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/hive/member';
OK
Time taken: 0.053 seconds
hive> desc bigdata.menber;
OK
user_id             	string              	用户id                
nick_name           	string              	昵称                  
name                	string              	姓名                  
gender              	string              	性别                  
register_time       	bigint              	注册时间                
birthday            	bigint              	生日                  
device_type         	string              	设备类型                
Time taken: 0.037 seconds, Fetched: 7 row(s)

--交叉查询
hive> SELECT t1.user_id,gender,register_time FROM 
    > (SELECT user_id FROM  bigdata.weblog WHERE active_name = 'order') t1
    > JOIN
    > (SELECT user_id,gender,register_time FROM bigdata.menber )t2
    > ON T1.USER_ID = T2.USER_ID limit 10;
Query ID = 11899517_20190106212348_0f745322-5582-4e0e-8de5-6545f15bcd8c
Total jobs = 1
Execution log at: /tmp/11899517/11899517_20190106212348_0f745322-5582-4e0e-8de5-6545f15bcd8c.log
2019-01-06 21:23:52	Starting to launch local task to process map join;	maximum memory = 508559360
2019-01-06 21:23:53	Dump the side-table for tag: 1 with group count: 30179 into file: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-23-48_129_8021444936995239706-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile11--.hashtable
2019-01-06 21:23:53	Uploaded 1 File to: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-23-48_129_8021444936995239706-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile11--.hashtable (1395272 bytes)
2019-01-06 21:23:53	End of local task; Time Taken: 1.553 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
Starting Job = job_1535253853575_21428, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21428/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21428
Hadoop job information for Stage-3: number of mappers: 2; number of reducers: 0
2019-01-06 21:24:01,759 Stage-3 map = 0%,  reduce = 0%
2019-01-06 21:24:07,125 Stage-3 map = 50%,  reduce = 0%, Cumulative CPU 3.6 sec
2019-01-06 21:24:08,153 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 8.18 sec
MapReduce Total cumulative CPU time: 8 seconds 180 msec
Ended Job = job_1535253853575_21428
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 2   Cumulative CPU: 8.18 sec   HDFS Read: 341517 HDFS Write: 380 SUCCESS
Total MapReduce CPU Time Spent: 8 seconds 180 msec
OK
0911531897102177	男	1519633447871
9511531897261655	男	1523063948348
6641531897118281	男	1518795859021
9011531897335411	女	1527652489929
9921531897309712	男	1521722601079
7661531897500316	男	1527643953317
1591531897238231	女	1519734654301
9371531896996639	女	1527685035678
4201531897485566	女	1523335366848
7281531897453960	男	1527643404566
Time taken: 21.213 seconds, Fetched: 10 row(s)
``` 

``` 
JOIN的不同类型
Join | left semi join  交集
left outer join  左表全显示
full outer join  两张表都显示
right outer join 右表全显示
``` 

#### 内置函数的使用
##### 分类：
<li> 简单函数  一进一出，格式转化，数据运算等
<li> 聚合函数  多进一出，统计计算，指标聚合
<li> 集合函数  array，map操作
<li> 特殊函数  窗口函数等

``` 
--将时间戳转换为年月日格式显示
--from_unixtime()使用
hive> SELECT t1.user_id,gender,from_unixtime(cast(register_time/1000 as bigint),'yyyy-MM-dd HH:MM:ss') as register_day 
    > FROM 
    > (SELECT user_id FROM  bigdata.weblog WHERE active_name = 'order') t1
    > JOIN
    > (SELECT user_id,gender,register_time FROM bigdata.menber )t2
    > ON T1.USER_ID = T2.USER_ID  limit 10;
Query ID = 11899517_20190106212630_045c28da-7c61-43c0-86ac-7bfb79f17ece
Total jobs = 1
Execution log at: /tmp/11899517/11899517_20190106212630_045c28da-7c61-43c0-86ac-7bfb79f17ece.log
2019-01-06 21:26:32	Starting to launch local task to process map join;	maximum memory = 508559360
2019-01-06 21:26:34	Dump the side-table for tag: 1 with group count: 30179 into file: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-26-30_068_3808927890601021906-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile21--.hashtable
2019-01-06 21:26:34	Uploaded 1 File to: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-26-30_068_3808927890601021906-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile21--.hashtable (1395272 bytes)
2019-01-06 21:26:34	End of local task; Time Taken: 1.887 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
Starting Job = job_1535253853575_21430, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21430/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21430
Hadoop job information for Stage-3: number of mappers: 2; number of reducers: 0
2019-01-06 21:26:42,372 Stage-3 map = 0%,  reduce = 0%
2019-01-06 21:26:48,551 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 8.14 sec
MapReduce Total cumulative CPU time: 8 seconds 140 msec
Ended Job = job_1535253853575_21430
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 2   Cumulative CPU: 8.14 sec   HDFS Read: 343101 HDFS Write: 440 SUCCESS
Total MapReduce CPU Time Spent: 8 seconds 140 msec
OK
0911531897102177	男	2018-02-26 16:02:07
9511531897261655	男	2018-04-07 09:04:08
6641531897118281	男	2018-02-16 23:02:19
9011531897335411	女	2018-05-30 11:05:49
9921531897309712	男	2018-03-22 20:03:21
7661531897500316	男	2018-05-30 09:05:33
1591531897238231	女	2018-02-27 20:02:54
9371531896996639	女	2018-05-30 20:05:15
4201531897485566	女	2018-04-10 12:04:46
7281531897453960	男	2018-05-30 09:05:24
Time taken: 19.539 seconds, Fetched: 10 row(s)
``` 

``` 
日期函数：from_unixtime,unix_timestamp,to_date,date_diff ...
字符串函数：concat,concat_ws,format_number,upper/lower,parse_url ...
条件函数：if,case..when.. ...
数学函数：round/ceil/floor,log,power,rand ...
``` 

``` 
--查看hive自带函数
show functions;
--查看某一函数的用法
describe function weekofyear
``` 

#### 聚合函数

``` 
--统计某商品页面的访问次数
hive> SELECT req_url,count(1) FROM bigdata.weblog 
    > WHERE active_name ='pageview' and req_url LIKE '%/product/%'
    > GROUP BY req_url limit 5;
Query ID = 11899517_20190106213717_6f3a4ffa-45ad-4aa9-ae24-6048912f6003
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21437, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21437/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21437
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2019-01-06 21:37:23,043 Stage-1 map = 0%,  reduce = 0%
2019-01-06 21:37:29,263 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 2.25 sec
2019-01-06 21:37:36,518 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 14.69 sec
2019-01-06 21:37:37,548 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 17.11 sec
MapReduce Total cumulative CPU time: 17 seconds 110 msec
Ended Job = job_1535253853575_21437
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 17.11 sec   HDFS Read: 156636366 HDFS Write: 285 SUCCESS
Total MapReduce CPU Time Spent: 17 seconds 110 msec
OK
http://www.bigdataclass.com/product/1527235438746721	663
http://www.bigdataclass.com/product/1527235438746730	652
http://www.bigdataclass.com/product/1527235438746733	628
http://www.bigdataclass.com/product/1527235438746740	619
http://www.bigdataclass.com/product/1527235438746770	658
Time taken: 21.578 seconds, Fetched: 5 row(s)
``` 

``` 
--建订单表
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.orders`(
    > `order_id` string COMMENT '订单id',
    > `user_id` string COMMENT '用户id',
    > `product_id` string COMMENT '商品id',
    > `order_time` bigint COMMENT '下单时间',
    > `pay_amount` double COMMENT '金额')
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/hive/orders';
OK
Time taken: 0.041 seconds
hive> 


``` 


#### 

``` 

``` 

#### 

``` 

``` 


#### 

``` 

``` 



#### 

``` 

``` 
