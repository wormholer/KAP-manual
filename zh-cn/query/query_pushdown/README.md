# 查询下压

KAP从2.4版本开始支持查询下压。当用户需要执行定制的Cube无法满足的查询时，可以使用查询下压，将该查询重定向至Spark SQL或者Hive，从而在查询的执行时间与灵活程度之间做一个权衡折中，获取更理想的使用体验。

继续阅读：

[开启 Query Pushdown](pushdown.cn.md)

[配置 Spark 查询引擎](pushdown_spark.cn.md)

[配置 Impala 查询引擎](pushdown_impala.cn.md)

