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
#### 关于hive如何去处理一条记录的说明

```
1 Serialize把hive使用的java object转换成能写入hdfs的字节序列，或者其他系统能识别的流文件
2 Deserilize把字符串或者二进制流转换成hive能识别的java object对象。
  如：select语句会用到Serialize对象，把hdfs数据解析出来；insert语句会使用Deserilize，数据写入hdfs系统，需要把数据序列化。
```

#### 内置类型

```
Avro
ORC
RegEx
Thrift
Parquet
CSV
JsonSerDe
```

#### 自定义类型

```
--定义一个类， 继承抽象类AbstractSerDe， 实现initialize, deserialize
public class MySerDe extends AbstractSerDe {
        // params
        private List<String> columnNames = null;
        private List<TypeInfo> columnTypes = null;
        private ObjectInspector objectInspector = null;
        // seperator
        private String nullString = null;
        private String lineSep = null;
        private String kvSep = null;
    
        @Override
        public void initialize(Configuration conf, Properties tbl)
                throws SerDeException {
            // Read sep
            lineSep = "\n";
            kvSep = "=";
            nullString = tbl.getProperty(Constants.SERIALIZATION_NULL_FORMAT, "");
    
            // Read Column Names
            String columnNameProp = tbl.getProperty(Constants.LIST_COLUMNS);
            if (columnNameProp != null && columnNameProp.length() > 0) {
                columnNames = Arrays.asList(columnNameProp.split(","));
            } else {
                columnNames = new ArrayList<String>();
            }
    
            // Read Column Types
            String columnTypeProp = tbl.getProperty(Constants.LIST_COLUMN_TYPES);
            // default all string
            if (columnTypeProp == null) {
                String[] types = new String[columnNames.size()];
                Arrays.fill(types, 0, types.length, Constants.STRING_TYPE_NAME);
                columnTypeProp = StringUtils.join(types, ":");
            }
            columnTypes = TypeInfoUtils.getTypeInfosFromTypeString(columnTypeProp);
    
            // Check column and types equals
            if (columnTypes.size() != columnNames.size()) {
                throw new SerDeException("len(columnNames) != len(columntTypes)");
            }
    
            // Create ObjectInspectors from the type information for each column
            List<ObjectInspector> columnOIs = new ArrayList<ObjectInspector>();
            ObjectInspector oi;
            for (int c = 0; c < columnNames.size(); c++) {
                oi = TypeInfoUtils
                        .getStandardJavaObjectInspectorFromTypeInfo(columnTypes
                                .get(c));
                columnOIs.add(oi);
            }
            objectInspector = ObjectInspectorFactory
                    .getStandardStructObjectInspector(columnNames, columnOIs);
    
        }
    
        @Override
        public Object deserialize(Writable wr) throws SerDeException {
            // Split to kv pair
            if (wr == null)
                return null;
            Map<String, String> kvMap = new HashMap<String, String>();
            Text text = (Text) wr;
            for (String kv : text.toString().split(lineSep)) {
                String[] pair = kv.split(kvSep);
                if (pair.length == 2) {
                    kvMap.put(pair[0], pair[1]);
                }
            }
    
            // Set according to col_names and col_types
            ArrayList<Object> row = new ArrayList<Object>();
            String colName = null;
            TypeInfo type_info = null;
            Object obj = null;
            for (int i = 0; i < columnNames.size(); i++) {
                colName = columnNames.get(i);
                type_info = columnTypes.get(i);
                obj = null;
                if (type_info.getCategory() == ObjectInspector.Category.PRIMITIVE) {
                    PrimitiveTypeInfo p_type_info = (PrimitiveTypeInfo) type_info;
                    switch (p_type_info.getPrimitiveCategory()) {
                    case STRING:
                        obj = StringUtils.defaultString(kvMap.get(colName), "");
                        break;
                    case LONG:
                    case INT:
                        try {
                            obj = Long.parseLong(kvMap.get(colName));
                        } catch (Exception e) {
                        }
                    }
                }
                row.add(obj);
            }    
            return row;
        }    
        @Override
        public ObjectInspector getObjectInspector() throws SerDeException {
            return objectInspector;
        }    
        @Override
        public SerDeStats getSerDeStats() {
            return null;
        }    
        @Override
        public Class<? extends Writable> getSerializedClass() {
            return Text.class;
        }    
        @Override
        public Writable serialize(Object arg0, ObjectInspector arg1)
                throws SerDeException {
            return null;
        }    
    }
```

```
--添加自定义的SerDe类的jar包
hive > add jar MySerDe.jar
```

```
CREATE EXTERNAL TABLE IF NOT EXISTS teacher ( 
          id BIGINT, 
          name STRING,
          age INT)
    ROW FORMAT SERDE 'com.coder4.hive.MySerDe'
    STORED AS TEXTFILE
    LOCATION '/usr/hive/text/'
```
