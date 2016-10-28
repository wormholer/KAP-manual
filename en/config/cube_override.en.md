## Cube Configuration Override

At the Cube design `Configuration Overwrites` phase, user could input new properties to override the system level default value. The following properties are supported to be overrideded. ![override](images/override.jpg)

### Override kylin.properties

*kylin.hbase.default.compression.codec*, default is none, other valid values includes snappy, lzo, gzip, lz4

*kylin.hbase.region.cut*, default is 5

*kylin.hbase.hfile.size.gb*, default is 2

*kylin.hbase.region.count.min*, default is 1

*kylin.hbase.region.count.max*, default is 500

*kylin.job.cubing.inmem.sampling.percent*, default is 100

*kylin.job.mapreduce.default.reduce.input.mb*, default is 500

*kylin.job.mapreduce.max.reducer.number*, default is 500

*kylin.job.mapreduce.mapper.input.rows*, default is 1000000

*kylin.cube.algorithm*, default is auto, other valid values includes inmem, layer

*kylin.cube.algorithm.auto.threshold*, default is 8

*kylin.cube.aggrgroup.max.combination*, default is 4096

*kylin.table.snapshot.max_mb*, default is 300

### Override kylin_hive_conf.xml

KAP supports override *kylin_hive_conf.xml*, replace by the following Key/Value formats：

kylin.hive.config.override.*key* = *value*

### Override kylin_job_conf.xml and kylin_job_conf_inmem.xml

KAP supports override *kylin_job_conf.xml* and *kylin_job_conf_inmem.xml*, replace by the following Key/Value formats：

kylin.job.mr.config.override.*key* = *value*