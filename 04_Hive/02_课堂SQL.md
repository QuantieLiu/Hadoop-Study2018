
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
hive> SELECT T2.gender,count(T1.USER_ID) as count_order,sum(pay_amount) as sum_amount,avg(pay_amount) as avg_amount
    > FROM
    > (SELECT user_id,pay_amount from bigdata.orders )t1
    > join
    > (SELECT user_id,gender from bigdata.menber) t2
    > on T1.USER_ID = T2.USER_ID
    > GROUP BY gender;
Query ID = 11899517_20190106215403_eab343cd-a743-44a1-81c7-0bc71f458c55
Total jobs = 1
Execution log at: /tmp/11899517/11899517_20190106215403_eab343cd-a743-44a1-81c7-0bc71f458c55.log
2019-01-06 21:54:07	Starting to launch local task to process map join;	maximum memory = 508559360
2019-01-06 21:54:08	Dump the side-table for tag: 0 with group count: 15104 into file: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-54-03_872_4633236082702098128-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile30--.hashtable
2019-01-06 21:54:08	Uploaded 1 File to: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_21-54-03_872_4633236082702098128-1/-local-10004/HashTable-Stage-2/MapJoin-mapfile30--.hashtable (652905 bytes)
2019-01-06 21:54:08	End of local task; Time Taken: 1.504 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21446, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21446/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21446
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2019-01-06 21:54:16,699 Stage-2 map = 0%,  reduce = 0%
2019-01-06 21:54:21,895 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 4.75 sec
2019-01-06 21:54:29,200 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 7.23 sec
MapReduce Total cumulative CPU time: 7 seconds 230 msec
Ended Job = job_1535253853575_21446
MapReduce Jobs Launched: 
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 7.23 sec   HDFS Read: 4830168 HDFS Write: 78 SUCCESS
Total MapReduce CPU Time Spent: 7 seconds 230 msec
OK
女	7635	474593.0	62.16018336607728
男	7469	464502.0	62.19065470611862
Time taken: 26.392 seconds, Fetched: 2 row(s)
``` 

#### 正则表达式

``` 
商品访问量,从req_url中提取商品id并统计数量
1 检查一个串是否含有某种子串  A regexp '[0-9]+'
2 将匹配的子串替换           regexp_replace(A,'[0-9]+','')
3 从某个串中取出符合某个条件的子串  regexp_extract(A,'([0-9]+)',1)
``` 

``` 
--统计商品访问量最高的几种商品
hive> SELECT regexp_extract(req_url,'.*/product/([0-9]+).*',1) as product_id,count(DISTINCT user_id) as count_user
    > FROM bigdata.weblog WHERE active_name = 'pageview' GROUP BY regexp_extract(req_url,'.*/product/([0-9]+).*',1)
    > sort BY count_user DESC limit 5;
