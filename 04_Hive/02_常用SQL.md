
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
    > ON T1.USER_ID = T2.USER_ID;
Query ID = 11899517_20190106205115_c7b3eb53-9f0e-42ba-9561-25f30965e716
Total jobs = 1
Execution log at: /tmp/11899517/11899517_20190106205115_c7b3eb53-9f0e-42ba-9561-25f30965e716.log
2019-01-06 20:51:20	Starting to launch local task to process map join;	maximum memory = 508559360
2019-01-06 20:51:21	Dump the side-table for tag: 1 with group count: 0 into file: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_20-51-15_885_6920543228601382816-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile01--.hashtable
2019-01-06 20:51:21	Uploaded 1 File to: file:/mnt/home/11899517/hivefile/tmpdir/11899517/f45b52e5-8b16-4978-903d-ade781a85356/hive_2019-01-06_20-51-15_885_6920543228601382816-1/-local-10003/HashTable-Stage-3/MapJoin-mapfile01--.hashtable (260 bytes)
2019-01-06 20:51:21	End of local task; Time Taken: 1.302 sec.
Execution completed successfully
MapredLocal task succeeded
Launching Job 1 out of 1
Number of reduce tasks is set to 0 since there's no reduce operator
Starting Job = job_1535253853575_21391, Tracking URL = http://bigdata0.novalocal:8088/proxy/application_1535253853575_21391/
Kill Command = /home/hadoop/hadoop-current/bin/hadoop job  -kill job_1535253853575_21391
Hadoop job information for Stage-3: number of mappers: 2; number of reducers: 0
2019-01-06 20:51:30,439 Stage-3 map = 0%,  reduce = 0%
2019-01-06 20:51:36,721 Stage-3 map = 50%,  reduce = 0%, Cumulative CPU 2.76 sec
2019-01-06 20:51:44,001 Stage-3 map = 100%,  reduce = 0%, Cumulative CPU 15.12 sec
MapReduce Total cumulative CPU time: 15 seconds 120 msec
Ended Job = job_1535253853575_21391
MapReduce Jobs Launched: 
Stage-Stage-3: Map: 2   Cumulative CPU: 15.12 sec   HDFS Read: 156637175 HDFS Write: 0 SUCCESS
Total MapReduce CPU Time Spent: 15 seconds 120 msec
OK
Time taken: 29.205 seconds

--由于bigdata.menber为空，此次查询没有输出数据
hive> select user_id,gender,register_time from bigdata.menber limit 10;
OK
Time taken: 0.092 seconds
``` 

``` 
JOIN的不同类型
Join | left semi join  交集
left outer join  左表全显示
full outer join  两张表都显示
right outer join 右表全显示
``` 

#### 内置函数的使用

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



#### 

``` 

``` 
