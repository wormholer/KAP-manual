## Table 
表， 是Cube的数据源；在创建Cube之前，KAP需要从数据源（通常为Hive）同步表的元数据，包含表名、列名、列属性等。

## Data Model
数据模型，定义了由若干张表的一个连接关系。KAP支持[星型模型](https://en.wikipedia.org/wiki/Star_schema)的多维分析；在创建Cube之前，用户需定义这么一个数据模型。

## Cube
数据立方体，是一种多维分析的技术，通过预计算，将计算结果存储在某多个维度值所映射的空间中；在运行时通过对Cube的再处理而快速获取结果。

## Partition
分区，用户可以定义一个分区日期或时间列，随后对Cube的构建按此列的值范围而进行，从而将Cube分成多个Segment。

## Cube Segment 
每个Cube Segment是对特定时间范围的数据计算而成的Cube。每个Segment对应一张HBase表。

## Aggregation Group 
聚合组，每个聚合组是全部维度的一个子集；通过将很多个维度分组，并把常一起使用的维度放在一起，可以有效降低Cube的组合数。

## Mandatory
必需的维度：这种类型用于对Cube生成树做剪枝：如果一个维度被标记为“Mandatory”，会认为所有的查询都会包含此维度，故所有不含此维度的组合，在Cube构建时都会被剪枝（不计算）.

## Hierarchy 
层级维度：如果多个维度之间有层级（或包含）的关系，通过设置为“Hierarchy”，那些不满足层级的组合会被剪枝。如果A, B, C是层级，并且A>B>C，那么只需要计算组合A, AB, ABC; 其它组合如B, C, BC, AC将不做预计算。 

## Derived 
衍生维度：维度表的列值，可以从它的主键值衍生而来，那么通过将这些列定义为衍生维度，可以仅将主键加入到Cube的预计算来，而在运行时通过使用维度表的快照，衍生出非PK列的值，从而起到降维的效果。

## Count Distinct(HyperLogLog) 
基于HyperLogLog的Count Distint：快速、精确的COUNT DISTINCT是较难计算的, 一个近似的轻量级算法 - [HyperLogLog](https://en.wikipedia.org/wiki/HyperLogLog) 为此而发明, 能够在大规模数据集上做去重并保持较低的误差率. 

## Count Distinct(Bitmap) 
基于Bitmap的COUNT DISTINCT，可以精确去重，但是存储开销较大。目前只支持int的数据类型.

## Top N 
预计算最top的某些记录的度量，如交易量最大的1000个卖家。

## BUILD 
构建：给定一个时间范围，将源数据通过运算而生成一个新的Cube Segment。

## REFRESH
刷新：对某个已经构建过的Cube Segment，重新从数据源抽取数据并构建，从而获得更新。

## MERGE
合并：将多个Cube Segment合并为一个Segment。这个操作可以减少Segment的数量，同时减少Cube的存储空间。

## PURGE 
清空：将Cube的所有Cube Segment删除。

## NEW 
新任务，刚刚创建。

## PENDING 
等待被调度执行的任务.

## RUNNING 
正在运行的任务。

## FINISHED 
正常完成的任务（终态）。

## ERROR 
执行出错的任务。

## DISCARDED
丢弃的任务（终态）。

## RESUME 
恢复：处于ERROR状态的任务，用户在排查或解决问题后，通过此操作来重试执行。

## DISCARD
丢弃：放弃此任务，立即停止执行并不会再恢复。

