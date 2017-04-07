## 创建数据模型
在数据源就绪的基础之上，我们开始创建数据模型。以KAP自带的样例数据为例，该数据集的数据模型包含1个事实表和2个维度表，表间通过外键进行关联。实际上，并不是表上所有的字段都有被分析的需要，因此我们可以有目的地仅选择所需字段添加到数据模型中。然后，根据分析师用户的具体分析场景，把这些字段设置为维度或度量。

### 创建步骤
打开KAP的Web UI，在左上角项目列表中选择刚刚创建的*KAP_Sample_1*项目，然后进入“模型”页面，并创建一个模型。
![](images/datamodel_1.png)
第一步，在基本信息页，输入模型名称为Sample_Model_1，然后单击下一步。
![](images/datamodel_2.png)
第二步，为模型选择事实表（*Fact Table*）和查找表（*Lookup Table*）。根据星型模型结构，选择KYLIN_SALES为事实表，然后添加KYLIN_CAL_DT和KYLIN_CATEGORY_GROUPINGS作为查找表，并设置好连接条件：

KYLIN_CAL_DT 连接类型：`Inner` 

连接条件：`DEFAULT.KYLIN_SALES.PART_DT = DEFAULT.KYLIN_CAL_DT.CAL_DT`

KYLIN_CATEGORY_GROUPINGS 连接类型：`Inner` 

连接条件：`KYLIN_SALES.LEAF_CATEG_ID = KYLIN_CATEGORY_GROUPINGS.LEAF_CATEG_ID`
`KYLIN_SALES.LSTG_SITE_ID = KYLIN_CATEGORY_GROUPINGS.SITE_ID`

设置好之后的界面如下：
![](images/datamodel_3.png)
第三步，从上一步添加的事实表和维度表中选择需要作为维度的字段。一般的，时间经常用来作为过滤条件，所以我们首先添加时间字段。此外，我们再添加商品分类、卖家ID等字段为维度，保存后结果如下图所示：
![](images/datamodel_4.png)
第四步，根据业务需要，从事实表上选择衡量指标的字段作为度量。例如，PRICE字段用来衡量销售额，ITEM_COUNT字段用来衡量商品销量，SELLER_ID用来衡量卖家的销售能力。保存后的最终结果如下图所示：
![](images/datamodel_5.png)
第五步，设置根据时间分段。一般来说，销售数据都是与日俱增的，每天都会有新数据通过ETL到达Hive中，需要选择增量构建方式构建Cube，所以需要选择用于分段的时间字段DEFAULT.KYLIN_SALES.PART_DT。根据样例数据可以看到，这一列时间的格式是yyyy-MM-dd，所以选择对应的日期格式。此外，我们既不需要设置单独的分区时间列，也不需要添加固定的过滤条件。设置效果如下图所示。
![](images/datamodel_6.png)最终，单击“保存”按钮，到此数据模型就创建成功了。
