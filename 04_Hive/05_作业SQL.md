
#### 数据准备：MR视频的output：log0605-output

``` 
--复制到集群
sudo scp -i /home/qly/.ssh/11899517 log0605-output 11899517@10.173.32.6:/mnt/home/11899517/
--复制到hdfs文件系统
hadoop fs -copyFromLocal log0605-output  /user/11899517/output
``` 

#### 建立数据表

``` 
hive> CREATE EXTERNAL TABLE IF NOT EXISTS `bigdata.mrlog`(
    > `time_tag` bigint COMMENT '时间',
    > `active_name` string COMMENT '事件名称',
    > `device_id` string COMMENT '设备id',
    > `session_id` string COMMENT '会话id',
    > `user_id` string COMMENT '用户id',
    > `ip` string COMMENT 'ip地址',
    > `address` map<string,string> COMMENT '地址',
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
    > '/user/11899517/mrlogs';
OK
Time taken: 0.802 seconds
hive> select user_id from bigdata.mrlog limit 5;
OK
Time taken: 1.094 seconds
``` 

<li>LOCATION'/user/11899517/mrlogs'中有 log0605-output 
<li>此时查询结果是空表
<li>但这些数据还需要经过hive处理才能查询

============往hive表中加载数据开始===============

``` 

``` 

``` 

``` 

============往hive表中加载数据开始===============

``` 

``` 

``` 


``` 


``` 

``` 
