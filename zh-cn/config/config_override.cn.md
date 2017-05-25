## 配置重定义

`KAP_HOME/conf`下的部分配置项可以在KAP GUI中进行配置重定义。配置重定义有两个作用域，分别是project级别和cube级别。二者与配置文件的覆盖优先级关系是：cube级别配置项 > project级别配置项 > 配置文件。

在project管理页面中打开某一项目的编辑页面，可以添加配置项，这些配置项将覆盖配置文件中的默认值。如图所示：

 ![override_project](images/override_project.jpg)

在cube设计的`配置覆盖`步骤中，可以添加配置项，这些配置项将覆盖project级别和配置文件中的默认值。如图所示：

 ![override](images/override.jpg)



### 覆盖kylin.properties中参数

目前kylin.properties中的下列配置项可以使用上述方法进行重定义：

kylin.hbase.default.compression.codec*，默认值none，其他有效值包括snappy，lzo，gzip，lz4

*kylin.hbase.region.cut*，默认值5

*kylin.hbase.hfile.size.gb*，默认值2

*kylin.hbase.region.count.min*，默认值1

*kylin.hbase.region.count.max*，默认值500

*kylin.job.cubing.inmem.sampling.percent*，默认值100

*kylin.job.mapreduce.default.reduce.input.mb*，默认值500

*kylin.job.mapreduce.max.reducer.number*，默认值500

*kylin.job.mapreduce.mapper.input.rows*，默认值1000000

*kylin.cube.algorithm*，默认值auto，其它有效值包括inmem，layer

*kylin.cube.algorithm.auto.threshold*，默认值8

*kylin.cube.aggrgroup.max.combination*，默认值4096

*kylin.table.snapshot.max_mb*，默认值300

### 覆盖kylin_hive_conf.xml中参数

KAP支持覆盖*kylin_hive_conf.xml*中的参数，以Key/Value的形式，按照如下格式替换：

kylin.hive.config.override.*key* = *value*

### 覆盖kylin_job_conf.xml和kylin_job_conf_inmem.xml中参数

KAP支持覆盖*kylin_job_conf.xml*和*kylin_job_conf_inmem.xml*中的参数，以Key/Value的形式，按照如下格式替换：

kylin.job.mr.config.override.*key* = *value*

注意：需要在参数名之前加上前缀*kylin.hive.config.override*或*kylin.job.mr.config.override*。举例如图：

![override_cube](images/override_cube.jpg)

其中红色矩形框内的部分为前缀，蓝色矩形框内的部分为参数名，二者之间用英文“.”连接。