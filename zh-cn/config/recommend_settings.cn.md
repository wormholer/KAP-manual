## 推荐配置

KAP的配置文件包括几个部分：*kylin.properties*，*kylin_hive_conf.xml*，*kylin_job_conf.xml*，*kylin_job_conf_inmem.xml*。其中*kylin.properties*是KAP的主要配置参见，控制KAP的运行时行为，*kylin_hive_conf.xml*用于配置KAP与Hive交互的参数，*kylin_job_\**用于配置KAP与Hadoop集群交互的参数，其中*kylin_job_conf_inmem.xml*表示用在in-memory构建算法，*kylin_job_conf.xml*用在layer构建算法。

以下推荐配置将按照集群的规模分类，系统性能可能受其它外部系统参数影响，这里推荐仅作为经验值。

其中*Sandbox*表示用于单机sandbox虚拟机测试的环境

Small Cluster表示5个节点组成的小型Hadoop集群。

Medium Cluster表示20个节点组成的中型Hadoop集群。

Large Cluster表示50个节点组成的大型Hadoop集群。

### kylin.properties

| Properties Name                          | Sandbox    | Small Cluster | Medium Cluster | Large Cluster |
| ---------------------------------------- | ---------- | ------------- | -------------- | ------------- |
| kylin.hbase.default.compression.codec    | none       | snappy        | snappy         | snappy        |
| kylin.hbase.region.cut                   | 5          |               |                |               |
| kylin.hbase.hfile.size.gb                | 2          |               |                |               |
| kylin.hbase.region.count.min             | 1          |               |                |               |
| kylin.hbase.region.count.max             | 500        |               |                |               |
| kylin.job.concurrent.max.limit           | 10         |               |                |               |
| kylin.job.yarn.app.rest.check.interval.seconds | 10         |               |                |               |
| kylin.job.cubing.inmem.sampling.percent  | 100        |               |                |               |
| kylin.job.mapreduce.default.reduce.input.mb | 500        |               |                |               |
| kylin.job.mapreduce.max.reducer.number   | 500        |               |                |               |
| kylin.job.mapreduce.mapper.input.rows    | 1000000    |               |                |               |
| kylin.job.step.timeout                   | 7200       |               |                |               |
| kylin.cube.algorithm                     | auto       |               |                |               |
| kylin.cube.algorithm.auto.threshold      | 8          |               |                |               |
| kylin.cube.aggrgroup.max.combination     | 4096       |               |                |               |
| kylin.dictionary.max.cardinality         | 5000000    |               |                |               |
| kylin.table.snapshot.max_mb              | 300        |               |                |               |
| kylin.query.scan.threshold               | 10000000   |               |                |               |
| kylin.query.mem.budget                   | 3221225472 |               |                |               |
| kylin.query.coprocessor.mem.gb           | 3          |               |                |               |
| kylin.query.filter.derived_in.max        | 100        |               |                |               |
|                                          |            |               |                |               |
|                                          |            |               |                |               |
|                                          |            |               |                |               |



### kylin.properties for KAP Plus

| Properties Name                          | Sandbox | Small Cluster | Medium Cluster | Large Cluster |
| ---------------------------------------- | ------- | ------------- | -------------- | ------------- |
| kap.storage.columnar.conf.spark.driver.memory | 512m    |               |                |               |
| kap.storage.columnar.conf.spark.executor.memory | 512m    |               |                |               |
| kap.storage.columnar.conf.spark.yarn.am.memory | 512m    |               |                |               |
| kap.storage.columnar.conf.spark.executor.cores | 1       |               |                |               |
| kap.storage.columnar.conf.spark.executor.instances | 1       |               |                |               |
|                                          |         |               |                |               |
|                                          |         |               |                |               |
|                                          |         |               |                |               |



### kylin_hive_conf.xml

| Properties Name                          | Sandbox   | Small Cluster                            | Medium Cluster                           | Large Cluster                            |
| ---------------------------------------- | --------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| dfs.replication                          | 2         |                                          |                                          |                                          |
| hive.exec.compress.output                | true      |                                          |                                          |                                          |
| hive.auto.convert.join.noconditionaltask | true      |                                          |                                          |                                          |
| hive.auto.convert.join.noconditionaltask.size | 100000000 |                                          |                                          |                                          |
| mapreduce.map.output.compress.codec      | N/A       | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.codec | N/A       | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK     |                                          |                                          |                                          |
| mapreduce.job.split.metainfo.maxsize     | -1        |                                          |                                          |                                          |
|                                          |           |                                          |                                          |                                          |

### kylin_job_conf.xml

| Properties Name                          | Sandbox | Small Cluster                            | Medium Cluster                           | Large Cluster                            |
| ---------------------------------------- | ------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1      |                                          |                                          |                                          |
| mapreduce.map.output.compress            | true    |                                          |                                          |                                          |
| mapreduce.map.output.compress.codec      | N/A     | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true    |                                          |                                          |                                          |
| mapreduce.output.fileoutputformat.compress.codec | N/A     | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK   |                                          |                                          |                                          |
| mapreduce.job.max.split.locations        | 2000    |                                          |                                          |                                          |
| dfs.replication                          | 2       |                                          |                                          |                                          |
| mapreduce.task.timeout                   | 3600000 |                                          |                                          |                                          |

### kylin_job_conf_inmem.xml

| Properties Name                          | Sandbox   | Small Cluster                            | Medium Cluster                           | Large Cluster                            |
| ---------------------------------------- | --------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1        |                                          |                                          |                                          |
| mapreduce.map.output.compress            | true      |                                          |                                          |                                          |
| mapreduce.map.output.compress.codec      | N/A       | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true      |                                          |                                          |                                          |
| mapreduce.output.fileoutputformat.compress.codec | N/A       | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK     |                                          |                                          |                                          |
| mapreduce.job.max.split.locations        | 2000      |                                          |                                          |                                          |
| dfs.replication                          | 2         |                                          |                                          |                                          |
| mapreduce.task.timeout                   | 7200000   |                                          |                                          |                                          |
| mapreduce.map.memory.mb                  | 3072      |                                          |                                          |                                          |
| mapreduce.map.java.opts                  | -Xmx2700m |                                          |                                          |                                          |
| mapreduce.task.io.sort.mb                | 200       |                                          |                                          |                                          |
|                                          |           |                                          |                                          |                                          |
|                                          |           |                                          |                                          |                                          |



