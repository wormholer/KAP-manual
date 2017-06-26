## Basic Query

When build completes, the status of cube will become "Ready", meaning it is ready to serve query. In this section we use KAP sample data to introduce how to do simple SQL query in KAP.

> Note: In production, it is more common to connect KAP via a programming interface, like Rest API or ODBC/JDBC. However KAP also provides a query UI to facilitate simple SQL ability mostly for demo and testing.

To run a query, first select the `Kylin_Sample_1` project, and then go to the "Insight" page. Find a big input box where you can enter SQL according to the relational data model. Click "Submit" button to run a query.Below is a few SQL examples and their result.  

### Row Count of a Single Table

```
SELECT COUNT(*) FROM KYLIN_SALES
```

This query returns the total row count of KYLIN_SALES table. User can run the same query in Hive to compare correctness and speed. For reference, in the writer's test, Hive query took 29.385 seconds, while KAP returned in 0.18 second. 

### Joint of Multiple Tables

```
SELECT KYLIN_SALES.PART_DT,KYLIN_SALES.LEAF_CATEG_ID,KYLIN_SALES.LSTG_SITE_ID,KYLIN_CATEGORY_GROUPINGS.META_CATEG_NAME,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL2NAME,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL3NAME,KYLIN_SALES.LSTG_FORMAT_NAME,SUM(KYLIN_SALES.PRICE),COUNT(DISTINCT KYLIN_SALES.SELLER_ID)FROM KYLIN_SALES as KYLIN_SALES INNER JOIN KYLIN_CAL_DT as KYLIN_CAL_DTON KYLIN_SALES.PART_DT = KYLIN_CAL_DT.CAL_DTINNER JOIN KYLIN_CATEGORY_GROUPINGS as KYLIN_CATEGORY_GROUPINGSON KYLIN_SALES.LEAF_CATEG_ID = KYLIN_CATEGORY_GROUPINGS.LEAF_CATEG_ID AND KYLIN_SALES.LSTG_SITE_ID = KYLIN_CATEGORY_GROUPINGS.SITE_IDGROUP BY KYLIN_SALES.PART_DT,KYLIN_SALES.LEAF_CATEG_ID,KYLIN_SALES.LSTG_SITE_ID,KYLIN_CATEGORY_GROUPINGS.META_CATEG_NAME,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL2NAME,KYLIN_CATEGORY_GROUPINGS.CATEG_LVL3NAME,KYLIN_SALES.LSTG_FORMAT_NAME
```

This SQL joins KYLIN_SALES and two lookup tables, sums sales price group by date and category. In writer's test, Hive query took 34.361 seconds and KAP returned in 0.33 second. 

### COUNT_DISTINCT on Dimension Column

```
SELECTCOUNT(DISTINCT KYLIN_SALES.PART_DT)FROM KYLIN_SALES as KYLIN_SALES INNER JOIN KYLIN_CAL_DT as KYLIN_CAL_DTON KYLIN_SALES.PART_DT = KYLIN_CAL_DT.CAL_DTINNER JOIN KYLIN_CATEGORY_GROUPINGS as KYLIN_CATEGORY_GROUPINGSON KYLIN_SALES.LEAF_CATEG_ID = KYLIN_CATEGORY_GROUPINGS.LEAF_CATEG_ID AND KYLIN_SALES.LSTG_SITE_ID = KYLIN_CATEGORY_GROUPINGS.SITE_ID
```

This SQL queries distinct count on the PART_DT column, which is not a defined measure. In this case, KAP will do calculation on-the-fly and still quickly returns. In writer's test, Hive query took 44.911 seconds and KAP returned in 0.12 second.

### Table Index Scan

```
SELECT * FROM KYLIN_SALES
```

By default, KAP does not memorize raw records, thus cannot answer queries that does not have a `GROUP BY` clause. However, user often like to "`SELECT *`" to peek a few sample records. In such cases, KAP will return result at the best effort, by grouping all dimensions implicitly. Such result is not accurate but gives a signal to user that the cube is loaded with good data.If user wants KAP to store and return raw records, please define table index(raw table) in cube definition.