## Cube Optimizer

Start from v2.5, KAP provides a ***multi-preference based Cube Optimizer*** to suggest cube designs, which helps reduce cube expansion and improve ***Query/Build*** performance.

### Introduction

According to best pratices of Cube tuning, Optimizer analyzes statistics of source data and SQL patterns, and suggests a optimized Cube design, which includes:

- ***Preference***: 

  - Data oriented: Optimizer would mainly digest source data feature to suggest one aggregate group, which optimizes all dimensions from model. Cubes follow data-oriented preference are suit to serve flexible queries.

    ![](images/Cube_optimizer/EN_data_oriented.png)

  - Business oriented: Optimizer would only digest SQL patterns to suggest multiple aggregate groups consisting of mandatory dimensions. Cubes follow business-oriented preference are designed to answer known queries.

    ![](images/Cube_optimizer/EN_buz_oriented.png)

  - Mix: Optimizer would combine two preference to serve queries, which contains some flexible queries and some known queries. The mix will cost more time and resource on build cube to meet satisfy two scenarios.

    ![](images/Cube_optimizer/EN_mix.png)

- ***Dimensions***: dimension and the type of dimension, such as ***Normal*** or ***Derived.***
- ***Measures***: suggest common aggregation mostly in entered SQL patterns as measures.
- ***Aggregation Groups***: Optimizer will suggest select rules for each group, such as Joint, Hierarchy, Max dimension combination(MDX) etc.
- ***Rowkey***: Optimizer will suggest order and configuration for each Rowkeys, such as Encoding.



In order to achieve accurate suggestion, Optimizer need following items as input:

- Preference: help you optimize cube exactly based on your query scenario

  > Note: when you choose one preference, the following input content may be the required condition to use cube optimizer later

- Model check: Model check must be completed before Optimize a cube, and the result is required input for Optimizer

- SQL patterns: Some history or target SQL statements, which guides the suggestion for Measures, Aggregation Groups and Rowkeys

### Steps

Step 1, To finish model check. Skip if already passed. To get more about model check, please click [here](../model_check.en.md).

Step 2, To create a cube with this model, and click "Collect SQL patterns" under "Cube Info" tabpage, and paste your SQL statements. For multiple SQLs, use ';' for seperation.

![](images/Cube_optimizer/suggestion_sql.png)



Step 3, Click "+ Dimensions" button on "Dimensions" tabpage and then the dimension window will pop up. You can select ***SQL output*** to get suggested dimensions from ***SQL patterns***, or select dimensions manually. All default dinmension type(normal/derived) are suggested by Cube Optimzier. 

![](images/Cube_optimizer/dimension.png)



Step 4, Click "Optimize" button under "Dimension optimizations" section, Aggregation groups will be filled with suggested rules, such as Mandatory, Hierarchy and Joint. Besides, configuration and order of Rowkeys will also be updated as suggestion.

Step 5, Click "SQL Output" button under "Measures" tabpage, Optimizer will fill suggest measures from SQL patterns.

![](images/Cube_optimizer/suggestion_measure.png)



Step 6, According to business requirements, users are able to make any adjustment to dimension, measures, aggregation groups, rowkeys, measures and so on. And save it at last.


