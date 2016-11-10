## KAP 2.1 发行说明

### KAP 2.1新功能

下面的段落将介绍KAP 2.1新引进的功能

#### Hadoop发行版支持

产品认证：

​	Cloudera CDH 5.7/5.8

兼容性测试：

​	Apache Hadoop 2.2+，HBase 0.98+，Hive 0.14+；

​	Hortonworks HDP 2.2/2.3/2.4；

​	Microsoft HDInsight；

​	Amazon EMR；

​	Huawei FusionInsight C50/C60；

​	亚信 OCDP 2.4

#### Apache Kylin Core 升级

KAP基于Apache Kylin内核引擎，与Apache Kylin完全兼容，本次发布基于1.5.4.1版本，完整发布公告参见[链接](http://kylin.apache.org/docs15/release_notes.html)。主要新功能如下：

1. 支持SQL Window操作
2. 支持SQL Grouping Sets操作
3. 优化TopN度量，支持更多维度

#### KyStorage 列式存储引擎

KyStorage是Kyligence基于HDFS全新研发的拥有自主知识产权的列式存储引擎。

本次更新主要功能如下：

1. 支持明细数据查询。KyStorage突破了传统OLAP引擎仅能查询聚合数据的局限，全面地支持了明细数据的查询。
2. 优化了对宽表的支持。降低了数据建模的难度，更好地服务数据探索式分析场景。
3. 针对稀疏列等场景优化了编码算法。

#### KyAnalyzer 敏捷BI工具

KyAnalyzer是Kyligence研发的敏捷BI自助多维分析工具。

本次主要更新如下：

1. 优化了元数据同步的流程，实现了数据模型一键同步，不再需要重新定义Data Source。
2. 支持Distinct Count／TopN的查询
3. 支持报表分享和导出

#### KyBot客户端整合

KyBot是Kyligence提供的在线智能诊断和优化服务，为Apache Kylin和KAP系统提供监控、性能优化、智能诊断服务。

KyBot客户端简化运维人员收集运行状态信息，降低运维成本。KAP 2.1集成了KyBot客户端。

#### 更多企业级功能更新

1. 支持单元格级别访问控制。突破传统Kylin只能支持项目和Cube级别的访问权限控制，提供单元格级别的访问控制能力，允许与用户已有账号、权限、组织结构系统深度集成，从而实现对行、列、单元格的访问权限管理。
2. 支持Percentile等高级分析函数

### 下载地址

[http://kyligence.io/kyligence-analytics-platform/](http://kyligence.io/kyligence-analytics-platform/)


