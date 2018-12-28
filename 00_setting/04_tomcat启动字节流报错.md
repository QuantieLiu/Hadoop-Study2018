### JDK1.8

``` 
在JRE中设置Default VM arguments
-noverify
```  

### JDK1.7

```  

-XX:-UseSplitVerifier
-Xms256M -Xmx512M -XX:PermSize=256m -XX:MaxPermSize=512m
============
若报错内存太小，调整MaxPermSize即可

============
-vmargs：说明后面是VM的参数
-Xms40m：虚拟机占用系统的最小内存
-Xmx256m：虚拟机占用系统的最大内存
-XX:PermSize：最小栈内存大小。一般报内存不足时,都是说这个太小,堆空间剩余小于5%就会警告,建议把这个稍微设大一点,不过要视自己机器内存大小来设置
-XX:MaxPermSize：最大栈内存大小。这个也适当大些
-Xmx512M的5%为25.6M，理论上要求-Xmx的数值与-XX:MaxPermSize必须大于25.6M

```  
