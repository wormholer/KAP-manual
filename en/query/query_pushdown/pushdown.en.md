##  Ture on Query Pushdown

Query pushdown is turned off by default. To turn on it, remove the comment character # ahead of the configuration item `kylin.query.pushdown.runner-class-name=io.kyligence.kap.storage.parquet.adhoc.PushDownRunnerSparkImp` in file `kylin.properties` to bring it into  effect. With query pushdown turned on, queries that cannot get results from Cubes will be redirected to Spark SQL in default. You can also configure it manually, and choose Hive as the default engine to be redirected. Please refer to [Important Configurations](../config/basic_settings.en.md) for more configuration settings.

After turning on query pushdown, all tables you have synchronized will be shown without building Cubes. When you submit a query, you can find *Pushdown* in the *Query Engine* item below *Status*, if query pushdown works correctly.

![](query_pushdown_images/query_pushdown_enable.png)
