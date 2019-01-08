
#### POM配置

```
<properties>
	<!-- 需使用中文，避免乱码，设定编码格式 -->
 	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
 	<maven.compiler.encoding>UTF-8</maven.compiler.encoding>
</properties> 
```

```
  	<dependency>
  		<groupId>org.apache.hive</groupId>
  		<artifactId>hive-exec</artifactId>
  		<version>1.2.1</version>
  		<scope>provided</scope>
  	</dependency>
```

#### 继承
<li> 继承org.apache.hadoop.hive.ql.exec.UDF
<li> 至少实现evaluate()方法
<li> 可接受的参数个数，数据类型及其返回值的数据类型 不受限制

```
public class UDFZodiac extends UDF{

  public String evaluate(Calendar bday){
		return this.evaluate( month: bday.get(Calendar.MONTH)+1,bday.get(Calendar.DAY_OF_MONTH));
	} 
	
	public String evaluate(String bday){
		Date date = null;
		try{
			date= df.parse(bday);
		}catch(Exception ex){
			return null;
		}
		Calendar calendar = Calendar.getInstance();
		calendar.setTime(date);
		return this.evaluate(month:calendar.get(Calendar.MONTH)+1,calendar.get(Calendar.DAY_OF_MONTH));
	}
}
```