Query ID = 11899517_20190106215818_3e02ab81-1c9f-46d7-92b1-3e237411668f
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21450, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21450/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21450
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2019-01-06 21:58:25,583 Stage-1 map = 0%,  reduce = 0%
2019-01-06 21:58:30,779 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 2.33 sec
2019-01-06 21:58:42,136 Stage-1 map = 79%,  reduce = 17%, Cumulative CPU 24.83 sec
2019-01-06 21:58:44,188 Stage-1 map = 100%,  reduce = 17%, Cumulative CPU 28.76 sec
2019-01-06 21:58:46,244 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 32.61 sec
MapReduce Total cumulative CPU time: 32 seconds 610 msec
Ended Job = job_1535253853575_21450
Launching Job 2 out of 3
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21451, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21451/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21451
Hadoop job information for Stage-2: number of mappers: 1; number of reducers: 1
2019-01-06 21:58:54,084 Stage-2 map = 0%,  reduce = 0%
2019-01-06 21:58:59,278 Stage-2 map = 100%,  reduce = 0%, Cumulative CPU 1.4 sec
2019-01-06 21:59:04,444 Stage-2 map = 100%,  reduce = 100%, Cumulative CPU 3.21 sec
MapReduce Total cumulative CPU time: 3 seconds 210 msec
Ended Job = job_1535253853575_21451
Launching Job 3 out of 3
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21452, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21452/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21452
Hadoop job information for Stage-3: number of mappers: 1; number of reducers: 1
2019-01-06 21:59:13,324 Stage-3 map = 0%,  reduce = 0%
2019-01-06 21:59:18,483 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 1.31 sec
2019-01-06 21:59:24,645 Stage-3 map = 100%,  reduce = 100%, Cumulative CPU 3.21 sec
MapReduce Total cumulative CPU time: 3 seconds 210 msec
Ended Job = job_1535253853575_21452
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 32.61 sec   HDFS Read: 156636383 HDFS Write: 3837 SUCCESS
Stage-Stage-2: Map: 1  Reduce: 1   Cumulative CPU: 3.21 sec   HDFS Read: 8097 HDFS Write: 265 SUCCESS
Stage-Stage-3: Map: 1  Reduce: 1   Cumulative CPU: 3.21 sec   HDFS Read: 4800 HDFS Write: 91 SUCCESS
Total MapReduce CPU Time Spent: 39 seconds 30 msec
OK
	10031
1527235438747497	697
1527235438749547	682
1527235438747427	679
1527235438751281	672
Time taken: 67.55 seconds, Fetched: 5 row(s)
``` 

#### 窗口函数
<li>多行输入，多行输出，包含所在分组信息

``` 
--下单用户路径分析
####从哪个页面着陆  -- 窗口内排序或偏移量
####下单前最后页面  -- 窗口内偏移量
####下单之前浏览多少个页面  -- 窗口内聚合

Function(arg1,...argn)  OVER([PARTITION BY<...>] [ORDER BY<...>] [window_clause])
偏移量函数 First_value/Last_value Lead/Lag
聚合函数 COUNT/SUM AVG/MIN/MAX
排序函数 ROW_NUMBER() RANK() DENSE_RANK()
``` 

``` 
--窗口范围
hive> SELECT user_id,session_id,landing_page,last_page,count_visit 
    > FROM
    > (SELECT 
    > user_id,
    > session_id,
    > active_name,
    > first_value(req_url) over (partition by session_id order by time_tag asc) as landing_page,
    > lag(req_url,1) over (partition by session_id order by time_tag asc) as last_page,
    > count(if(active_name = 'pageview',req_url,null)) over (partition by session_id order by time_tag asc rows between unbounded preceding and current row) as count_visit
    > from bigdata.weblog ) t1
    > where active_name = 'order' limit 5;
Query ID = 11899517_20190106220951_bfa870e8-5433-4f92-824e-84ef445f9c6c
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21454, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21454/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21454
Hadoop job information for Stage-1: number of mappers: 2; number of reducers: 1
2019-01-06 22:09:58,868 Stage-1 map = 0%,  reduce = 0%
2019-01-06 22:10:04,126 Stage-1 map = 50%,  reduce = 0%, Cumulative CPU 1.35 sec
2019-01-06 22:10:11,339 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 13.41 sec
2019-01-06 22:10:13,386 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 18.2 sec
MapReduce Total cumulative CPU time: 18 seconds 200 msec
Ended Job = job_1535253853575_21454
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 2  Reduce: 1   Cumulative CPU: 18.2 sec   HDFS Read: 156638133 HDFS Write: 753 SUCCESS
Total MapReduce CPU Time Spent: 18 seconds 200 msec
OK
0911531897102177	00139537ff9c4538af543724872bebc3	http://www.bigdataclass.com/product/1527235438746733	http://www.bigdataclass.com/product/1527235438746948	18
9511531897261655	001665d33b2943478ee9b490bb589b05	http://www.bigdataclass.com/category	http://www.bigdataclass.com/product/1527235438749767	18
6641531897118281	001a9d543d7c4d7e94fcf4eed53b579e	http://www.bigdataclass.com/my/6641531897118281	http://www.bigdataclass.com/product/1527235438747370	15
9011531897335411	00389d719b9a4c719bade3881142597c	http://www.bigdataclass.com/category	http://www.bigdataclass.com/product/1527235438746948	13
9921531897309712	00395e8bed5448e491330d0e447c246b	http://www.bigdataclass.com/my/9921531897309712	http://www.bigdataclass.com/product/1527235438747744	18
Time taken: 22.67 seconds, Fetched: 5 row(s)
``` 

#### 行转列 列转行(聚合函数的一种)

``` 
### collect_set 不插入重复记录
### collect_list 保留重复记录
``` 

``` 
--
hive> SELECT user_id,collect_set(product_id),collect_list(product_id)
    > FROM bigdata.orders
    > GROUP BY user_id limit 10;
