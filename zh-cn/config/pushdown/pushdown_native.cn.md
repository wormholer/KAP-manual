## 原生SparkSQL查询下压
KAP从2.4版本开始支持查询下压。当用户需要执行定制的Cube无法满足的查询时，可以使用查询下压，将该查询下压至Spark SQL／Hive／Impala，从而在查询中获取更灵活的使用体验。

### 启动开启

KAP Plus内置Spark执行引擎，因此无需依赖外部下压引擎。

查询下压功能默认未开启。如果用户需要开启查询下压，需要在`kylin.properties`文件中删去`kylin.query.pushdown.runner-class-name=io.kyligence.kap.storage.parquet.adhoc.PushDownRunnerSparkImpl`这一配置项前的**注释符号**使其生效。

非KAP Plus或者需要其他下压引擎，可以参考[外部SparkSQL集成](pushdown_sparksql.cn.md)／[Impala集成](pushdown_impala.cn.md)／[Hive集成](pushdown_hive.cn.md)。

### 验证查询下压

开启查询下压后，所有同步的数据表将对用户可见，而无需构建相应的Cube。用户在提交查询时，若查询下压发挥作用，则状态下方的查询引擎条目里，会显示Push down。

![](images/query_pushdown_enable.png)
