## KAP 2.0 Release Notes

### KAP 2.0 Highlight Features

The highlight features introduced by KAP 2.0 are as follows:

#### Hadoop Distribution Certification Support

Certificated distributions: 

​	Cloudera CDH 5.7

Compatible distributions: 

​	Apache Hadoop 2.2+, HBase 0.98+, Hive 0.14+

​	Hortonworks HDP 2.2/2.3/2.4

​	Microsoft HDInsight

​	Amazon EMR

#### Apache Kylin Core Upgrade

KAP is based on core engine of Apache Kylin, thus is totally compatible with the Apache Kylin. This release is based on the Apache Kylin 1.5.3. Please find the complete release announcement through the [link](http://kylin.apache.org/docs15/release_notes.html).

The highlight features including:

1. Realize accurate distinct count function
2. Support global dictionary encoding
3. Support Cube level configuration overriding
4. Simplify JDBC dependency
5. Retrieve task status by standard API Hadoop


#### KyStorage: Columnar Storage Engine

KyStorage is a columnar storage engine based on HDFS and developed by Kyligence with independent intellectual property.

The main updates including:

1. Compared with Apache Kylin, KAP improves query performance from 3 to 40 times and reduces over 50%  storage space.
2. It supports multiplexed indexes, specially optimizes for ultra-high cardinality dimensions and complex filtering conditions.

#### KyAnalyzer: Agile Self-serve OLAP BI Tool

KyAnalyzer is a self-serve agile BI tool developed by Kyligence.

The main updates including:

1. Sync Cube's definition from KAP/Apache Kylin
2. Online metadata editor 
3. Kylin-mondrian component designed for KAP/Apache Kylin
4. Integrate authentication with KAP/Apache Kylin
5. Support MDX syntax

#### More Enterprise Level Features

1. Build-in Out of box user management; allow quick configuration of user accounts and access
2. Internationalize support. It supports both Chinese and English version as well as extensible language packages. 
3. Support Job Server high availability. It supports Job Server high availability built on Zookeeper and is automatically restored. 


### Download link

http://kyligence.io/kyligence-analytics-platform/
