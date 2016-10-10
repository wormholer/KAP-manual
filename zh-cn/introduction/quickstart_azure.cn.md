## KAP Azure 镜像快速上手
该镜像实例中，包含了Hadoop 2.6＋HBase0.98 + Hive 0.14 和KAP最新体验版，免去了用户安装配置Hadoop集群的复杂工作，该上手指南将帮助用户尽快熟悉KAP的概念和流程，体验OLAP on Hadoop技术。本镜像不适用于生产环境配置和性能相关测试。


#### 登录
KAP默认访问地址：HOSTNAME:7070/kylin/，默认用户名：ADMIN  密码:KYLIN 

![](/images/introduction/quickstart/login.png)

#### 了解样例数据集

镜像中的KAP包含了一份用于测试的样例数据集， 共计3张表，其中事实表有10000条数据。该数据集是一个规范的星型模型结构，它总共包含了3个数据表：
* KYLIN_SALES 该表是事实表，保存了销售订单的明细信息。每一列保存了卖家、商品分类、订单金额、商品数量等信息，每一行对应着一笔交易订单。
* KYLIN_CATEGORY_GROUPINGS 该表是维表，保存了商品分类的详细介绍，例如商品分类名称等。
* KYLIN_CAL_DT 该表是维表，保存了时间的扩展信息。如单个日期所在的年始、月始、周始、年份、月份等。
这三张表一起构成了整个星型模型的结构，下图是实例-关系图（图中未列出表上的所有列）

##### 表关系
![](/images/introduction/quickstart/ER.png)

##### 表关键字段

| 表  | 字段  | 意义  |
|---|---|---|
| KYLIN_SALES  | PART_DT  | 订单日期  |
| KYLIN_SALES | LEAF_CATEG_ID  | 商品分类ID  |
| KYLIN_SALES  | SELLER_ID  | 卖家ID  |
| KYLIN_SALES  | PRICE  | 订单金额  |
| KYLIN_SALES  | ITEM_COUNT  | 购买商品个数  |
| KYLIN_SALES  | LSTG_FORMAT_NAME  | 订单交易类型  |
| KYLIN_CATEGORY_GROUPINGS  | USER_DEFINED_FIELD1  | 用户定义字段1  |
| KYLIN_CATEGORY_GROUPINGS  | USER_DEFINED_FIELD3  | 用户定义字段3  |
| KYLIN_CATEGORY_GROUPINGS  | UPD_DATE  | 更新日期  |
| KYLIN_CATEGORY_GROUPINGS  | UPD_USER  | 更新负责人  |
| KYLIN_CATEGORY_GROUPINGS  | META_CATEG_NAME  | 一级分类  |
| KYLIN_CATEGORY_GROUPINGS  | CATEG_LVL2_NAME  | 二级分类  |
| KYLIN_CATEGORY_GROUPINGS  | CATEG_LVL3_NAME  | 三级分类  |
| KYLIN_CAL_DT  | CAL_DT  | 日期  |
| KYLIN_CAL_DT  | WEEK_BEG_DT  | 周始日期  |

#### 尝试SQL查询

镜像中的KAP已经建立了例子项目learn_kylin，设计了模型和Cube，并且已经完成了样例Cube的构建，用户可以直接执行SQL查询。

##### 操作步骤：
![](/images/introduction/quickstart/insight.png)
1.	选择第一个分析TAB，跳转分析页
2.	在页面左上角选择样例所用的learn_kylin 项目
3.	在新查询输入框里输入SQL语句
4.	点击提交按钮
5.	查看返回结果

##### SQL举例：
* 单表行数统计
> SELECT COUNT(*) FROM KYLIN_SALES

这条SQL用于查询KYLIN_SALES表中总行数，读者可以同时在Hive中执行同样的查询进行性能对比。在笔者的对比中，Hive查询耗时29.385秒，KAP查询耗时0.18秒。

* 多表连接
>SELECT
KYLIN_SALES.PART_DT
,KYLIN_SALES.LEAF_CATEG_ID
,KYLIN_SALES.LSTG_SITE_ID
,KYLIN_CATEGORY_GROUPINGS.META_CATEG_NAME
,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL2_NAME
,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL3_NAME
,KYLIN_SALES.LSTG_FORMAT_NAME
,SUM(KYLIN_SALES.PRICE)
,COUNT(DISTINCT KYLIN_SALES.SELLER_ID)
FROM KYLIN_SALES as KYLIN_SALES 
INNER JOIN KYLIN_CAL_DT as KYLIN_CAL_DT
ON KYLIN_SALES.PART_DT = KYLIN_CAL_DT.CAL_DT
INNER JOIN KYLIN_CATEGORY_GROUPINGS as KYLIN_CATEGORY_GROUPINGS
ON KYLIN_SALES.LEAF_CATEG_ID = KYLIN_CATEGORY_GROUPINGS.LEAF_CATEG_ID AND KYLIN_SALES.LSTG_SITE_ID = KYLIN_CATEGORY_GROUPINGS.SITE_ID
GROUP BY 
KYLIN_SALES.PART_DT
,KYLIN_SALES.LEAF_CATEG_ID
,KYLIN_SALES.LSTG_SITE_ID
,KYLIN_CATEGORY_GROUPINGS.META_CATEG_NAME
,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL2_NAME
,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL3_NAME
,KYLIN_SALES.LSTG_FORMAT_NAME

这条SQL将事实表KYLIN_SALES和两张维表根据外键进行了内部连接。在对比测试中，Hive查询耗时34.361秒，KAP查询耗时0.33秒。

* 维度列 COUNT_DISTINCT
>SELECT
COUNT(DISTINCT KYLIN_SALES.PART_DT)
FROM KYLIN_SALES as KYLIN_SALES 
INNER JOIN KYLIN_CAL_DT as KYLIN_CAL_DT
ON KYLIN_SALES.PART_DT = KYLIN_CAL_DT.CAL_DT
INNER JOIN KYLIN_CATEGORY_GROUPINGS as KYLIN_CATEGORY_GROUPINGS
ON KYLIN_SALES.LEAF_CATEG_ID = KYLIN_CATEGORY_GROUPINGS.LEAF_CATEG_ID AND KYLIN_SALES.LSTG

这条SQL对PART_DT字段进行了COUNT_DISTINCT查询，但是该字段并没有被添加为COUNT_DISTINCT的度量。KAP会在运行时计算结果。在对比测试中，Hive查询耗时44.911秒，KAP查询耗时0.12秒。

* 全表查询
>SELECT * FROM KYLIN_SALES

默认的，KAP并不对原始数据的明细进行保存，因此并不支持类似的不带GROUP BY的查询。但是，用户经常希望通过执行SELECT * 获取部分样例数据；因此KAP对这种SQL会返回不精确的查询结果（通过隐式地GROUP BY所有维度）。如果用户希望KAP支持原始数据的保存和查询，可以在Cube中定义RAW类型的度量。

#### 体验Cube操作

如用户想进一步体验模型，Cube的设计和构建，也可以根据样例数据来尝试创建与构建Cube。具体操作步骤可参考本手册中的[创建数据模型](https://kyligence-git.gitbooks.io/kap-user-manual/content/zh-cn/molap/datamodel.cn.html)，[创建Cube](https://kyligence-git.gitbooks.io/kap-user-manual/content/zh-cn/molap/create_cube.cn.html) 和[构建Cube章节](https://kyligence-git.gitbooks.io/kap-user-manual/content/zh-cn/molap/build_cube.cn.html)。

更多意见与建议，欢迎与我们联系：support@kyligence.io

