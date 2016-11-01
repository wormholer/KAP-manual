## KAP 2.0 发行说明

### 下载地址

http://kyligence.io/kyligence-analytics-platform/

### KAP 2.0新功能

下面的段落将介绍KAP 2.0新引进的功能

#### Hadoop发行版支持

产品认证：Cloudera CDH 5.7

兼容性测试：Hortonworks HDP 2.2/2.3/2.4；Microsoft HDInsight；Amazon EMR

#### 全新的列式存储引擎

采用了全新的基于HDFS的列式存储引擎，不再依赖HBase存储索引数据。支持多路复合索引，针对超高基数维度、复杂过滤条件等的场景进行了专门优化，相对*Apache Kylin*，查询性能有几倍到几十倍的提升，在存储空间上也有超过50％的节省。

#### 集成Saiku

Saiku是一款易用的开源的敏捷BI工具，KAP提供了元数据编辑器，允许导入KAP的Cube定义。Saiku是面向MDX查询语言设计的OLAP分析工具，配合mondrian插件，可以将MDX转换为SQL。

#### 开箱即用的用户管理

内置用户友好的管理界面，快速配置用户账号和权限，实现开箱即用。

#### 支持多国语言

支持中英两种语言，支持可扩展语言包。

#### Job引擎高可用

支持基于ZooKeeper的Job引擎高可用，自动恢复。