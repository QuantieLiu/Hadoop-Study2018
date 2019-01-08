## Reduce阶段Iterable values中的每个值都共享一个对象

[源码剖析](https://blog.csdn.net/cnweike/article/details/51025340) .

```
 /**
   * Iterate through the values for the current key, reusing the same value 
   * object, which is stored in the context.
   * @return the series of values associated with the current key. All of the 
   * objects returned directly and indirectly from this method are reused.
   */
  public 
  Iterable<VALUEIN> getValues() throws IOException, InterruptedException {
    return iterable;
  }
```
<li>关于Iterable参考。。。
  
 ```
   protected void reduce(KEYIN key, Iterable<VALUEIN> values, Context context
                        ) throws IOException, InterruptedException {
    for(VALUEIN value: values) {
      context.write((KEYOUT) key, (VALUEOUT) value);
    }
  }

 ```
 
<li>一个循环中，每次得到的value实际上都是指向的同一个对象，只是在每次迭代的时候，将新的值反序列化到这个对象中，以更新此对象的值：
  
```
  /**
   * Advance to the next key/value pair.
   */
  @Override
  public boolean nextKeyValue() throws IOException, InterruptedException {
    if (!hasMore) {
      key = null;
      value = null;
      return false;
    }
    firstValue = !nextKeyIsSame;
    DataInputBuffer nextKey = input.getKey();
    currentRawKey.set(nextKey.getData(), nextKey.getPosition(), 
                      nextKey.getLength() - nextKey.getPosition());
    buffer.reset(currentRawKey.getBytes(), 0, currentRawKey.getLength());
    key = keyDeserializer.deserialize(key);
    DataInputBuffer nextVal = input.getValue();
    buffer.reset(nextVal.getData(), nextVal.getPosition(),
        nextVal.getLength() - nextVal.getPosition());
    value = valueDeserializer.deserialize(value);
 
    currentKeyLength = nextKey.getLength() - nextKey.getPosition();
    currentValueLength = nextVal.getLength() - nextVal.getPosition();
 
    hasMore = input.next();
    if (hasMore) {
      nextKey = input.getKey();
      nextKeyIsSame = comparator.compare(currentRawKey.getBytes(), 0, 
                                     currentRawKey.getLength(),
                                     nextKey.getData(),
                                     nextKey.getPosition(),
                                     nextKey.getLength() - nextKey.getPosition()
                                         ) == 0;
    } else {
      nextKeyIsSame = false;
    }
    inputValueCounter.increment(1);
    return true;
  }

```

<p>虽然reduce方法会反复执行多次，但key和value相关的对象只有两个，reduce会反复重用这两个对象。所以如果要保存key或者value的结果，
只能将其中的值取出另存或者重新clone一个对象（例如Text store = new Text(value) 或者 String a = value.toString()），而不能直接赋引用。
因为引用从始至终都是指向同一个对象，你如果直接保存它们，那最后它们都指向最后一个输入记录。会影响最终计算结果而出错
</p>

'''
  protected void reduce(KEYIN key, Iterable<VALUEIN> values, Context context
                        ) throws IOException, InterruptedException {
    for(VALUEIN value: values) {
      context.write((KEYOUT) key, (VALUEOUT) value);
    }
  }

'''

<li>在for循环中，每次得到的value实际上都是指向的同一个对象，只是在每次迭代的时候，将新的值反序列化到这个对象中，以更新此对象的值


特殊场景需要注意：
<li>1如果值的类型是自己实现的某种Writable，比如说AType
<li>2持有对其它对象的引用
<li>3在Reduce阶段还要对相邻的两个值（current_value，value）同时进行操作
  

<li>如果你仅仅是将value强制类型转换成相应的AType，current_value和value其实是指向的同一段内存空间，也就是说，当我们迭代完第一次的时候，current_value缓存了当前的值，但是当进行下一次迭代，取到的value，其实就是将它们共同指向的那段内存做了更新，
<li>另一种理解：current_value所指向的内存也会被更新成value的值，如果不了解Reduce阶段values的迭代实现，很可能就会造成莫名其妙的程序行为
<li>解决方案:创建一个全新的对象来缓存“上一个”值，从而避免这种情况。
