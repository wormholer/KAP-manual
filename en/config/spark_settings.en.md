## Introduction
By default, KAP stores all cubes in a columar format on HDFS. When querying, KAP uses Spark (http://spark.apache.org, specifically we use Spark on yarn mode) for cube reading and possibly storage pre-aggregations.  One or more Spark executors are started as long-running processes to receive any cube visit requests. For production deployments you should go through this page to make sure your spark executors are well configured.

## Tune spark parameters

KAP ships with all necessary spark assemblies and configurations, which reside in KAP_HOME/spark folder. KAP uses the spark-submit script to launch all the executors, so theoridically you can directly go to KAP_HOME/spark/conf to make changes on spark-shipped configuration as http://spark.apache.org/docs/latest/configuration.html explains. However it's NOT the recommended way because for maintainence convenicence we suggest to keep all KAP configurations staying in KAP_HOME/conf. 

Our solution is to allow users to change spark configurations in `kylin.properties`.  Typically there're two major categories of configurations for Spark: environment variables and  Spark properties:

### environment variables

This category relates to SPARK_HOME/conf/spark-env.sh, there's detailed explanation in http://spark.apache.org/docs/latest/configuration.html#environment-variables. A typical example is `HADOOP_CONF_DIR`, which tells Spark where the hadoop configurations reside. By default we have a config entry in `kylin.properties`:

> ```
> kap.storage.columnar.env.HADOOP_CONF_DIR=/etc/hadoop/conf
> ```

As the example illustrates, by prefixing the Spark environment variable with `kap.storage.columnar.env`, we can specify any Spark environment variable in `kylin.properties`.

### Spark properties

This categoy relates to SPARK_HOME/conf/spark-defaults.conf, there's detailed explanation in http://spark.apache.org/docs/latest/configuration.html#spark-properties. A typical example is `spark.executor.instances`, which specifies how many executors should be there. By default we have (although it's commented out) a config entry in `kylin.properties`:

> ```
> kap.storage.columnar.conf.spark.executor.instances=4
> ```

The config entry tells Spark to launch 4 executors for KAP. As the example illustrates, by prefixing the Spark property with `kap.storage.columnar.conf` we can specify any Spark property in `kylin.properties`

## Advices on configurations

By Spark's default configuration, the number of executors to start (`spark.executor.instances`) is 2, the number of cores for each executor (`spark.executor.cores`) is 1, the memory per executor (`spark.executor.memory`) is 1G, the memory for driver (`spark.driver.memory`) is 1G. Obviously it's not enough for serious KAP deployments. 

The optimal configuration depends on your cluster hardware specifications. In most cases we suggest one executors per hdfs/yarn server. The number of cores for each executor could be somewhere between 2~5. Users can flexibly adjust memory for executor and memory depending on data scale. Here's a sample configuration for a 4-node cluster, where each server has 24 cores and 64G RAM:

> ```
> kap.storage.columnar.conf.spark.driver.memory=8192m
> kap.storage.columnar.conf.spark.executor.memory=8192m
> kap.storage.columnar.conf.spark.executor.cores=5
> kap.storage.columnar.conf.spark.executor.instances=4
> ```

