## SQL 交集（Intersect）函数

留存率和转化率在互联网的数据分析场景下非常常见。一般来说，留存率的计算需要的就是两个数据集的交集的值，它们具有一些相同的维度（城市，类别等）和一个变化的维度（日期等）。KAP支持的留存率类的计算就是通过基于位图算法和UDAF算法的**intersect_count** 函数。本节我们将向您介绍如何使用交集函数。



### Intersect 查询语句

SQL **INTERSECT** 从句/操作符用来接合两个SELECT 语句，且返回的行来自第一个SELECT语句中且等同于第二个SELECT语句中某一行。也就是说INTERSECT只会返回两个SELECT语句的结果中共同的行。

对 intersect_count使用的具体规则描述如下：

`intersect_count(column To Count, column To Filter, filter Value List)`

`column To Count` 指向将要计算和应用于distinct count函数的列；

`column To Filter` 指向可变的维度；

`filter Value List` 指向可变维度中的值，且必须列在数组“array[ ]”中；



### 查询前提

为了在KAP中应用留存率的计算，sql查询需要符合以下要求：

- 有且只有一个可变维度；
- 需要计算的测量值必须已经被设定为可以进行精确count distinct计算的度量值（具体方法请见[Count Distinct(精确)查询优化](../../model/cube/count_distinct_precise.cn.md) ）；




### 示例

请选择默认**数据源** `learn_kylin`，其中表的结构如下：该数据集中有一张事实表（`KYLIN_SALES`）和两张维度表（`KYLIN_CAL_DT` 、`KYLIN_CATEGORY_GROUPINGS`）。请通过**数据样例**来熟悉事实表的结构 `KYLIN_SALES` 以便之后查询使用。

![](images/wd_datasample.png)

事实表 `KYLIN_SALES`  模拟了在线交易数据的记录表，包括的交易双方为卖家和买家。由此，我们可以通过以下查询语句，获得有多少比例的卖家能在新年假期阶段（2012.01.01-2012.01.03）进行持续的在线交易。

```
select LSTG_FORMAT_NAME,
intersect_count(seller_id, part_dt, array[date'2012-01-01']) as first_day,
intersect_count(seller_id, part_dt, array[date'2012-01-02']) as second_day,
intersect_count(seller_id, part_dt, array[date'2012-01-03']) as third_day,
intersect_count(seller_id, part_dt, array[date'2012-01-01',date'2012-01-02']) as retention_oneday,
intersect_count(seller_id, part_dt, array[date'2012-01-01',date'2012-01-02',date'2012-01-03']) as retention_twoday
from kylin_sales
where part_dt in (date'2012-01-01',date'2012-01-02',date'2012-01-03')
group by LSTG_FORMAT_NAME
```

如运行正确，则返回结果将如下。以下结果表示并没有卖家在新年阶段进行持续的在线交易。

![](images/intersect_count.1.png)