## Cube配置重定义

在Cube设计的`配置覆盖`步骤中，用户可以输入新的配置项，该配置项将覆盖系统级别默认值，目前支持重定义以下参数。 ![override](images/override.jpg)

### 覆盖kylin.properties中参数

*kylin.hbase.default.compression.codec*，默认值none，其他有效值包括snappy，lzo，gzip，lz4

*kylin.hbase.region.cut*，默认值5

*kylin.hbase.hfile.size.gb*，默认值2

*kylin.hbase.region.count.min*，默认值1

*kylin.hbase.region.count.max*，默认值500

*kylin.job.cubing.inmem.sampling.percent*，默认值100

*kylin.job.mapreduce.default.reduce.input.mb*，默认500

*kylin.job.mapreduce.max.reducer.number*，默认500

*kylin.job.mapreduce.mapper.input.rows*，默认1000000

*kylin.cube.algorithm*，默认auto，其它有效值包括inmem，layer

*kylin.cube.algorithm.auto.threshold*，默认8

*kylin.cube.aggrgroup.max.combination*，默认4096

*kylin.table.snapshot.max_mb*，默认300

### 覆盖kylin_hive_conf.xml中参数

KAP支持覆盖*kylin_hive_conf.xml*中的参数，以Key/Value的形式，按照如下格式替换：

kylin.hive.config.override.*key* = *value*

### 覆盖kylin_job_conf.xml和kylin_job_conf_inmem.xml中参数

KAP支持覆盖*kylin_job_conf.xml*和*kylin_job_conf_inmem.xml*中的参数，以Key/Value的形式，按照如下格式替换：

kylin.job.mr.config.override.*key* = *value*