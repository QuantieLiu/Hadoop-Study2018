## Reduce阶段Iterable values中的每个值都共享一个对象

[源码剖析](https://blog.csdn.net/cnweike/article/details/51025340) has no title attribute.

<p>虽然reduce方法会反复执行多次，但key和value相关的对象只有两个，reduce会反复重用这两个对象。所以如果要保存key或者value的结果，
只能将其中的值取出另存或者重新clone一个对象（例如Text store = new Text(value) 或者 String a = value.toString()），而不能直接赋引用。
因为引用从始至终都是指向同一个对象，你如果直接保存它们，那最后它们都指向最后一个输入记录。会影响最终计算结果而出错
</p>
