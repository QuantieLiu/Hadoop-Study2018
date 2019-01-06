#### 

```  
--模糊搜索表
show tables like '*name*';
--查看表结构信息
desc formatted table_name;
desc table_name;
--查看分区信息
show partitions table_name;
```  

```  
--从文件加载数据进表(OVERWRITE覆盖,追加不需要OVERWRITE关键字)
LOAD DATA LOCAL INPATH 'dim_csl_rule_config.txt' OVERWRITE into table dim.dim_csl_rule_config;
--从查询语句给table插入数据
INSERT OVERWRITE TABLE test_h02_click_log PARTITION(dt) select *
from stage.s_h02_click_log where dt='2014-01-22' limit 100;
导出数据到文件
insert overwrite directory '/tmp/csl_rule_cfg' select a.* from dim.dim_csl_rule_config a;
hive -e "select day_id,pv,uv,ip_count,click_next_count,second_bounce_rate,return_visit,pg_type from tmp.tmp_h02_click_log_baitiao_ag_sum where day_id in ('2014-03-06','2014-03-07','2014-03-08','2014-03-09','2014-03-10');"> /home/jrjt/testan/baitiao.dat;
```  

9.查询显示列名 及 行转列显示
  set hive.cli.print.header=true; // 打印列名
  set hive.cli.print.row.to.vertical=true; // 开启行转列功能, 前提必须开启打印列名功能
  set hive.cli.print.row.to.vertical.num=1; // 设置每行显示的列数

10.查看表文件大小,下载文件到某个目录,显示多少行到某个文件
   dfs -du hdfs://BJYZH3-HD-JRJT-4137.jd.com:54310/user/jrjt/warehouse/stage.db/s_h02_click_log;
   dfs -get /user/jrjt/warehouse/ods.db/o_h02_click_log_i_new/dt=2014-01-21/000212_0 /home/jrjt/testan/;
   head -n 1000 文件名 > 文件名

11.杀死某个任务  不在hive shell中执行
   hadoop job -kill job_201403041453_58315
