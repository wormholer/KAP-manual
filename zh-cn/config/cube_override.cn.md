## Cube配置重定义

以下参数可以在Cube级别进行重定义，重定义的值将覆盖系统级别默认值。

### 覆盖kylin.properties中参数

```
# Compression codec for htable, valid value [none, snappy, lzo, gzip, lz4]
kylin.hbase.default.compression.codec=none

# HBase Cluster FileSystem, which serving hbase, format as hdfs://hbase-cluster:8020
# Leave empty if hbase running on same cluster with hive and mapreduce
#kylin.hbase.cluster.fs=

# The cut size for hbase region, in GB.
kylin.hbase.region.cut=5

# The hfile size of GB, smaller hfile leading to the converting hfile MR has more reducers and be faster.
# Set 0 to disable this optimization.
kylin.hbase.hfile.size.gb=2

kylin.hbase.region.count.min=1
kylin.hbase.region.count.max=500

# The percentage of the sampling, default 100%
kylin.job.cubing.inmem.sampling.percent=100

kylin.job.mapreduce.default.reduce.input.mb=500

kylin.job.mapreduce.max.reducer.number=500

kylin.job.mapreduce.mapper.input.rows=1000000

# 'auto', 'inmem', 'layer' or 'random' for testing
kylin.cube.algorithm=auto

kylin.cube.algorithm.auto.threshold=8

kylin.cube.aggrgroup.max.combination=4096

kylin.table.snapshot.max_mb=300
```


### 覆盖kylin_hive_conf.xml中参数

```
kylin.hive.config.override.[key]=value
```



### 覆盖kylin_job_conf.xml或 kylin_job_conf_inmem.xml中参数

```
kylin.job.mr.config.override.[key]=value
```