### channel

<li>type

```
hdfs
```

<li>path

```
写入hdfs的路径，需要包含文件系统标识，比如：hdfs://namenode/flume/webdata/
可以使用flume提供的日期及%{host}表达式
```

<li>filePrefix

```
默认值：FlumeData
写入hdfs的文件名前缀，可以使用flume提供的日期及%{host}表达式
```

<li>fileSuffix

```
写入hdfs的文件名后缀，比如：.lzo .log等
```

<li>inUsePrefix

```
临时文件的文件名前缀，hdfs sink会先往目标目录中写临时文件，再根据相关规则重命名成最终目标文件；
```


<li>inUseSuffix

```
默认值：.tmp
临时文件的文件名后缀
```

<li>rollInterval

```
默认值：30
hdfs sink间隔多长将临时文件滚动成最终目标文件，单位：秒；
如果设置成0，则表示不根据时间来滚动文件；
注：滚动（roll）指的是，hdfs sink将临时文件重命名成最终目标文件，并新打开一个临时文件来写入数据；
```

<li>rollSize

```
默认值：1024
当临时文件达到该大小（单位：bytes）时，滚动成目标文件；
如果设置成0，则表示不根据临时文件大小来滚动文件；
```

<li>rollCount

```
默认值：10
当events数据达到该数量时候，将临时文件滚动成目标文件；
如果设置成0，则表示不根据events数据来滚动文件；
```

<li>idleTimeout

```
默认值：0
当目前被打开的临时文件在该参数指定的时间（秒）内，没有任何数据写入，则将该临时文件关闭并重命名成目标文件；
```

<li>batchSize

```
默认值：100
每个批次刷新到HDFS上的events数量；
```


<li>codeC

```
文件压缩格式，包括：gzip, bzip2, lzo, lzop, snappy
```

<li>fileType

```
默认值：SequenceFile
文件格式，包括：SequenceFile, DataStream,CompressedStream
当使用DataStream时候，文件不会被压缩，不需要设置hdfs.codeC;
当使用CompressedStream时候，必须设置一个正确的hdfs.codeC值；
```

<li>maxOpenFiles

```
默认值：5000
最大允许打开的HDFS文件数，当打开的文件数达到该值，最早打开的文件将会被关闭；
```

<li>minBlockReplicas

```
默认值：HDFS副本数
写入HDFS文件块的最小副本数。
该参数会影响文件的滚动配置，一般将该参数配置成1，才可以按照配置正确滚动文件。
待研究。
```

<li>writeFormat

```
写sequence文件的格式。包含：Text, Writable（默认）
```

<li>callTimeout

```
默认值：10000
执行HDFS操作的超时时间（单位：毫秒）；
```

<li>threadsPoolSize

```
默认值：10
```

<li>threadsPoolSize

```
默认值：10
hdfs sink启动的操作HDFS的线程数
```

<li>rollTimerPoolSize

```
默认值：1
hdfs sink启动的根据时间滚动文件的线程数
```

<li>kerberosPrincipal

```
HDFS安全认证kerberos配置；
```

<li>kerberosKeytab

```
HDFS安全认证kerberos配置；
```

<li>proxyUser

```
代理用户
```

<li>round

```
默认值：false
是否启用时间上的”舍弃”，这里的”舍弃”，类似于”四舍五入”，后面再介绍。
如果启用，则会影响除了%t的其他所有时间表达式；
```

<li>roundValue

```
默认值：1
时间上进行“舍弃”的值；
```

<li>roundUni

```
默认值：seconds
时间上进行”舍弃”的单位，包含：second,minute,hour
```

```
示例：
a1.sinks.k1.hdfs.path = /flume/events/%y-%m-%d/%H%M/%S
a1.sinks.k1.hdfs.round = true
a1.sinks.k1.hdfs.roundValue = 10
a1.sinks.k1.hdfs.roundUnit = minute
当时间为2015-10-16 17:38:59时候，hdfs.path依然会被解析为：
/flume/events/20151016/17:30/00
因为设置的是舍弃10分钟内的时间，因此，该目录每10分钟新生成一个。
```

<li>timeZone

```
默认值：Local Time
时区
```


<li>useLocalTimeStamp

```
默认值：flase
是否使用当地时间
```

<li>closeTries

```
默认值：0
hdfs sink关闭文件的尝试次数；
如果设置为1，当一次关闭文件失败后，hdfs sink将不会再次尝试关闭文件，这个未关闭的文件将会一直留在那，并且是打开状态。
设置为0，当一次关闭失败后，hdfs sink会继续尝试下一次关闭，直到成功。
```

<li>retryInterval

```
默认值：180（秒）
hdfs sink尝试关闭文件的时间间隔，如果设置为0，表示不尝试，相当于于将hdfs.closeTries设置成1.
```

<li>serializer

```
默认值：TEXT
序列化类型。其他还有：avro_event或者是实现了EventSerializer.Builder的类名
```
