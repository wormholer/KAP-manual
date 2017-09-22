##  Pushdown to Native SparkSQL

KAP supports Query Pushdown since 2.4. If the query is Cube incapable, KAP will route the query to the pushdown query engine. 

#### Turn on Query Pushdown

KAP Plus has embedded Spark engine, no 3rd party dependency needed for query pushdown.

Query pushdown is turned off by default. To turn on it, remove the comment character # ahead of the configuration item `kylin.query.pushdown.runner-class-name=io.kyligence.kap.storage.parquet.adhoc.PushDownRunnerSparkImp` in file `kylin.properties` to bring it into  effect. 

With query pushdown turned on, queries that cannot get results from Cubes will be redirected to Spark SQL in default. 

For non KAP Plus or other engines needed, you can also configure it manually, and choose Hive as the default engine to be redirected. Please refer to [Pushdown to 3rd Party SparkSQL](pushdown_sparksql.en.md)／[Pushdown to Impala](pushdown_impala.en.md)／[Pushdown to Hive](pushdown_hive.en.md)

#### Verify Query Pushdown

After turning on query pushdown, all tables you have synchronized will be shown without building Cubes. When you submit a query, you can find *Pushdown* in the *Query Engine* item below *Status*, if query pushdown works correctly.

![](images/query_pushdown_enable.png)
