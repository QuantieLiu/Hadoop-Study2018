
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
语法如下：
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]

说明：
    filepath 可能是：
        一个相对路径
        一个绝对路径，例如：/root/project/data1
        一个url地址，可选的可以带上授权信息，例如：hdfs://namenode:9000/user/hive/project/data1 
    目标可能是一个表或者分区，如果该表是分区，则必须制定分区列。
    filepath 可以是一个文件也可以是目录
    如果指定了 LOCAL，则：
        load 命令会在本地查找 filepath。如果 filepath 是相对路径，则相对于当前路径，也可以指定一个 url 或者本地文件，例如：file:///user/hive/project/data1
    如果没有指定 LOCAL ，则hive会使用全路径的url，url 中如果没有制定 schema，则默认使用 fs.default.name的值；如果该路径不是绝对路径，则会相对于 /user/<username>
    如果使用 OVERWRITE ，则会删除原来的数据，然后导入新的数据，否则，就是追加数据。

需要注意的：
    filepath 中不能包括子目录
    如果没有指定 LOCAL，则 filepath 指向目标表或者分区所在的文件系统。
``` 

``` 
hive> load data local inpath 'file:/mnt/home/11899517/log0605-output' into table bigdata.mrlog partition (day='2019-01-08');
Loading data to table bigdata.mrlog partition (day=2019-01-08)
Partition bigdata.mrlog{day=2019-01-08} stats: [numFiles=1, numRows=0, totalSize=158180009, rawDataSize=0]
OK
Time taken: 4.515 seconds
hive> select user_id from bigdata.mrlog limit 5;
OK
4921528165744221
4921528165744221
4921528165744221
4921528165744221
4921528165744221
Time taken: 0.13 seconds, Fetched: 5 row(s)
``` 
============往hive表中加载数据结束===============

``` 

``` 

``` 


``` 


``` 

``` 
