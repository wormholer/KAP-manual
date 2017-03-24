## 	KAP 2.3 Release Notes

### KAP 2.3 Highlight Features

The highlight features introduced by KAP 2.3 are as follow:

#### Intelligent Modeling Assistant

1. In the modeling phase, KAP could provide users with proper dimension/measure settings according to its resource data structure in order to simplify modeling process and improve system usability. 
2. Enhance data source statistics collecting efficiency. KAP can offer a lightweight sampling and stats collecting on extra large tables, providing data feature for modeling.

#### Fully Optimized Storage Engine KyStorage

1. Built-in tuning tools, which can automatically suggest optimal allocation according to cluster environment, so as to utilize cluster resource and improve query efficiency.
2. Enable snappy compression in storage by default, saving more storage space.
3. Push down filter to enhance filtering query efficiency by 20%.
4. Dynamic allocating of query node(Spark node) and computing resource, support self-adaptive resource allocation according to query request load level.

#### Refined Kafka Topic Load Process

Enhance Kafka as a core data source, support one-click load Topic and streaming data sampling, in order to provide customers with convenient integration solution.  

#### Asynchronous Export Massive Result Set

Support asynchronous query request, check query schedule and export 10 million level result set, so that KAP can be applied as data pre-treatment system.


#### Other updates including

- Optimize diagnostic package generating and support one-click upload KyBot diagnostic package.
- One-click promoting from Kylin to KAP.
- Better Cube expansion ratio computing method.
- Integration accessibility to Spark clusters in user environments and desired cluster management.
- Support various Cloud Hadoop file systems used as Kylin working directory.



#### Hadoop Distribution Support

  Certificated distributions:

  	Cloudera CDH 5.7/5.8/5.9

  Compatible distributions:

  ​	Apache Hadoop 2.2+，HBase 0.98+，Hive 0.14+

  	Hortonworks HDP 2.2/2.3/2.4/2.5

  	Microsoft HDInsight

  ​	Amazon EMR

  	Huawei FusionInsight C50/C60

  	AsiaInfo OCDP 2.4

### Download link

[http://kyligence.io/kyligence-analytics-platform/](http://kyligence.io/kyligence-analytics-platform/)


