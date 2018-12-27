#### Eclipse经常出现OutOfMemoryError

```  
-startup
plugins/org.eclipse.equinox.launcher_1.3.0.v20140415-2008.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.gtk.linux.x86_64_1.1.200.v20150204-1316
-product
org.eclipse.epp.package.jee.product
--launcher.defaultAction
openFile
-showsplash
--launcher.XXMaxPermSize2048M
org.eclipse.platform
--launcher.XXMaxPermSize
2048m
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vmargs
-Dosgi.requiredJavaVersion=1.6
-XX:MaxPermSize=1024m
-Xms2048m
-Xmx4096m
-XX:PermSize=256m  
```  

### 对外提供的可配置堆区的参数
<li>-Xms4096m
初始堆大小,设置JVM促使内存为2048m
堆区内存初始内存分配的大小

<li>-Xmx4096m
最大堆大小,设置JVM最大可用内存为2048M
堆区内存可被分配的最大上限
通常会将 -Xms 与 -Xmx两个参数的配置相同的值，其目的是为了能够在java垃圾回收机制清理完堆区后不需要重新分隔计算堆区的大小而浪费资源

一般来讲对于堆区的内存分配只需要对上述两个参数进行合理配置即可


<li>-XX:newSize
新生代初始内存的大小，应该小于 -Xms的值；

<li>-XX:MaxnewSize
新生代可被分配的内存的最大上限；当然这个值应该小于 -Xmx的值

<li>-Xmn
这个参数则是对 -XX:newSize、-XX:MaxnewSize两个参数的同时配置，
也就是说如果通过-Xmn来配置新生代的内存大小，那么-XX:newSize = -XX:MaxnewSize = -Xmn
JDK1.4版本以后才使用的


### 非堆区内存配置的参数
<li>-XX:PermSize=256m  
非堆区初始内存分配大小,缩写为permanent size（持久化内存）

<li>-XX:MaxPermSize=256m
设置持久代大小为1024m,非堆区分配的内存的最大上限
此处内存是不会被java垃圾回收机制进行处理

