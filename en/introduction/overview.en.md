## KAP Overview

Kyligence Analytics Platform（KAP）is an enterprise ready Big Data product, powered by *Apache Kylin*, which enables fast insight into petabyte-scale data with sub-second latency and SQL query capabilities on Apache Hadoop. It simplified Big Data Analytics for business users, analysts and engineers. 

KAP fills emerging needs of Big Data ecosystem to enable enterprise data warehouse and OLAP on top of Apache Hadoop, offering interactive query capability for analysts to build data model and analyze data in a self-service manner. KAP supports existing intelligence and visualization tools. With familiar concepts and experience, analysts could directly leverage KAP's power to analyze hundreds of billions of data records on Big Data tech stack without learning curve. The enterprise product is fully compatible with the open source Apache Kylin, users can seamlessly migrate to KAP for enterprise-class features, including better performance, user management, enhanced security and data encryption, built-in web analyzer, management and service, etc.

**KyStorage: Columnar Storage Engine**

KAP introduces new columnar storage engine *KyStorage*, supports multiplexed indexes, specially optimizes for ultra-high cardinality dimensions and complex filtering conditions. Compared with *Apache Kylin*, KAP increases query performance from several times to dozen times and reduced over 50%  storage space.

**Raw Data Query**

KAP which is built on *KyStorage* and inverted index technologies, breaks the traditional OLAP engine‘s limitations which query aggregation data only. It supports raw data query, optimizes the support for super wide table, reduces the difficulties of data modeling, provides better service for data exploration and analyses.

**KyAnalyzer: Agile self-serve OLAP BI tool**

Kyligence Analyzer as the built in its agile BI tool *KyAnalyzer*, users can explore multiple data sources interactively, and design report by familiar drag-and-drop style. Supports multidimensional analysis methods such as drilling, rolling, slicing, rotating, and so on. Provides dozens of reporting charts, simplifies multi-format data sharing, improvies big data analysis productivitity greatly.

**More Enterprise Level Features**

KAP supports out-of-the-box user management and cell-level access control. Integrates with customer exsiting authentication system easily. Guarantees security, manageability and traceability.

Supports more analysis functions such as *Percentitle*.

**Compatible with Hadoop Distribution**

KAP is compatible with open source Hadoop and mainstream commercial Hadoop distributions, including but not limited to *Apache Hadoop*, *Hortonworks HDP*, *Microsoft HDInsight*, *AWS EMR*. Has certified with  *Cloudera CDH*.

![](images/kap_eco.jpeg)



### KAP vs. Apache Kylin

|                            | Apache Kylin                      | KAP企业版                                | KAP高级企业版                                 |
| -------------------------- | --------------------------------- | ------------------------------------- | ---------------------------------------- |
| **Position**               | OLAP on Hadoop                    | Data Warehouse on Hadoop              | Data Warehouse on Hadoop                 |
| **Core**                   | Apache Kylin                      | Apache Kylin                          | Apache Kylin                             |
| **Query Performance**      | Sub-second query latency          | Same with Apache Kylin                | **Faster 3 ~ 40 times than Apache Kylin** |
| **Parallel Computing**     | Coprocessor                       | Coprocessor                           | **Spark**                                |
| **Storage Engine**         | HBase                             | HBase                                 | **Columnar Storage Engine：KyStorage**    |
| **Index**                  | Single Index                      | 单索引                                   | **Efficiently Support**                  |
| **Ultra High Cardinality** | Limited                           | Limited                               | **Efficiently Support**                  |
| **Wide Table**             | Limited                           | Limited                               | **Efficiently Support**                  |
| **Raw Data Query**         | Limited                           | Limited                               | **Efficiently Support**                  |
| **Security**               | Limited                           | **LDAP/Kerberos/Cell Level Security** | **LDAP/Kerberos/Cell Level Security**    |
| **BI Tool**                | No built-in                       | **Built-in Agile BI tool：KyAnalyzer** | **Built-in Agile BI tool：KyAnalyzer**    |
| **Technical Support**      | Open source community, no SLA     | SLA with 5*8 or 7\*24                 | SLA with 5\*8 or 7\*24                   |
| **KyBot Service**          | Not included, Purchase seperately | Included                              | Included                                 |