Query ID = 11899517_20190106221416_2e4036c3-a532-4dd0-92c6-b0c886188063
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21456, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21456/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21456
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2019-01-06 22:14:24,229 Stage-1 map = 0%,  reduce = 0%
2019-01-06 22:14:29,502 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 3.6 sec
2019-01-06 22:14:35,741 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 6.3 sec
MapReduce Total cumulative CPU time: 6 seconds 300 msec
Ended Job = job_1535253853575_21456
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 6.3 sec   HDFS Read: 2064753 HDFS Write: 510 SUCCESS
Total MapReduce CPU Time Spent: 6 seconds 300 msec
OK
0001528166031761	["1527235438747744"]	["1527235438747744"]
0001531897281376	["1527235438746891"]	["1527235438746891"]
0001531897292909	["1527235438750711"]	["1527235438750711"]
0001531897299559	["1527235438750618"]	["1527235438750618"]
0001531897444689	["1527235438750087"]	["1527235438750087"]
0001531898180216	["1527235438750087"]	["1527235438750087"]
0001531898269760	["1527235438747610"]	["1527235438747610"]
0001531898480773	["1527235438748507"]	["1527235438748507"]
0011528165930044	["1527235438748829"]	["1527235438748829"]
0011528165983913	["1527235438746782"]	["1527235438746782"]
Time taken: 19.963 seconds, Fetched: 10 row(s)
``` 

#### 用户画像
<li>建立key-value中间表，分组计算各字段，列转行，存入中间表，中间表 行转列 为大宽表

``` 
--建中间表
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.user_tag_value`(
    > `user_id` string COMMENT '用户id',
    > `tag` string COMMENT '标签',
    > `value` string COMMENT '标签值')
    > PARTITIONED BY(
    > `module` string COMMENT '标签模块')
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/hive/user_profile/tag_value';
OK
Time taken: 0.136 seconds

--建立标签表
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.user_profile`(
    > `user_id` string COMMENT '用户id',
    > `profile` string COMMENT '用户标签')
    > ROW FORMAT SERDE
    > 'org.openx.data.jsonserde.JsonSerDe'
    > STORED AS INPUTFORMAT
    > 'org.apache.hadoop.mapred.TextInputFormat'
    > OUTPUTFORMAT
    > 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
    > LOCATION
    > '/user/11899517/hive/user_profile/user_profile';
OK
Time taken: 0.064 seconds
``` 

``` 
--计算各字段，存入中间表
--计算 性别 年龄 设备类型 注册日期
hive> INSERT overwrite table bigdata.user_tag_value partition(module = 'basic_info')
    > SELECT user_id,mp['key'],mp['value'] FROM
    > (SELECT 
    > user_id,
    > array(map('key','gender','value',gender),
    > map('key','age','value',cast(2018-from_unixtime(cast(birthday/1000 as bigint),'yyyy') as string)),
    > map('key','device_type','value',device_type),
    > map('key','register_day','value',from_unixtime(cast(register_time / 1000 as bigint),'yyyy-MM-dd'))
    > )as arr 
    > FROM bigdata.menber ) s lateral view explode(arr) arrtable as mp;
