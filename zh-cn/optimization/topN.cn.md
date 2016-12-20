# Top-N 查询

我们在生活中也总是看到“世界100强公司”、“最受欢迎的20大电子产品”等新闻标题的报道。分析Top-N也是数据分析场景中常常遇到的需求。因而易见，人们普遍认同分析顶级项对大多数数据分析都是很有价值也很有必要的。在大数据时代，这种需求显现得越来越强，因为明细数据集越来越大。在没有预计算的情况下，得到一个分布式大数据集的Top-N结果需要很长时间，导致点对点查询的效率很差。

在Kylin v1.5.0（KAPv2.1），我们引入了“Top-N” 度量，旨在在Cube构建的时候预计算好需要的Top-N；在查询阶段，KAP就可以迅速的获取并返回Top-N记录。这样，查询性能就远远高于没有Top-N预计算结果的Cube，使得分析师对数据查询更有力。（**注意**：这里的“Top-N”度量是一个近似的实现，为了更好的应用它，你需要更多的了解Top-N背后的算法和数据分布的结构。）



## Top-N 查询语句

让我们用KAP包中初始默认的项目 `learn_kylin`（在KAP Web中也已经提前上传好了）。我们将重点使用其中的事实表 `kylin_sales`。如果你还没有建立过Cube，请参见以下文档构建Cube： [Quick Start with Sample Cube](https://kylin.apache.org/docs15/tutorial/kylin_sample.html).

这张样例表 “default.kylin_sales” 模拟了在线集市的交易数据，内含多个维度和度量列。这里我们仅用其中的四列即可：“PART_DT”，“LSTG_SITE_ID”，“SELLER_ID”和“PRICE”。下表为这些列的内容和基数简介，其中易见 “SELLER_ID”是一个高基列。

| Column       | Description                | Cardinality       |
| ------------ | -------------------------- | ----------------- |
| PART_DT      | Transaction Date           | 730: two years    |
| LSTG_SITE_ID | Site ID, 0 represents ‘US’ | 50                |
| SELLER_ID    | Seller ID                  | About one million |
| PRICE        | Sold amount                | -                 |

**方法1**： 该电商公司需要查询卖家中特定时段内交易额最高的100位卖家。查询语句如下：

```
SELECT SELLER_ID, SUM(PRICE) FROM KYLIN_SALES
 WHERE 
	PART_DT >= date'2012-02-18' AND PART_DT < date'2013-03-18' 
		AND LSTG_SITE_ID in (0) 
	group by SELLER_ID 
	order by SUM(PRICE) DESC limit 100
```

结果返回迅速，如下所示：

 ![](images/topN_1.png)

**方法2**: 为了得到同样的查询结果。如果在创建Cube时，对需要的Top-N进行了预计算则查询会更加高效。在创建Cube时，对所需的度量进行如下编辑，则可以在Cube构建时完成对目标列的预计算。如果Cube已被建立且没有对目标列进行过Top-N预计算，则需要重新建立Cube。度量编辑界面如下：

![](images/topN_2.png)



## 无Top-N 预计算

在 Kylin v1.5.0之前，只有维度列可以用“group by”查询，于是我们设计如下：用 `PART_DT`、 `LSTG_SITE_ID` 、 `SELLER_ID` 作为维度，同时定义 SUM(PRICE) 作为度量。Cube构建之后，基本的Cuboid则如下所示：

| Rowkey of base cuboid     | SUM(PRICE) |
| ------------------------- | ---------- |
| 20140318_00_seller0000001 | xx.xx      |
| 20140318_00_seller0000002 | xx.xx      |
| …                         | …          |
| 20140318_00_seller0999999 | xx.xx      |
| 20140318_01_seller0999999 | xx.xx      |
| …                         | …          |
| …                         | …          |
| 20160318_49_seller0999999 | xx.xx      |

假设这些维度都是彼此独立的，则基本Cuboid中行数为：$730*50*1million=36.5billion$。其他包含`SELLER_ID` 字段的Cuboid也有百万行。现在你应该意识到这种处理方法会使得Cube的膨胀率很高，如果维度更多或基数更高，则情况更糟。但真正的挑战还不在这里。

之后你还可能发现 Top-N查询并不能正常工作，或者话费特别长的时间。假设你想查30天内美国销售额排名最前的100名卖家，则查询引擎会从存储读取3000万记录行，然后聚合，分类，最终返回排名最前的100个卖家。由于没有进行预计算，即使最终结果很小，内存和其中的控制器都被严重耗用了。



## Top-N 预计算

如果在Cube创建时，设置了目标列Top-N的预计算，则当这个Cube被构建时，预计算结果会被存成一个新列。With the Top-N measure, KAP will pre-calculate the top entities for each dimension combination duing the cube create section, saving the result (both entity ID and measure value) as a column in storage. The entity ID (“SELLER_ID” in this case) now can be moved from dimension to the measure, which doesn’t participate in the rowkey. For the sample scenario described above, the newly designed cube will have 2 dimensions (PART_DT, LSTG_SITE_ID), and 1 Top-N measure.

| Rowkey of base cuboid | Top-N measure                            |
| --------------------- | ---------------------------------------- |
| 20140318_00           | seller0010091:xx.xx, seller0005002:xx.xx, …, seller0001789:xx.xx |
| 20140318_01           | seller0032036:xx.xx, seller0010091:xx.xx, …, seller000699:xx.xx |
| …                     | …                                        |
| 20160318_49           | seller0061016:xx.xx, seller0665091:xx.xx, …, seller000699:xx.xx |

The base cuboid only has $730 * 50 = 36.5 k$ rows now. In the measure cell, the Top certain records are stored in a container in descending order, those tail entities have been filtered out.

For the same query, “Top sellers in past 30 days in US” now only need to read 30 rows from storage. The measure object, also called as counter containers will be further aggregated/merged at the storage side, finally only one container is returned. KAP would extract the “SELLER_ID” and “SUM(PRICE)” from it before returns to client. The cost is much lighter than before, the performance is highly improved as well.



## 算法

Kylin’s Top-N implementation referred to [stream-lib](https://github.com/addthis/stream-lib/blob/master/src/main/java/com/clearspring/analytics/stream/StreamSummary.java), which is based on the Space-Saving algorithm and the Stream-Summary data structure as described in *[1]Efficient Computation of Frequent and Top-k Elements in Data Streams* by Metwally, Agrawal, and Abbadi.

A couple of modifications are made to let it better fit with Kylin:

- Using double as the counter data type;
- Simplfied data strucutre, using one linked list for all entries;
- A more compact serializer;

Besides, in order to run SpaceSaving in parallel on Hadoop, we make it mergable with the algorithm introduced in *[2] A parallel space saving algorithm for frequent items and the Hurwitz zeta distribution*.



## 准确度

Although the experiments in paper"Efficient Computation of Frequent and Top-k Elements in Data Streams" **[1]** has proved SpaceSaving’s efficiency and accuracy for realistic Zipfian data, it doesn’t ensure 100% accuracy for all scenarios. SpaceSaving uses a fixed space to put the most frequent candidates;  when the entities exceeds the space size, the tail entities will be truncated, causing data loss. The parallel algorithm merges multiple SpaceSavings into one, at that moment for the entities appeared in one but not in the other it had some assumptions, this will also cause some data distortion. Finally, the result from Top-N measure may have minor difference with the real result.

A couple of factors can affect the accuracy:

###### Zipfian distribution

Many rankings in the world follows the **[3] Zipfian distribution**, such as the population ranks of cities in various countries, corporation sizes, income rankings, etc. But the exponent of the distribution varies in different scenarios, this will affect the correctness of the result to some extend. The higher the exponent is (the distribution is more sharp), the more accurate answer will get. If the distribution is very flat, entities’ values are very close, the rankings from SpaceSaving will be less accurate. When using SpaceSaving, you’d better have an calculation on your data distribution.

###### Space in SpaceSaving

As mentioned above, SpaceSaving use a limited space to put the most frequent elements. Giving more space it will provide more accurate answer. For example, to calculate Top N elements, using 100 * N space would provide more accurate answer than 50 * N space. If the space is more than the entity’s cardinality, the result will be accurate. More space will take more CPU, memory and storage, this need be balanced.

###### Entity cardinality

Element cardinality is also a factor to consider. Calculating Top 100 among 10 thousands is easiser than among 10 million.

###### Dataset size

Error ratio from a big dataset is less than from a small dataset. The same for Top-N calculation.



## 测试结果

We designed a test case to calculate the top 100 elements using the **parallel SpaceSaving** among a generated data set (with commons-math3’s ZipfianDistribution). The entity’s occurancy follows the Zipfian distribution, adjusting the parameters of Zipfian exponent, space, entity cardinality and dataset size time to times, compare the result with the accurate result (using mergesort) to collect the statistics, we get a rough accuracy report in below.

The first column is the entity cardinality, which means among how many entities to identify the top 100 elements. The other three columns represent how much space using in the algorithm: 20X means using 2,000, 50X means use 5,000, and so on. Each cell of the table shows how many records are matched with the real result; if the error (or say difference) is less than 5 per million of total data size, then we would think it is matched. E.g, for a 1 million data set, if the difference < 5. The SpaceSaving is calculated in parallel with 10 threads.

#### Test 1. Calculate top-100 in 1 million records, Zipf distribution exponent = 0.5, error tolerance < 5

| Element cardinality | 20X space | 50X space | 100X space |
| ------------------- | --------- | --------- | ---------- |
| 1,000               | 100%      | 100%      | 100%       |
| 10,000              | 78%       | 100%      | 100%       |
| 100,000             | 12%       | 50%       | 95%        |

Conclusion: More space can get better accuracy.

#### Test 2. Calculate top-100 in 1 million records, Zipf distribution exponent = 0.6, error tolerance < 5

| Element cardinality | 20X space | 50X space | 100X space |
| ------------------- | --------- | --------- | ---------- |
| 1,000               | 100%      | 100%      | 100%       |
| 10,000              | 93%       | 100%      | 100%       |
| 100,000             | 30%       | 89%       | 99%        |

Conclusion: the more sharp entities distribute, the better answer SpaceSaving provides.

#### Test 3. Calculate top-100 in 20 million records, Zif distribution exponent = 0.5, error tolerance < 100

| Element cardinality | 20X space | 50X space | 100X space |
| ------------------- | --------- | --------- | ---------- |
| 1,000               | 100%      | 100%      | 100%       |
| 10,000              | 100%      | 100%      | 100%       |
| 100,000             | 100%      | 100%      | 100%       |
| 1,000,000           | 99%       | 100%      | 100%       |

Conclusion: the result from SpaceSaving will be closed to actual when the dataset is enough big.

#### Test 4. Calculate top-100 in 20 million records, Zif distribution exponent = 0.6, error tolerance < 100

| Element cardinality | 20X space | 50X space | 100X space |
| ------------------- | --------- | --------- | ---------- |
| 10,000              | 100%      | 100%      | 100%       |
| 20,000              | 100%      | 100%      | 100%       |
| 100,000             | 100%      | 100%      | 100%       |
| 1,000,000           | 99%       | 100%      | 100%       |

Conclusion: we would have the same conclusion as test 3 had.

These statistics match with our assumption before. It just gives us a rough estimation on the result correctness. To use this feature well in Kylin, you need to know about all these variables, and do some pilots before publish it to analysts.



### Further works

This feature is developed as a basic version, which may solve over 80% cases. While it has some limitations or hard-codings that need your attention:

- SUM() is a default aggregation function;
- Sort in descending order always;
- Use 50X space always;
- Use dictionary encoding for the literal column;
- UI only allow selecting topn(10), topn(100) and topn(1000) as the return type;

**Notice**: If you select “topn(10)” as the return type, it doesn’t mean you have to use `limit 10` in your query. You can use other limit numbers, Kylin can return the top 500 entities for one combination, but the precision of `limit query` (over 10) are not terrified.



## 参考文献

**[1]**[Efficient Computation of Frequent and Top-k Elements in Data Streams](https://dl.acm.org/citation.cfm?id=2131596)

**[2]**[A parallel space saving algorithm for frequent itemsand the Hurwitz zeta distribution](http://arxiv.org/pdf/1401.0702.pdf)

**[3]**[Zipfian law on wikipedia](https://en.wikipedia.org/wiki/Zipf%27s_law)

**[4]**[Approximate Top-N support in Kylin](http://kylin.apache.org/blog/2016/03/19/approximate-topn-measure/)