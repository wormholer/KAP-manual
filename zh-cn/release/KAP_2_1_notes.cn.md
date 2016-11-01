## KAP 2.1 发行说明

### 下载地址

[http:\/\/kyligence.io\/kyligence-analytics-platform\/](http://kyligence.io/kyligence-analytics-platform/)

### KAP 2.1新功能

下面的段落将介绍KAP 2.1新引进的功能

#### Hadoop发行版支持

产品认证：Cloudera CDH 5.7\/5.8

兼容性测试：Hortonworks HDP 2.2\/2.3\/2.4；Microsoft HDInsight；Amazon EMR

#### 升级列式存储引擎

#### 明细记录查询

KAP基于自身列式存储引擎，及倒排索引等多种索引技术，突破了传统OLAP引擎仅能查询聚合数据的局限，全面地支持了明细数据的查询，优化了对宽表的支持，降低了数据建模的难度，更好地服务数据探索式分析场景。

#### 内置敏捷BI工具KyAnalyzer

优化了Saiku元数据同步的流程，实现了数据模型自动同步，修复了大量Saiku与KAP SQL不匹配的问题，并将Saiku重命名为KyAnalyzer，作为KAP的内置BI工具。

#### 单元格级别访问控制

突破传统Kylin只能支持项目和Cube级别的访问权限控制，提供单元格级别的访问控制能力，允许与用户已有账号、权限、组织结构系统深度集成，从而实现对行、列、单元格的访问权限管理。

#### 集成KyBot客户端

KyBot是Kyligence提供的智能在线服务，提供KAP运行监控、性能优化、智能诊断服务，KyBot客户端简化运维人员收集运行状态信息，降低运维成本。

