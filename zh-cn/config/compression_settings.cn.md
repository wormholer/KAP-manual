## 启用压缩

KAP在默认状态下不会启用压缩。这并非适合生产环境的推荐配置，而是出于为新用户的考虑而做出的保守选择。配置合适的压缩算法可以大大降低存储开销；但配置环境未支持的压缩算法也可能破坏任务进程。KAP中有三处使用压缩，分别是HBase表压缩、Hive输出压缩和MR任务输出压缩。

### HBase表压缩

该项压缩通过kylin.properties中的kylin.hbase.default.compression.codec参数进行配置。默认值为*none*，有效可选值包括：*none*、*snappy*、*lzo*、*gzip*以及*lz4*。在修改压缩算法前，请确保你的HBase集群支持所选的压缩算法，尤其是对于*snappy*、*lzo*和*lz4*，并非所有的Hadoop环境都支持这些算法。

注意：由于KAP Plus中默认使用KyStorage代替HBase作为存储引擎，该项压缩算法配置仅在仍使用HBase作为存储引擎之一时有效。

### Hive输出压缩

该项压缩通过kylin_hive_conf.xml进行配置。默认配置为空，即直接使用Hive的默认配置。如想覆盖改配置，请添加下面的参数至kylin_hive_conf.xml中，或替换掉其中原先存在的相应部分。此处以*snappy*压缩算法为例：

```
<property>
    <name>mapreduce.map.output.compress.codec</name>
    <value>org.apache.hadoop.io.compress.SnappyCodec</value>
    <description></description>
</property>
<property>
    <name>mapreduce.output.fileoutputformat.compress.codec</name>
    <value>org.apache.hadoop.io.compress.SnappyCodec</value>
    <description></description>
</property>
```

### MR任务输出压缩

该项压缩通过kylin_job_conf.xml和kylin_job_conf_inmem.xml进行配置。默认配置为空，即直接使用MR的默认配置。如想覆盖改配置，请添加下面的参数至kylin_job_conf.xml和kylin_job_conf_inmem.xml中，或替换掉其中原先存在的相应部分。仍以*snappy*压缩算法为例：

```
<property>
    <name>mapreduce.map.output.compress.codec</name>
    <value>org.apache.hadoop.io.compress.SnappyCodec</value>
    <description></description>
</property>
<property>
    <name>mapreduce.output.fileoutputformat.compress.codec</name>
    <value>org.apache.hadoop.io.compress.SnappyCodec</value>
    <description></description>
</property>
```

**注意：上述配置仅在重启KAP后方生效。**