Query ID = 11899517_20190107133146_4bf5b13c-6c63-4e4c-973c-b1aec796d999
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks is set to 0 since there's no reduce operator
Starting Job = job_1535253853575_21542, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21542/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21542
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 0
2019-01-07 13:31:53,763 Stage-1 map = 0%,  reduce = 0%
2019-01-07 13:32:00,137 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 6.96 sec
MapReduce Total cumulative CPU time: 6 seconds 960 msec
Ended Job = job_1535253853575_21542
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to: hdfs://bigdata0:50000/user/11899517/hive/user_profile/tag_value/module=basic_info/.hive-staging_hive_2019-01-07_13-31-46_013_7520910552980103743-1/-ext-10000
Loading data to table bigdata.user_tag_value partition (module=basic_info)
Partition bigdata.user_tag_value{module=basic_info} stats: [numFiles=1, numRows=120716, totalSize=7796231, rawDataSize=0]
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1   Cumulative CPU: 6.96 sec   HDFS Read: 4822266 HDFS Write: 7796331 SUCCESS
Total MapReduce CPU Time Spent: 6 seconds 960 msec
OK
Time taken: 16.284 seconds



--计算 首次下单时间 最近下单时间 下单次数 下单余额
INSERT overwrite table bigdata.user_tag_value partition(module = 'consume_info')
SELECT user_id,mp['key'],mp['value']
FROM
(SELECT user_id,
array(map('key','first_order_time','value',min(order_time)),
map('key','last_order_time','value',max(order_time)),
map('key','order_count','value',count(1)),
map('key','order_sum','value',sum(pay_amount))
) as arr 
FROM
bigdata.orders
GROUP BY
user_id
) s lateral view explode(arr) arrtable as mp;

INSERT结果：Time taken: 22.334 seconds, Fetched: 60416 row(s)
hive> SELECT user_id,mp['key'],mp['value']
    > FROM
    > (SELECT user_id,
    > array(map('key','first_order_time','value',min(order_time)),
    > map('key','last_order_time','value',max(order_time)),
    > map('key','order_count','value',count(1)),
    > map('key','order_sum','value',sum(pay_amount))
    > ) as arr 
    > FROM
    > bigdata.orders
    > GROUP BY
    > user_id
    > ) s lateral view explode(arr) arrtable as mp limit 10;
Query ID = 11899517_20190107212752_3e67814a-9e6c-4cbb-8ecb-6d99b0396fa3
Total jobs = 1
Launching Job 1 out of 1
Number of reduce tasks not specified. Estimated from input data size: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1535253853575_21562, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21562/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21562
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2019-01-07 21:27:59,336 Stage-1 map = 0%,  reduce = 0%
2019-01-07 21:28:04,638 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 3.6 sec
2019-01-07 21:28:10,849 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 7.2 sec
MapReduce Total cumulative CPU time: 7 seconds 200 msec
Ended Job = job_1535253853575_21562
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 7.2 sec   HDFS Read: 2067731 HDFS Write: 411 SUCCESS
Total MapReduce CPU Time Spent: 7 seconds 200 msec
OK
0001528166031761	first_order_time	1527578321393
0001528166031761	last_order_time	1527578321393
0001528166031761	order_count	1
0001528166031761	order_sum	40.0
0001531897281376	first_order_time	1527642284658
0001531897281376	last_order_time	1527642284658
0001531897281376	order_count	1
0001531897281376	order_sum	64.0
0001531897292909	first_order_time	1527639077523
0001531897292909	last_order_time	1527639077523
Time taken: 19.798 seconds, Fetched: 10 row(s)


--地理位置信息
INSERT overwrite table bigdata.user_tag_value partition(module = 'geography_info')
SELECT user_id,'province' as key , province
FROM(SELECT user_id,row_number() over (partition by user_id order by time_tag desc ) as order_rank,adress['province'] as province
FROM bigdata.weblog
WHERE active_name = 'pay') t1
WHERE order_rank =1 limit 20;


``` 


``` 

``` 
