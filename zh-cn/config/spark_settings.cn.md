## 介绍
在默认情况下，KAP将所有的cube都以一种列式存储格式保存在HDFS上。在查询时，KAP使用Spark (http://spark.apache.org, 具体来说我们使用Spark on yarn模式)来读取cube，并做可能的存储层预聚合。一个或者多个Spark executor作为长进程启动，用来接收可能的cube访问。对于生产环境部署，您应当仔细阅读本文档并保证您的executors被正确配置。

## 调整参数

KAP打包了所有spark的二进制包和配置文件，这些都位于KAP_HOME/spark目录。KAP使用spark-submit脚本来启动所有的executor，因此在理论上您可以直接到KAP_HOME/spark/conf并按照 http://spark.apache.org/docs/latest/configuration.html 来修改spark自带的所有配置。但是这并不是我们推荐的方式，因为从运维的便捷性考虑我们推荐所有的KAP相关的设置都应该在KAP_HOME/conf目录中。



### 环境变量

这一类设置与SPARK_HOME/conf/spark-env.sh中的设置项相关，在 http://spark.apache.org/docs/latest/configuration.html#environment-variables 中有详细的介绍。一个典型的例子是`HADOOP_CONF_DIR`，它可以用来告诉Spark该从哪里读到hadoop相关的设置。在默认的`kylin.properties`中有如下配置：

> kap.storage.columnar.env.HADOOP_CONF_DIR=/etc/hadoop/conf

如这个例子所示，我们只要在任何的环境变量前面加上`kap.storage.columnar.env`,就能在`kylin.properties`中指定一个Spark的环境变量。

### Spark属性

这一类设置与SPARK_HOME/conf/spark-defaults.conf中的设置项相关，在 http://spark.apache.org/docs/latest/configuration.html#spark-properties 中有详细的介绍。一个典型的例子是`spark.executor.instances`，它可以用来指定应该运行多少个executor。在默认的`kylin.properties`中有如下配置（虽然被注释掉了）：

> ```
> kap.storage.columnar.conf.spark.executor.instances=4
> ```

这个配置项告诉Spark应当为KAP启动4个executor。如这个例子所示，只要在Spark属性前面加上`kap.storage.columnar.conf`，我们就能够在`kylin.properties`中指定一个Spark属性。

## 配置建议

在Spark的默认配置下，启动的executor数量(`spark.executor.instances`)为2，每个executor的CPU核数量(`spark.executor.cores`)是1，每个executor的内存(`spark.executor.memory`)是1G，driver的内存(`spark.driver.memory`)是1G。显然这样的配置对于严肃的KAP部署来说太小了。

最佳的配置取决于具体的集群硬件配置。在大多数情况下我们建议在每台hdfs或者yarn节点上安排一个executor。每个executor的CPU核数量应该在2~5个之间。用户可以根据数据量的大小灵活地调节executor和driver的内存大小。以下是一个示例集群上的配置，该集群有4个节点，每台有24个CPU核，64G的内存。

> ```
> kap.storage.columnar.conf.spark.driver.memory=8192m
> kap.storage.columnar.conf.spark.executor.memory=8192m
> kap.storage.columnar.conf.spark.executor.cores=5
> kap.storage.columnar.conf.spark.executor.instances=4
> ```
