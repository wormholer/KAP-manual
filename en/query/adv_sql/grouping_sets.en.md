# Grouping Sets

Since KAP v2.1, we've provided grouping sets function in KAP to support when we want aggregate data by different keys in one SQL statements. Here we are going to introduce how to use this function. 



## Grouping Sets Function

Grouping sets function query supported by KAP is list as follow:

| Operator syntax                          | Description                              |
| ---------------------------------------- | ---------------------------------------- |
| GROUPING(expression)                     | Returns 1 if expression is rolled up in the current row’s grouping set, 0 otherwise |
| GROUP_ID()                               | Returns an integer that uniquely identifies the combination of grouping keys |
| GROUPING_ID(expression [, expression ] * ) | Returns a bit vector of the given grouping expressions |



## Example

In this section, we would take a dataset defaulted in KAP as data source to practice some typical query mentioned above. Step by step, we will introduce you how to use Grouping sets Function in KAP.

#### Dataset

Select a default **Data Source** named as `learn_kylin`, then the table structure would present below: there are one fact table (`KYLIN_SALES`) and two lookup tables (`KYLIN_CAL_DT` and `KYLIN_CATEGORY_GROUPINGS`). Take a minute to check the `KYLIN_SALES` as well as its sample data, and we'll use it later.

![](images/wd_datasample.png)

## Group query

To get sales of different brands(or products), input query as: `select LEAF_CATEG_ID, LSTG_FORMAT_NAME, sum(PRICE) as metric1 from kylin_sales group by LEAF_CATEG_ID, LSTG_FORMAT_NAME`, then results returned in 0.26sec would be as follow.

![](images/grouping_sets.1.png)



## Grouping sets query

If you also want to see the result with `lstg_format_name`(dim2) rolled up, we can rewrite the sql:

```
select LEAF_CATEG_ID,
case grouping(LSTG_FORMAT_NAME) when 1 then 'ALL' else LSTG_FORMAT_NAME end,
sum(PRICE) as metric1 from kylin_sales
group by grouping sets((LEAF_CATEG_ID, LSTG_FORMAT_NAME), (LEAF_CATEG_ID))
order by LEAF_CATEG_ID
```

 Then you would get results as below:

![](images/grouping_sets.2.png)

