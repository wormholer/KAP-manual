#基于最大维度组合数的Cuboid剪枝

聚合组以及其他的高级优化功能很好得解决了Cuboid数量爆炸问题，但为了达到优化效果用户需要对数据模型有一定了解，这对于初级用户有一定使用难度。这一章将介绍一种简单的Cuboid剪枝工具——基于最大维度组合数的Cuboid剪枝（MDC）。这个剪枝方法能够避免生成大的Cuboid（包含dimension数目过多的Cuboid），从而减少生成Cube的开销。该剪枝方法适用的场景为大多数查询语句访问的维度不多于N的情况，这里的N是可以配置的MDC参数。

## 查询维度统计方法

查询时访问维度数目的计算方法对于不同版本 KAP 有所区别，将在下列几节详细介绍。

### 版本2.4.0 ~ 版本2.4.2

查询访问的维度数目为group-by列和filter列并集后的总列数。为了方便理解，给出下面三个例子：

1. 查询仅仅包含group-by列

```sql
-- 4个维度 
select count(*) from table group by column1, column2, column3, column4
```

2. 查询仅仅包含filter列

```sql
-- 4个维度
select count(*) from table where column1='a' and column2='b' or column3='c' and column4='d'
```

3. 查询既包含group-by列，又包含filter列。

```sql
-- 3个维度
select count(*) from table where column1='a', column2='b' group by column2, column3
```

### 版本2.4.3 ~ 最新版本

从版本2.4.3开始，我们在Cuboid剪枝中将同在一个联合维度当做一个整体，即为一个维度，同理一个层级维度也是当做一个维度，而必需维度则不考虑在内。如下例所示：

```sql
select count(*) from table group by column_mandatory, column_joint1, column_joint2, column_hierarchy1, column_hierarchy2, column_normal
```

查询涉及到一个必须维度，两个从属于一个联合维度的维度，两个从属于一个层级维度的维度和一个普通维度。在这里我们计算出3个维度。

## 功能开启

这一小节将介绍如何开启该剪枝工具。

该剪枝工具位于Cube维度设计页的维度优化中，如下图所示。默认值为0，意思为关闭MDC剪枝。在输入框中输入一个正整数再点击Apply即可开启MDC剪枝功能。该正整数为一个Cuboid能够包含的最多的维度数。

![](images/cuboid_pruning_1.jpg)

<p align="center">图1</p>

该例子中维度数目从161下降为20，除了Base Cuboid，包含多余4个维度的Cuboid都被剪裁掉了。

![](images/cuboid_pruning_2.jpg)

<p align="center">图2</p>

## 注意事项

一方面该剪枝方法能够显著减少Cube中包含的Cuboid数目，另一方面一些需要访问许多dimension的复杂查询则会命中较大的Cuboid，造成大量的在线计算，最终导致查询速度变慢。和其他的剪枝方法一样，该方法是一种数据模型的妥协和权衡，要在存储空间和查询速度间进行取舍。如果多数查询访问的维度数目不多，该方法能个起到显著的作用。
