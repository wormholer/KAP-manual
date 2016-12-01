# KyBot Case Analysis

We have met a case that an e-commerce company had run Apache Kylin over 21 nodes, they turned to KyBot and looking forward to a diagnose about their cluster health status and advices on Cube optimization. KyBot analyzed a five-day time span diagnostic pack, which including 98 Cubes as well as over 7000 Queries, ultermately identified the bottleneck of query process and optimized it.

## Overall situation

**98** Cube, its total size is **871GB** and with a quite low expansion rate;
Over **86%** queries are completed within **1 second**, **98%** queries are completed within 5 second. Besides, average query performance goes greatly;
The medium of building’s time comsumed is below 15mins, which is in the normal range;

![](https://kyligence.io/wp-content/uploads/2016/11/01-1.png)


## SQL Execution Analysis

```sql
Select CATEGORY_LV1, sum(order_amt) as order_amt, sum(payment_amt) as payment_amt, sum(discount_amt) as discount_amt, sum(shipping_fee) as shipping_fee, sum(tax_amt) astax_amt, sum(coupon_amt) as coupon_amt, count(distinct CUSTOMER_ID) as uv, count(distinct SHIPPING_AGT_ID) as shipping_agt, count(distinct province_id) as province from t_sales_order WHERE PART_DT > ’20160901’ and  PART_DT < ’20161001’ group by CATEGORY_LV1 order by CATEGORY_LV1
```

### Cube Index’s Matching Degree Analysis
There are 8 dimensions displayed on the SQL executing process page, showing that 8 dimensions within a Cube has been involved in the SQL execution. Only the PART_DT is an effective filtering dimension and the CATEGORY_LV1 is a working aggregation group dimension, the rest dimensions are mandatory to participate execution yet not matchable for the case, causing a low match degree overall(25%). Uses are capable to cancel this mandatory setting or use joint instead.

The filtering dimension PART_DT is at the end of the index combination; considering that front dimensions have an ultra high cardinality that leading a costly filtering time comsuming and inefficient query, we sincerely suggest users update ranking of index dimensions.

![](https://kyligence.io/wp-content/uploads/2016/11/02-1.png)

### SQL Execute Life Cycle Analysis

As the SQL executing life cycle analysis diagram shows, the blue part refers to SQL works on parallel scaning among multiple storage nodes, the green part refers to SQL executtions on query nodes. The green part is much longer, suggesting that executing query bottleneck is on query nodes. Due to the reason before, we advise users to reduce data post aggregation pressure or improve query nodes performance.

![](https://kyligence.io/wp-content/uploads/2016/11/03-1.png)

## Optimizing Action

Add a Hierarchy aggregation group, which includes CATEGORY_LV1 and CATEGORY_LV2; and add a Joint aggregation group, which includes SELLER_ID and SHOP_NAME to cancle the defaulted Mondatory aggregation group.

## Optimizing Result

Queries hit a more matchable Cuboid, leading to a great improvement on query efficiency, and query node’s running time has been shorten to 0.4 seconds.

## Customer Feedback

According to KyBot’s analysis, correspondingly, we optimized Cube and increased internal storage of query nodes. Comparison tests show that the query efficiency is significantly improved. Next, we will continue to optimize Cube according to the further analysis.