## KAP 2.2 Release Notes

### KAP 2.2 Highlight Features

The highlight features introduced by KAP 2.2 are as follow:

#### Hadoop Distribution Support

Certificated distributions:

​	Cloudera CDH 5.7/5.8

Compatible distributions:

​	Apache Hadoop 2.2+, HBase 0.98+, Hive 0.14+

​	Hortonworks HDP 2.2/2.3/2.4

​	Microsoft HDInsight

​	Amazon EMR

​	Huawei FusionInsight C50/C60

​	AsiaInfo OCDP 2.4

#### Apache Kylin Core Upgrade: Support Scalable Streaming Cubing

KAP is based on core engine of the Apache Kylin, thus is totally compatible with the Apache Kylin. This release is based on the Apache Kylin 1.6.0. Please find the complete release announcement through the [Link](https://kylin.apache.org/docs16/release_notes.html).

The highlight features including:

1. Scalable streaming cubing, supports Kafka as data source
2. TopN measure optimisization
3. Support build/refresh/merge cube concurrently

#### Enhanced Source Statistic

Introduced Source Statistic. KAP collects statistic information from data source, including column cardinality, Null number, min/max value, character value and sample data, and it supports for Hive table and view.

#### Enhanced Modeling Assistant

Enable dimensions and measures auto generationand provide cuboid combination number suggestion to avoid explosion.

#### Optimized KyStorage

Improve the raw table query performance and stability, upgrade the encoding mechanism to reduce more storage.

#### Other updates including

- Support user-friendly Cube level properties overwrite, improve the build flexibility
- Support build job pause/resume, easy for debugging
- Provide optimized out-of-box configuration for sandbox and production profile
- Simplified Project/Cube metadata backup

### Download link

[http://kyligence.io/kyligence-analytics-platform/](http://kyligence.io/kyligence-analytics-platform/)


