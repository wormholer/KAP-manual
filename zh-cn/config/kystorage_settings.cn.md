## KyStorage参数配置
KAP Plus将所有的cube数据都保存在KyStorage，一种基于HDFS的列式存储之上。在查询时，KAP Plus使用Spark (http://spark.apache.org，具体而言是Spark on Yarn模式)来读取cube，并做可能的存储层预聚合。一个或者多个Spark executor作为长进程启动，用来接收可能的cube访问。对于生产环境部署，请仔细阅读本文档并保证您的executors已经正确配置。另外，我们使用grpc(http://www.grpc.io/ )来连接KAP的查询服务器和Spark，必要时，需要增加grpc的配置。

### 配置Spark参数

KAP Plus将Spark的二进制包和配置文件打包至`KAP_HOME/spark/`目录。KAP Plus使用spark-submit脚本来启动executor。理论上可以直接在`KAP_HOME/spark/conf/`路径下按照http://spark.apache.org/docs/latest/configuration.html来修改Spark自带的所有配置。但是我们不推荐这种方式，因为从运维的便捷性考虑所有KAP相关的配置都应限制在`KAP_HOME/conf/`路径下。

我们建议用户通过修改kylin.properties配置文件中对应的参数值来配置Spark参数。这些参数大致分为两个类别：环境变量和Spark属性。

### 环境变量

这一类配置与`SPARK_HOME/conf/spark-env.sh`中的配置项相关，在 http://spark.apache.org/docs/latest/configuration.html#environment-variables 中有详细的介绍。一个典型的例子是HADOOP_CONF_DIR参数，它为Spark指定读取Hadoop相关配置的路径。在kylin.properties中有如下配置：

```
kap.storage.columnar.env.HADOOP_CONF_DIR=/etc/hadoop/conf
```

如例子所示，我们只要在任何Spark环境变量前加上前缀*kap.storage.columnar.env*，就能在kylin.properties中指定并覆盖相应的Spark环境变量。

### Spark属性

这一类配置与`SPARK_HOME/conf/spark-defaults.conf`中的配置项相关，在 http://spark.apache.org/docs/latest/configuration.html#spark-properties 中有详细的介绍。一个典型的例子是spark.executor.instances参数，它指定运行的executor数量。在默认的`kylin.properties`中有如下配置（虽然被注释掉了）：

  ```
kap.storage.columnar.conf.spark.executor.instances=4
  ```

这个配置项告诉Spark应当为KAP启动4个executor。如例子所示，只要在Spark属性前加上前缀*kap.storage.columnar.conf*，就能在kylin.properties中指定并覆盖相应的Spark属性。

| Property Name                            | Default | Meaning                                  |
| ---------------------------------------- | ------- | ---------------------------------------- |
| kap.storage.columnar.conf.spark.driver.memory | 1G      | Amount of memory to use for the driver process, i.e. where SparkContext is initialized. (e.g. `1g`, `2g`). *Note:* In client mode, this config must not be set through the `SparkConf` directly in your application, because the driver JVM has already started at that point. Instead, please set this through the `--driver-memory` command line option or in your default properties file. |
| kap.storage.columnar.conf.spark.executor.memory | 1G      | Amount of memory to use per executor process (e.g. `2g`, `8g`). |
| kap.storage.columnar.conf.spark.executor.cores | 1       | The number of cores to use on each executor. In standalone and Mesos coarse-grained modes, setting this parameter allows an application to run multiple executors on the same worker, provided that there are enough cores on that worker. Otherwise, only one executor per application will run on each worker. |
| kap.storage.columnar.conf.spark.executor.instances | 2       | The number of executors for static allocation. With `spark.dynamicAllocation.enabled`, the initial set of executors will be at least this large. |

### 配置建议

在Spark的默认配置下，启动的executor数量(*spark.executor.instances*)为2，每个executor的CPU核数量(*spark.executor.cores*)为1，为每个executor分配的内存(*spark.executor.memory*)为1GB，为driver分配的内存(*spark.driver.memory*)为1GB。显然这样的配置对于严肃的KAP部署来说太小了。

最佳的配置取决于具体的集群硬件配置。大多数情况下我们建议为每个HDFS或者Yarn节点上分配一个executor。每个executor的CPU核数量应该在2~5个之间。用户可以根据数据量的大小灵活地调节为executor和driver分配的内存大小。以下是一个示例集群上的配置，该集群有4个节点，每个节点有24个CPU核以及64GB的内存。

  ```
kap.storage.columnar.conf.spark.driver.memory=8192m
kap.storage.columnar.conf.spark.executor.memory=8192m
kap.storage.columnar.conf.spark.executor.cores=5
kap.storage.columnar.conf.spark.executor.instances=4
  ```

## 调整grpc参数

### 修改最大返回值（从KAP 2.2开始）

由于https://github.com/grpc/grpc-java/issues/1676， grpc默认的最大的返回结果的大小被减小至4M。为了避免类似“Caused by: io.grpc.StatusRuntimeException: INTERNAL: Frame size 108427384 exceeds maximum: 104857600”这样的异常，在KAP的默认配置中，我们把这个值增加到了128MB。如果这个配置还是不够大，可以考虑在kylin.properties中做如下配置：


```
kap.storage.columnar.grpc-max-response-size=SIZE_IN_BYTES
```
