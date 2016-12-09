## Recommended Configurations for KAP v2.2

The configuration files of KAP include following files: *kylin.properties*, *kylin_hive_conf.xml*, *kylin_job_conf.xml*, *kylin_job_conf_inmem.xml*. Among those files, *kylin.properties* is a major configuration parameter to control KAP's running behavior; *kylin_hive_conf.xml* is applied to configure parameters of interaction between KAP and Hive; *kylin_job_conf.xml* is applied to configure parameters of interaction between KAP and Hadoop cluster; *kylin_job_conf_inmem.xml is applied on in-memory* algorithm, *kylin_job_conf.xml* is applied on layer algorithm.

The following recommended configurations are classified according to the size of the cluster, system performance could be influenced by other external system parameters. Here our recommending configurations are based on experience.

*Sandbox* refers the testing environment for single machine sandbox virtual machine, dual core, 10GB internal storage, 10GB hard disk.

*Prod* represents recommended configuration for the production environment, usually, the Hadoop cluster consisting of at least 5 nodes, single machine 32 core, 128GB internal storage, 20TB hard disk.

> Tip: KAP v2.2 provides sample configurations of *Sandbox* and *Prod* profiles. *Sandbox* is used as default profile and can be switched by rebuild soft link:
>
> ```bash
> cd $KYLIN_HOME/conf
>
> # Use sandbox(min) profile
> ln -sfn profile_min profile
>
> # Or use production(prod) profile
> ln -sfn profile_prod profile
> ```

### kylin.properties

| Properties Name                          | Sandbox    | Prod       |
| ---------------------------------------- | ---------- | ---------- |
| kylin.storage.hbase.compression-codec    | none       | snappy     |
| kylin.storage.hbase.region-cut-gb        | 1          | 5          |
| kylin.storage.hbase.hfile-size-gb        | 1          | 2          |
| kylin.storage.hbase.min-region-count     | 1          | 1          |
| kylin.storage.hbase.max-region-count     | 100        | 500        |
| kylin.storage.hbase.coprocessor-mem-gb   | 3          | 3          |
| kylin.job.max-concurrent-jobs            | 10         | 20         |
| kylin.job.sampling-percentage            | 100        | 100        |
| kylin.job.step.timeout                   | 7200       | 7200       |
| kylin.engine.mr.yarn-check-interval-seconds | 10         | 10         |
| kylin.engine.mr.reduce-input-mb          | 100        | 500        |
| kylin.engine.mr.max-reducer-number       | 100        | 500        |
| kylin.engine.mr.mapper-input-rows        | 200000     | 1000000    |
| kylin.cube.algorithm                     | auto       | auto       |
| kylin.cube.algorithm.layer-or-inmem-threshold | 8          | 8          |
| kylin.cube.aggrgroup.max-combination     | 4096       | 4096       |
| kylin.dictionary.max.cardinality         | 5000000    | 5000000    |
| kylin.snapshot.max-mb                    | 300        | 300        |
| kylin.query.scan-threshold               | 10000000   | 10000000   |
| kylin.query.memory-budget-bytes          | 3221225472 | 3221225472 |
| kylin.query.derived-filter-translation-threshold | 100        | 100        |


### kylin.properties for KAP Plus

| Properties Name                          | Sandbox | Prod   |
| ---------------------------------------- | ------- | ------ |
| kap.storage.columnar.spark-conf.spark.driver.memory | 512m    | 8192m  |
| kap.storage.columnar.spark-conf.spark.executor.memory | 512m    | 4096m  |
| kap.storage.columnar.spark-conf.spark.yarn.am.memory | 512m    | 4096m  |
| kap.storage.columnar.spark-conf.spark.executor.cores | 1       | 5      |
| kap.storage.columnar.spark-conf.spark.executor.instances | 1       | 4      |
| kap.storage.columnar.page-compression    |         | SNAPPY |
| kap.storage.columnar.ii-spill-threshold-mb | 128     | 512    |




### kylin_hive_conf.xml

| Properties Name                          | Sandbox   | Prod                                     |
| ---------------------------------------- | --------- | ---------------------------------------- |
| dfs.replication                          | 2         | 2                                        |
| hive.exec.compress.output                | true      | true                                     |
| hive.auto.convert.join.noconditionaltask | true      | true                                     |
| hive.auto.convert.join.noconditionaltask.size | 100000000 | 100000000                                |
| mapreduce.map.output.compress.codec      | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.codec | N/A       | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK     | BLOCK                                    |
| mapreduce.job.split.metainfo.maxsize     | -1        | -1                                       |

### kylin_job_conf.xml

| Properties Name                          | Sandbox | Prod                                     |
| ---------------------------------------- | ------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1      | -1                                       |
| mapreduce.map.output.compress            | true    | true                                     |
| mapreduce.map.output.compress.codec      | N/A     | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true    | true                                     |
| mapreduce.output.fileoutputformat.compress.codec | N/A     | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK   | BLOCK                                    |
| mapreduce.job.max.split.locations        | 2000    | 2000                                     |
| dfs.replication                          | 2       | 2                                        |
| mapreduce.task.timeout                   | 3600000 | 3600000                                  |

### kylin_job_conf_inmem.xml

| Properties Name                          | Sandbox  | Prod                                     |
| ---------------------------------------- | -------- | ---------------------------------------- |
| mapreduce.job.split.metainfo.maxsize     | -1       | -1                                       |
| mapreduce.map.output.compress            | true     | true                                     |
| mapreduce.map.output.compress.codec      | N/A      | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress | true     | true                                     |
| mapreduce.output.fileoutputformat.compress.codec | N/A      | org.apache.hadoop.io.compress.SnappyCodec |
| mapreduce.output.fileoutputformat.compress.type | BLOCK    | BLOCK                                    |
| mapreduce.job.max.split.locations        | 2000     | 2000                                     |
| dfs.replication                          | 2        | 2                                        |
| mapreduce.task.timeout                   | 7200000  | 7200000                                  |
| mapreduce.map.memory.mb                  | 1024     | 4096                                     |
| mapreduce.map.java.opts                  | -Xmx700m | -Xmx3700m                                |
| mapreduce.task.io.sort.mb                | 100      | 200                                      |
