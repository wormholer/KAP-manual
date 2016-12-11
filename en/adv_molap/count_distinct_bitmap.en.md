# Use Precise Count Distinct in KAP

Count distinct is a frequent-used function for most data analysts. Since KAP v2.1, KAP implements precise count distinct based on bitmap. For the data with type tiny int(byte), small int(short) and int, project the value into the bitmap directly. For the data with type long, string and others, encode the value as String into a dict, and project the dict id into the bitmap. The result of measure is the serialized data of bitmap, not just the count value. This ensures results are always correct within any segment, even roll-up across segments. 



## Prerequisite

Before using count distinct query, you need to clarify if the target column is ready for it. You can get measures information by checking `measures` of built `Cube`(as shown below). If the measure desired has been pre-calculated on precise count distinct syntax(here requires both `Expression` to be count_distinct and `Return Type` to be bitmap), and stored within the Cube information, then this measure is ready to do further count distinct query. Otherwise, you need to create a new Cube.



## Precise Count Distinct Setting 

Firstly, after creating a new Cube and ensure all dimensions selected, then click `Measures+` on the lower left corner to start measures setting.  

![](image/cd_measures_add.4.png)

Next, choose the column desired from `Param Value` and COUNT_DISTINCT from `Expression`. Here be careful to select accuracy requirement from `Return Type`.  KAP offers both approximate count distinct function and precise count distinct function. To get the pre-calculated precise count distinct value, you should select  `Return Type: Precisely` based on bitmap, which would return a no error result if storage resource is sufficient enough. For instance, one result size might be hundreds of MB, when the count distinct value over millions.

![](image/cd_measures_add.2.png)



There is another little setting difference on`Advanced Setting`. To complete precise count distinct setting, which means to get correct results from precise count distinct function not only within a segment, but also across several segments when it is roll-up. So we need to add a global dictionary within `Advanced Dictionaries`.   ![](image/cd_measures_add.5.png)

Select `Dictionaries+` , then choose desired columns as Column and the global dictionary shown below as its `Builder Class` . Follow the [Create Cube](molap/create_cube.en.md) introduction for rest steps, the Cube would be ready after you set segments during the [Build Cube](molap/build_cube.en.md) section.

![](image/cd_meausres_add.6.png)

## Example

Select a default **Data Source** named as `learn_kylin`, then the table structure would present below: there are one fact table (`KYLIN_SALES`) and two lookup tables (`KYLIN_CAL_DT` and `KYLIN_CATEGORY_GROUPINGS`). Take a minute to check the `KYLIN_SALES` as well as its sample data, and we'll use it later.

![](image/wd_datasample.png)



Input `select count(distinct seller_id) as seller_num from kylin_sales where part_dt= DATE '2012-01-02'` query in **Insight** dashboard, then result returned in 0.18sec.  

![](image/cd_measures_add.7.png)

Next input `select count(distinct seller_id) as seller_num from kylin_sales where part_dt in ('2012-01-02','2013-01-02')`  and we got the result shown below in 0.25sec. 

![](image/cd_measures_add.8.png)



Both results are testified right, which prove that precise count distinct query works well and correctly. More information about approximate count distinct function, please refer to [Approximate Count Distinct](adv_molap/count_distinct_hllc.en.md) Introduction.



## Global Dictionary

KAP encodes values into dictionay at the segment level by default. That means one value in different segments maybe encoded into different ID, then the result of count distinct will be incorrect. In v2.1 we introduce “Global Dictionary” with ensurance that one value always be encoded into the same ID across different segments. Meanwhile, the capacity of dictionary has expanded dramatically, upper to support 2 billion values in one dictionary. It can also be used to replace the default dictionary which has 5 million values limitation. 

The global dictionay cannot be used for dimension encoding for now, that means if one column is used for both dimension and count distinct measure in one cube, its dimension encoding should be others instead of dict. 

### Reference

[Use Count Distinct in Apache Kylin](http://kylin.apache.org/blog/2016/08/01/count-distinct-in-kylin/) (Yerui Sun)

