#### 初始化hive 元数据

```
ERROR:"Unable to instantiate org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient"
```

<li>将metastore_db文件夹改名或者干脆删除

突然离线，易造成hive没能来得及删除自动创建的metastore_db文件夹(~home/bin/metastore_db),这时再次用hive命令进入，则会产生如下报错。
解决办法：将metastore_db文件夹改名或者干脆删除
1)#rm -r metastore_db/
2)# mv metastore_db/ metastore_db1/

```
Hive单机启动遇到metastore 未初始化错误，
Exception in thread "main" java.lang.RuntimeException: Hive metastore database is not initialized. Please use schematool (e.g. ./schematool -initSchema -dbType ...) to create the schema. If needed, don't forget to include the option to auto-create the underlying database in your JDBC connection string (e.g. ?createDatabaseIfNotExist=true for mysql)
按照提示运行：
schematool -initSchema -dbType derby
接着出现如下错误：

Starting metastore schema initialization to 2.0.0
Initialization script hive-schema-2.0.0.derby.sql
Error: FUNCTION 'NUCLEUS_ASCII' already exists. (state=X0Y68,code=30000)
org.apache.hadoop.hive.metastore.HiveMetaException: Schema initialization FAILED! Metastore state would be inconsistent !!

*** schemaTool failed ***

发现bin目录下已存在metastore_db的文件夹，删除后初始化成功
```

<li>初始化derby元数据schematool -initSchema -dbType derby

```
[11899517@bigdata4 metastore_db]$ ls
dbex.lck  db.lck  log  README_DO_NOT_TOUCH_FILES.txt  seg0  service.properties  tmp
[11899517@bigdata4 metastore_db]$ cd ..
[11899517@bigdata4 bin]$ schematool -dbType mysql -initSchema
Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
Metastore connection User:	 APP
Starting metastore schema initialization to 1.2.0
Initialization script hive-schema-1.2.0.mysql.sql
Error: Syntax error: Encountered "<EOF>" at line 1, column 64. (state=42X01,code=30000)
org.apache.hadoop.hive.metastore.HiveMetaException: Schema initialization FAILED! Metastore state would be inconsistent !!
*** schemaTool failed ***
[11899517@bigdata4 bin]$ pwd
/mnt/home/11899517/sqoop/bin
[11899517@bigdata4 bin]$ ls
configure-sqoop      sqoop                    sqoop-export             sqoop-job             sqoop_test.java
configure-sqoop.cmd  sqoop.cmd                sqoop-help               sqoop-list-databases  sqoop-version
derby.log            sqoop-codegen            sqoop-import             sqoop-list-tables     start-metastore.sh
metastore_db         sqoop-create-hive-table  sqoop-import-all-tables  sqoop-merge           stop-metastore.sh
metastore_db1        sqoop-eval               sqoop-import-mainframe   sqoop-metastore
[11899517@bigdata4 bin]$ rm -rf metastore_db
[11899517@bigdata4 bin]$ ls
configure-sqoop      sqoop.cmd                sqoop-help               sqoop-list-databases  sqoop-version
configure-sqoop.cmd  sqoop-codegen            sqoop-import             sqoop-list-tables     start-metastore.sh
derby.log            sqoop-create-hive-table  sqoop-import-all-tables  sqoop-merge           stop-metastore.sh
metastore_db1        sqoop-eval               sqoop-import-mainframe   sqoop-metastore
sqoop                sqoop-export             sqoop-job                sqoop_test.java
[11899517@bigdata4 bin]$ schematool -initSchema -dbType derby
Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
Metastore connection User:	 APP
Starting metastore schema initialization to 1.2.0
Initialization script hive-schema-1.2.0.derby.sql
Initialization script completed
schemaTool completed
[11899517@bigdata4 bin]$ 
```
