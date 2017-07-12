## KAP 2.4 Release Notes

### KAP 2.4 Highlight Features

The highlight features introduced by KAP 2.4 are as follow:

#### Redesigned User Experience

**KyStudio: Modeling Tool**

KyStudio is totally redesigned with a modeling GUI. With intuitive model structure and drag-and-drop modeling process, it brings a new visual experience, and enables the analysts to import data, design model/cube, build cube and process more works smoothly through a *self-service* interface.  

![KyStudio](images/24_kystudio.png)

**Support Snowflake Model**

KAP extends the modeling design capability to support Snowflake schema. Both Star schema and snowflake schema are supported by KAP now. By leveraging optimized index technology, KAP supports ultra-high dimension lookup table and resolves huge table join problem.  

#### Extended Query Capability

**Query Pushdown** 

Previously, KAP only answer cube-based query. Upgraded to 2.4, KAP could route the cube incapable query to another SQL engine by enabling Query Pushdown. KAP has embedded Spark SQL and Hive as the pushdown engines, other SQL on Hadoop engines will be onboard soon. KAP supports both mission-critical and exploratory analytics, by leveraging cube-based sub-second high performance query and pushdown-based query respectively.   

![Beyond OLAP](images/24_beyondolap.png)

**Hybrid-OLAP Architecture** 

KAP upgrades Kylin's architecture from MOLAP(Multidimensional OLAP) to HOLAP(Hybrid OLAP). It supports both aggregation query and detailed-data query, to meet multiple analytics scenario requirement. 

**Seamless Integration with SQL on Hadoop** 

KAP integrates with existing SQL on Hadoop seamlessly, with no data movement and no model redesign required, and it reuses existing analytics capability. KAP brings the transparent speedup power to data access layer, and empowers the unified query gateway for all Business Intelligence(BI) applications. By taking full advantage of pre-calculation technology, KAP enables BI to analyze massive data on Hadoop directly, and fills the gap between BI and Hadoop.  

#### Enriched Semantic Layer

**Computed Column**

The semantic layer is enriched by introducing computed column technology. KAP allows user to define computed column on the original source table to extract/transform/redefine original column into new virtual column. The computed column works like other original column which will be pre-calculated during cubing phase. The computed column enables analyst to do data clean/transform without IT team, all by themselves. It also improves the query performance by pre-calculated the filter condition. Hive User Defined Function(UDF) is supported on computed column, this allow user to reuse existing code and libraries. 

![Computed Column](images/24_computedcolumn.png)

#### Enhanced Intelligent Modeling

**Model Health Inspection**

Model Health Inspection is enabled by running statistics algorithm. It will figure out the modeling potential issues, such as primary-foreign key mismatch and data skew. The inspection result indicates how to improve the model design directly and efficiently. 

**Cube Optimizer**

Cube Optimizer will analyze source data characters and inputted SQL patterns first, and then generates the suggested cube design, including suggested dimensions, aggregation group settings, measures settings, encoding algorithm and the rowkey order. This method reduces the modeling learning curve, helps user to finish the modeling steps by simple clicks. 

**Cuboid Pruning based on the Maximum Dimension Combination**

The Maximum Dimension Combination is the biggest usage of dimension combination number during queries. KAP would prune the cuboid combination by following the Max Dimension Combination rule. The cuboid pruning avoids the rarely used cuboid build, reduces the cubing time, and resolves the cuboid explosion problem. In some real-cases, it shrinks the cuboid by more than 90%. 

#### New Cube Scheduler

**Cube Building Scheduler**

Cube Building Scheduler enables to build the cube on schedule, minutely, hourly or daily. It reduces the operation cost, and enables analysts to build the cube by themselves with automatic scheduler service. The Cube Build Scheduler works very well with Kafka in streaming cubing case, with better operation experience and reliability. 

#### Easy to Operate  

**Installation Environment Inspection**

Full environment check scripts are provided, it inspects the environment dependency, permission, version and other necessary resource. The inspection result indicates the potential issues and provides solution before KAP get starts. 

**New Metadata Storage**

Relational databases, such as MySQL, can be used as the KAP metadata store. By moving the metadata from HBase to relational database, the database operation strategies would be followed. With no HBase needed, the total operation cost and risk are reduced dramatically.

#### Upgrade Apache Kylin to 2.0

KAP is built on Apache Kylin core, it's 100% compatible with Apache Kylin. KAP 2.4 upgrades Apache Kylin to 2.0, and the complete Kylin release notes could be found on the [Kylin website](http://kylin.apache.org/blog/2017/02/25/v2.0.0-beta-ready/). The highlight features including:

KYLIN-2467: Support TPCH queries

KYLIN-2331: Spark cubing engine

KYLIN-2006: Job Engine HA

KYLIN-2351: Support cloud-based storage

#### More enhancement and bug-fix

KYLIN-2521: Upgrade Apache Calcite to 1.12

KYLIN-490: Support Distinct Count for multiple columns

Table Index supports multiple sorted by/shard by definitions, improves the detailed query

Build engine upgraded, reduces the IO cost, and accelerates the cubing 

Allow to set the time range for KyBot diagnostic package, reduces the log size

Support save model and cube as draft, improve the modeling experience

Support cluster service discovery based on ZooKeeper, eliminates the manual mistakes. 

Support customized measure precision 

Easy to upgrade, all configurations are back-compatible

KyAnalyzer access control is integrated with KAP backend

#### Hadoop Distribution Support

 Certificated distributions ：

  	Cloudera CDH 5.7+

  Compatible distributions：

  	Apache Hadoop 2.2+，HBase 0.98+，Hive 0.14+

  	Hortonworks HDP 2.2+

  	Microsoft HDInsight

  	Amazon EMR

  	Huawei FusionInsight C50/C60