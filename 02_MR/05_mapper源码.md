
#### 方法

```
setup()：
map真正开始干活之前的准备工作，例如相关配置文件的读取，参数的传递等

map()：
承担主要的处理工作

cleanup()：
收尾工作如关闭文件或者执行map()后的K-V分发等

run()：
提供了setup->map->cleanup()的执行模板
```

```
在MapReduce中，Mapper从一个输入分片中读取数据，然后经过Shuffle and Sort阶段，分发数据给Reducer，
在Reduce进行前，Map端和Reduce端我们可能使用设置的Combiner进行合并。
Partitioner控制每个K-V对应该被分发到哪个reducer[我们的Job可能有多个reducer]，
Hadoop默认使用HashPartitioner，HashPartitioner使用key的hashCode对reducer的数量取模得来
```
  
```
context是mapper里的一个内部类，主要是为了在map任务或者reduce任务中跟踪task的相关状态。
在mapper类中，这个context就可以存储一些job conf有关的信息。
在setup()方法中，就可以用context读取相关的配置信息。
```

#### 子类
<li>InverseMapper

```
--重写map()方法，将map阶段的k,v掉换了个，然后输出(v,k)对
```
  
<li>


```

```

