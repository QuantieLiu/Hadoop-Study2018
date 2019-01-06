
#### NoViableAltException

``` 
NoViableAltException(313@[192:1: tableName : (db= identifier DOT tab= identifier -> ^( TOK_TABNAME $db $tab) |tab= identifier -> ^( TOK_TABNAME $tab) );])
	at org.antlr.runtime.DFA.noViableAlt(DFA.java:158)
	at org.antlr.runtime.DFA.predict(DFA.java:144)
	at org.apache.hadoop.hive.ql.parse.HiveParser_FromClauseParser.tableName(HiveParser_FromClauseParser.java:4747)
	at org.apache.hadoop.hive.ql.parse.HiveParser.tableName(HiveParser.java:45945)
	at org.apache.hadoop.hive.ql.parse.HiveParser.createTableStatement(HiveParser.java:5032)
	at org.apache.hadoop.hive.ql.parse.HiveParser.ddlStatement(HiveParser.java:2643)
	at org.apache.hadoop.hive.ql.parse.HiveParser.execStatement(HiveParser.java:1653)
	at org.apache.hadoop.hive.ql.parse.HiveParser.statement(HiveParser.java:1112)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:202)
	at org.apache.hadoop.hive.ql.parse.ParseDriver.parse(ParseDriver.java:166)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:397)
	at org.apache.hadoop.hive.ql.Driver.compile(Driver.java:309)
	at org.apache.hadoop.hive.ql.Driver.compileInternal(Driver.java:1145)
	at org.apache.hadoop.hive.ql.Driver.runInternal(Driver.java:1193)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1082)
	at org.apache.hadoop.hive.ql.Driver.run(Driver.java:1072)
	at org.apache.hadoop.hive.cli.CliDriver.processLocalCmd(CliDriver.java:213)
	at org.apache.hadoop.hive.cli.CliDriver.processCmd(CliDriver.java:165)
	at org.apache.hadoop.hive.cli.CliDriver.processLine(CliDriver.java:376)
	at org.apache.hadoop.hive.cli.CliDriver.executeDriver(CliDriver.java:736)
	at org.apache.hadoop.hive.cli.CliDriver.run(CliDriver.java:681)
	at org.apache.hadoop.hive.cli.CliDriver.main(CliDriver.java:621)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:221)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:136)
FAILED: ParseException line 1:36 cannot recognize input near ''bigdata.weblog'' '(' ''time_tag'' in table name
``` 



