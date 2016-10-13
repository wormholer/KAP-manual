## 样例数据集KAP
二进制包中包含了一份用于测试的样例数据集，总共大小仅1MB左右，共计3张表，其中事实表有10000条数据。因为数据规模较小，方便在虚拟机中进行快速实践和操作，用户可以自行搭建Hadoop Sandbox的虚拟机并快速部署KAP，然后导入该数据集进行试验。

KAP仅支持星型数据模型，这里用到的样例数据集就是一个规范的星型模型结构，它总共包含了3个数据表：

- *KYLIN_SALES* 该表是事实表，保存了销售订单的明细信息。每一列保存了卖家、商品分类、订单金额、商品数量等信息，每一行对应着一笔交易订单。
- *KYLIN_CATEGORY_GROUPINGS* 该表是维表，保存了商品分类的详细介绍，例如商品分类名称等。
- *KYLIN_CAL_DT* 该表是维表，保存了时间的扩展信息。如单个日期所在的年始、月始、周始、年份、月份等。这三张表一起构成了整个星型模型的结构，下图是实例-关系图（图中未列出表上的所有列）：
![](images/dataset_1.png)

### 数据表与关系


| 表                        | 字段                  | 意义      |
| ------------------------ | ------------------- | ------- |
| KYLIN_SALES              | PART_DT             | 订单日期    |
| KYLIN_SALES              | LEAF_CATEG_ID       | 商品分类ID  |
| KYLIN_SALES              | SELLER_ID           | 卖家ID    |
| KYLIN_SALES              | PRICE               | 订单金额    |
| KYLIN_SALES              | ITEM_COUNT          | 购买商品个数  |
| KYLIN_SALES              | LSTG_FORMAT_NAME    | 订单交易类型  |
| KYLIN_CATEGORY_GROUPINGS | USER_DEFINED_FIELD1 | 用户定义字段1 |
| KYLIN_CATEGORY_GROUPINGS | USER_DEFINED_FIELD3 | 用户定义字段3 |
| KYLIN_CATEGORY_GROUPINGS | UPD_DATE            | 更新日期    |
| KYLIN_CATEGORY_GROUPINGS | UPD_USER            | 更新负责人   |
| KYLIN_CATEGORY_GROUPINGS | META_CATEG_NAME     | 一级分类    |
| KYLIN_CATEGORY_GROUPINGS | CATEG_LVL2_NAME     | 二级分类    |
| KYLIN_CATEGORY_GROUPINGS | CATEG_LVL3_NAME     | 三级分类    |
| KYLIN_CAL_DT             | CAL_DT              | 日期      |
| KYLIN_CAL_DT             | WEEK_BEG_DT         | 周始日期    |