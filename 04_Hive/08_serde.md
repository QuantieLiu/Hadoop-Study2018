Serialize/Deserilize
简称：SerDe

#### 序列化
<li>对象转换为字节序列的过程

```
--用途
1 对象的持久化，把对象转换成字节序列后保存到文件中
2 对象数据的网络传送
3 将数据加载到表中而不需要对数据进行转换（hive特有）
```

#### 反序列化
<li>字节序列恢复为对象的过程

```
--用途
1 对key/value反序列化成hive table的每个列的值
```
#### hive如何去处理一条记录

```
1 Serialize把hive使用的java object转换成能写入hdfs的字节序列，或者其他系统能识别的流文件
2 Deserilize把字符串或者二进制流转换成hive能识别的java object对象。比如：select语句会用到Serialize对象， 把hdfs数据解析出来；insert语句会使用Deserilize，数据写入hdfs系统，需要把数据序列化。
```





