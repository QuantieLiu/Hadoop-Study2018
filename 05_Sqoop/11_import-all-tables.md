将一组表从RDBMS导入到HDFS。来自每个表的数据存储在HDFS的单独目录中

### 应用示例

```
import-all-tables --connect jdbc:mysql://db.foo.com/corp


sqoop import-all-tables -m 12 --connect "jdbc:mysql://sandbox.hortonworks.com:3306/retail_db" 
--username -P --warehouse-dir ./HDPCD/sqoop_import --driver com.mysql.jdbc.Driver

```

#### 使用条件

```
1.每个表必须具有主键或使用--autoreset-to-one-mapper选项。
2.导入每张表的所有列。
3.使用默认拆分列，不能使用WHERE。
```

### 参数

#### 通用参数

```
--connect <jdbc-uri>：指定JDBC连接字符串。
--connection-manager <class-name>：指定要使用的连接管理器类。
--driver <class-name>：手动指定要使用的JDBC驱动程序类。
--hadoop-mapred-home <dir>：覆盖$ HADOOP_MAPRED_HOME。
--help：打印使用说明。
--password-file：为包含认证密码的文件设置路径。
-P：从控制台读取密码。
--password <password>：设置验证密码。
--username <username>：设置验证用户名。
--verbose：在运行时打印更多信息。
--connection-param-file <filename>：提供连接参数的可选属性文件。
--relaxed-isolation：将mapper的连接事务隔离设置为只读。
```

#### 导入控制参数

```
--as-avrodatafile：将数据导入Avro数据文件。
--as-sequencefile：将数据导入到SequenceFiles。
--as-textfile：以纯文本形式导入数据（默认）。
--as-parquetfile：将数据导入Parquet文件。
--direct：使用direct快速导入。
--inline-lob-limit <n>：设置内联LOB的最大大小。
-m,--num-mappers <n>：使用n个mapper任务并行导入。
--warehouse-dir <dir>：表目的地的HDFS父级目录。
-z,--compress：启用压缩。
--compression-codec <c>：使用Hadoop编解码器（默认gzip）。
--exclude-tables <tables>：逗号分隔的表格列表，以便从导入过程中排除。
--autoreset-to-one-mapper：如果表没有主键，导入时使用一个mapper执行。
```

这些参数的使用方式和sqoop-import工具的使用方式一样，
但是--table、--split-by、--columns和--where参数不能用于sqoop-import-all-tables工具。
--exclude-tables参数只能在sqoop-import-all-tables工具中使用。

#### 输出格式参数

```
--enclosed-by <char>：设置必需的字段包围字符。
--escaped-by <char>：设置转义字符。
--fields-terminated-by <char>：设置字段分隔符。
--lines-terminated-by <char>：设置行尾字符。
--mysql-delimiters：使用MySQL的默认分隔符集：fields：, lines：\n escaped-by：\ optional-enclosed-by：'。
--optionally-enclosed-by <char>：设置字段包含字符。
```

#### 输入分析参数

```
--input-enclosed-by <char>：设置必需的字段封闭器。
--input-escaped-by <char>：设置输入转义字符。
--input-fields-terminated-by <char>：设置输入字段分隔符。
--input-lines-terminated-by <char>：设置输入的行尾字符。
--input-optionally-enclosed-by <char>：设置字段包含字符。
```

#### Hive参数

```
--hive-home <dir>：覆盖 $HIVE_HOME。
--hive-import：将表导入Hive（如果没有设置，则使用Hive的默认分隔符。）。
--hive-overwrite：覆盖Hive表中的现有数据。。
--create-hive-table：如果设置，则作业将失败，如果目标配置单元表存在。默认情况下，该属性为false。
--hive-table <table-name>：设置导入到Hive时要使用的表名。
--hive-drop-import-delims：导入到Hive时，从字符串字段中 删除\ n，\ r和\ 01。
--hive-delims-replacement：在导入到Hive时，将字符串字段中的\ n，\ r和\ 01 替换为用户定义的字符串。
--hive-partition-key：分区的配置单元字段的名称被打开
--hive-partition-value <v>：字符串值，用作此作业中导入配置单元的分区键。
--map-column-hive <map>：覆盖从SQL类型到配置列的Hive类型的默认映射。如果在此参数中指定逗号，请使用URL编码的键和值，例如，使用DECIMAL（1％2C％201）而不是DECIMAL（1，1）。
```

#### 代码生成参数

```
--bindir <dir>：编译对象的输出目录。
--jar-file <file>：禁用代码生成; 使用指定的jar。
--outdir <dir>：生成代码的输出目录。
--package-name <name>：将自动生成的类放入此包中。
```


