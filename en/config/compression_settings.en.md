## Enable compression

By default, KAP does not enable compression, which is not the recommended setting for production environment, but a tradeoff for new KAP users. A suitable compression algorithm could reduce the storage overhead. However, unsupported algorithm might break the KAP jobs also. There are three kinds of compression settings used in KAP: HBase table compression, Hive output compression and MR job output compression.

###HBase table compression

The compression settings are defined in kylin.properties by *kylin.hbase.default.compression.codec*. The default value is *none*. Valid values include *none*, *snappy*, *lzo*, *gzip* and *lz4*. Before changing the compression algorithm, please make sure the selected algorithm is supported on your HBase cluster. Especially for snappy, lzo and lz4, because not all Hadoop distributions include these algorithms. 

Notice: considering that KyStorage is the default storage engine in KAP Plus rather than HBase, the compression settings are valid only if KAP users HBase as one of its storage engines. 

### Hive output compression

The compression settings are defined in kylin_hive_conf.xml. The default setting is empty which leverages the Hive default configuration. If you want to override the settings, please add (or replace) the following properties into kylin_hive_conf.xml. Take the snappy compression for example: 

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

### MR jobs output compression

The compression settings are defined in kylin_job_conf.xml and kylin_job_conf_inmem.xml. The default setting is empty which leverages the MR default configuration. If you want to override the settings, please add (or replace) the following properties into kylin_job_conf.xml and kylin_job_conf_inmem.xml. Take the snappy compression for example: 

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

**Notice: all configurations above will take effect after restarting KAP.  **