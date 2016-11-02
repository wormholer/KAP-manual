## KAP 2.0 发行说明

### KAP 2.0新功能

下面的段落将介绍KAP 2.0新引进的功能

#### Hadoop发行版支持

产品认证：Cloudera CDH 5.7

兼容性测试：Apache Hadoop 2.2+，HBase 0.98+，Hive 0.14+；Hortonworks HDP 2.2/2.3/2.4；Microsoft HDInsight；Amazon EMR

#### Apache Kylin Core

KAP基于Apache Kylin内核引擎，与Apache Kylin完全兼容，本次发布基于1.5.3版本，完整发布公告参见[链接](http://kylin.apache.org/docs15/release_notes.html)。

主要新功能如下：

1. 支持精确去重度量
2. 支持全局字典编码
3. 支持Cube级别配置重写
4. 精简JDBC依赖
5. 通过标准Hadoop API获取任务状态

#### KyStorage

KyStorage是Kyligence基于HDFS全新研发的拥有自主知识产权的列式存储引擎。

主要新功能如下：

1. 将存储引擎从HBase透明替换为KyStorage，相对*Apache Kylin*查询性能有几倍到几十倍的提升，存储空间节省超过50%
2. 支持多路复合索引，针对超高基数维度、复杂过滤条件等的场景进行了专门优化。

#### KyAnalyzer

KyAnalyzer是Kyligence研发的敏捷BI工具。

主要新功能如下：

1. 支持导入KAP中的Cube定义
2. 提供元数据编辑器，允许在线编辑所导入的Cube定义
3. 发布KAP兼容的kylin-mondrian插件
4. 集成了KAP用户认证系统
5. 支持MDX语法

#### 安全性、稳定性及其它更新

1. 开箱即用的用户管理。内置用户友好的管理界面，快速配置用户账号和权限，实现开箱即用。
2. 支持多国语言。支持中英两种语言，支持可扩展语言包。
3. Job引擎高可用。支持基于ZooKeeper的Job引擎高可用，自动恢复。


### 下载地址

http://kyligence.io/kyligence-analytics-platform/
