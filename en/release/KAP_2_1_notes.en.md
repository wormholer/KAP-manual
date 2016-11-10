## KAP 2.1 Release Notes

### KAP 2.1 Highlight Features

The highlight features introduced by KAP 2.1 are as follow:

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

#### Apache Kylin Core Upgrade

KAP is based on core engine of the Apache Kylin, thus is totally compatible with the Apache Kylin. This release is based on the Apache Kylin 1.5.4.1. Please find the complete release announcement through the [Link](http://kylin.apache.org/docs15/release_notes.html).

The highlight features including:

1. Support SQL Window action
2. Support SQL Grouping Sets action
3. Optimize TopN measure; support more dimensions

#### KyStorage: Columnar Storage Engine

KyStorage, is a HDFS based columnar storage engine developed by Kyligence with independent intellectual property. 

The main updates including:

1. Support raw data query. KyStorage breaks the limitation of traditional OLAP engine on querying aggregation data only, provides fully support for the raw data query.
2. Optimize the wide table support. It reduces the data modeling difficulty, thus would offer better service for data-exploratory analysis scenarios.
3. Optimize coding algorithm for sparse columns and other such scenarios.  

#### KyAnalyzer: Agile Self-serve OLAP BI Tool

KyAnalyzer is a multi-dimension agile BI tool developed by Kyligence.

The main updates including:

1. Realize sync KAP/Kylin data model by one click. The process of metadata synchronization is optimized and there is no need to redefine Data Source.
2. Support Distinct Count query／TopN query
3. Support report sharing and export

#### KyBot Client Integration

KyBot is an online intelligent diagnosis and optimization service provided by Kyligence, which provides monitoring, cube optimizing, intelligent diagnosis and ticket service. 

KyBot client aims to simplify the operating and maintaining information collection by administrator to reduce operation cost. KAP 2.1 integrated KyBot client.

#### More Enterprise-Level Features

1. Support cell level access control. KAP 2.1 breaks the limitation that Apache Kylin can only support the project and cube level access control. It provides cell level access control capability, allows integrating with user existing AAA system, thus managing the access rights for rows, columns, and cells.
2. Support Percentile and more advanced analysis functions

### Download link

[http://kyligence.io/kyligence-analytics-platform/](http://kyligence.io/kyligence-analytics-platform/)


