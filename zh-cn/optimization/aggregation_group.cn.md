# 聚合组优化

在所有基于预计算的OLAP引擎中，维度灾难是一个广为诟病的问题。在v1.5 （KAP2.1）之前的版本中，Kylin试图通过一些基本的技术解决这个问题，也确实在某些程度上减轻了问题的严重性。在之后的开源实践中，我们发现这些基本技术缺乏系统性的设计思维，也无法解决更多更普遍的问题。于是在Kylinv1.5中，我们重新设计了聚合组的设计机制使得Kylin更好的服务于所有Cube的设计场景。



## 聚合组简介

Kylin通过预计算Cubes提高了查询的性能，而Cube则包含了所有维度的不同的两两组合，每一种组合即为一个Cuboid。问题在于随着维度增加，Cuboids也会急剧增加。比如，一个有3种维度的Cube里，总共包括8个Cuboid；当增加1个维度时，Cuboid数目翻倍变成16个。即使Kylin使用可扩展的框架（MapReduce）和可扩展的存储（HBase）来计算和存储Cubes，当数据增长到原来的几倍后，Cube仍然会增长到一个难以接受的大小。

一般的解决途径是清空不必要的维度。正如我们之前在[优化Cube](http://kylin.apache.org/docs/howto/howto_optimize_cubes.html)中讨论的，清空可以通过以下两种方法实现：

首先，我们应该移除不必要的维度。比如，假设有一个日期的维度表，包含了作为分区列的`cal_dt`，和 `week_begin_dt`，`month_begin_dt`等很多衍生列。即使数据分析师会需要 `week_begin_dt`作为一个维度，我们仍然可以清空它。因为`week_begin_dt`可以由`cal_dt`计算得到。这就是衍生维度优化。

其次，有些维度的组合可以被清除。这也是本节主要讨论的问题，我们可以称之为“组合清空”。比如，如果一个维度被定义为“mandatory”，而其他所有未包含这个维度的组合都可以被清空。再比如，如果维度A，B，C形成了一个层级维度（Hierarchy），则只有A，AB，和ABC应被保留。在版本v1.5之前，Kylin也曾经提出过“聚合组”的概念，服务于清空不必要的组合。但它既没有很好地文档说明也难于理解（笔者也认为它很难解释清楚）。总之我们将跳过它，重新定义什么是“聚合组”。

在开源的实践中，我们发现了一些原始清空组合技术的严重弊端。首先，这些技术相互独立而不是经过系统性地良好设计的。此外，原始的聚合组概念设计和撰写地较为粗陋，在eBAY之外难以应用。其三，也是最重要的一点，原始的聚合组对于语义的表达比较模糊。

为了进一步描述语义表达的问题，我们将以下面的场景为例进行详述。假设一个交易数据的Cube，它具有一个高基维度`buyer_id`和其他很多普通的维度，像是交易日期`cal_dt`，买家所在的城市`city`等。分析师可能需要通过分组聚合`non-buyer_id` 维度来获得，例如只聚合 `cal_dt`。有时候，分析师需要通过以 `buyer_id`过滤下钻，来检查买家的行为。已知`buy_id`具有很高的基数，一旦 `buy_id`确定，相关的记录应该很少（也就是说仅适用基本的Cuboid来做一些时间上聚合的查询，集合出一些不需要的维度是可以的）此例中，清空规则应如下所示。但在Kylin v1.5之前，无法在既有的语义工具上添加这样的清空设置。

| Cuboid               | Compute or Skip | Reason                                   |
| -------------------- | --------------- | ---------------------------------------- |
| city                 | compute         | Group by location                        |
| cal_dt               | compute         | Group by date                            |
| buyer_id             | skip            | Group by buyer yield too many results to analyze, buyer_id should be used as a filter and used by visiting base cuboid |
| city,cal_dt          | compute         | Group by location and date               |
| city,buyer_id        | skip            | Group by buyer yield too many results to analyze, buyer_id should be used as a filter and used by visiting base cuboid |
| cal_dt,buyer_id      | skip            | Group by buyer yield too many results to analyze, buyer_id should be used as a filter and used by visiting base cuboid |
| city,cal_dt,buyer_id | compute         | Base cuboid                              |



## 设计聚合组

在Kylin v1.5中,我们通过 [jira issue]( https://issues.apache.org/jira/browse/KYLIN-242.) 讨论并重新设计了聚合组的设定机制。这个问题被命名为“Kylin Cuboid Whitelist”，因为新的聚合组设置使得Cube designer可以指定想要的Cuboid并将其保存在白名单中。

在新的设计中，聚合组（abbr. AGG）被定义为Cuboid云——受共享的规则约束。 Cube designer可以为一个Cube定义一个或多个AGG，并且联合所有AGGs约束的cuboids，使它们形成一个有效且无冗余的Cube。请注意一个Cuboid可以出现在多个AGGs中，且它只能在Cube构建时被计算一次。

如果你查看[内部的AGG](https://github.com/apache/kylin/blob/kylin-1.5.0/core-cube/src/main/java/org/apache/kylin/cube/model/AggregationGroup.java)，其中有两个重要的属性文件被定义如下： `@JsonProperty("includes")` 和 `@JsonProperty("select_rule")`。

`@JsonProperty("includes")`
这个属性文件是用来规定哪些维度可以被包括在AGG中的。属性的值必须是完整维度的子集.。保持适当的最小限度通过只包含必要的维度。

`@JsonProperty("select_rule")`
Select rules指所有有效的Cuboid都需要遵守的规则。Cube designer 可以定义多个规则，应用在所有被包含的维度中。现存的规则有三种：

- 层级维度规则，如上文所述

- 必需的维度规则，如上文所述

- 联合规则。这是一个新引入的规则。

  如果两个及以上的维度“Joint”, 则任何有效的Cuboid都不包含这些维度或者包含所有维度。换言之，这些维度永远都是“一起的”，当Cube designer 很确定某些维度总是一起被查询的时候这个规则就很有用。

  另一个“核级武器”则是将组合清除应用在很少用到的维度上。假设你有20个维度，前10个经常用到，后10个则很少用到。通过将后10个维度joint，你可以高效地将Cuboid数从2^20个减少至2^11个。实际上，这也就是原来的聚合组的机制。如果你曾使用Kylin v1.5之前的版本，我们的元数据升级工具将将其自动升级为joint语义。
  ​
  通过灵活地使用这些聚合组，理论上，你可以随意地控制任何一个Cuboid令其被计算或是被跳过。进而极大地减少计算量和存储开销，特别是当一个Cube服务于固定的仪表板——在特定的Cuboid上重复使用一些SQL查询。在极端情况下，你可以配置AGG使其值包含一个Cuboid，而另一些常用的AGG则包含白名单上你一定需要的Cuboid。

Kylin’s cuboid computation scheduler will arrange all the valid cuboids’ computation order based on AGG definition. You don’t need to care about how it’s implemented, because every cuboid will just got computed and computed only once. The only thing you need to keep in mind is: don’t abuse AGG. Leverage AGG’s select rules as much as possible, and avoid introducing a lot of “single cuboid AGG” unless it’s really necessary. Too many AGG is a burden for cuboid computation scheduler, as well as the query engine.

## Buyer_id 问题再观

现在我们有了新的AGG工具，buyer_id 问题就可以得到解决。我们只需要做以下两步来定义新的Cube聚合组：

- AGG1 包含： `[cal_dt, city, buyer_id] select_rules:{joint:[cal_dt,city,buyer_id]}`
- AGG2 包含： `[cal_dt,city] select rules:{}`

第一个AGG将只对应基础的Cuboid，第二个AGG将包括所有不含`buyer_id`的Cuboid。

## 开始使用

从Kylin v1.5 (KAP v2.1)开始，新聚合定义可以使用。对于使用过Kylin v1.5之前的Kylin的用户，你需要将元数据升级至最新版本。 
