

## Introduction to Computed Column


**Computed Column** allows you to pre-define operations like data extraction/transformation/redefinition in modes, and thus enhance the data semantic abstraction. By replacing runtime calculation with offline cube construction, KAP's pre-calculation capability is fully utilized. As a result, query performance could improve significantly. It's allowed to use Hive UDF in computed column, so that existing business codes can be reused.


### Create Computed Column

KAP allows you to define computed columns for each model seprately. A column column is based on specific table in the table, using one or more columns on that table to form an expression. For example, say you have a fact table named `kylin_sales` with following columns: `price` (price for each item in the transaction), `item_count` (number of sold items in the transaction) and `part_dt` (time when the transaction happens). You can define two more computed columns on `kylin_sales`: `total_amount = price * item_count` and `deal_year = year(part_dt)`. Thus, later when creating a cube, you can not only define dimensions/measures based on original columns price/item_count/part_dt, but also from newly added computed columns total_amount/deal_year.

You can create computed columns by clicking the icon as the arrow points to:

![](images/computed_column_en.1.png)


the following information is required:

![](images/computed_column_en.2.png)

+ **Column**：Display name of the created column
+ **Expression**：Definition of the computed column. Notice no columns from other tables is allowed to appear here.
+ **Data Type**：The data type of the created column

After defining the computed columns in model, you need to use them to build cube (either as dimension or measure), so that computed column can be pre-calculated and performance advantages can be observed.


![](images/computed_column_en.3.png)

### Explicit Query vs. Implicit Query

A computed column is logically appended to the table's column list after creation. You can query the column as if it were normal columns under the premise that the computed column is included by a ready **Cube**/**TableIndex** or **Query Pushdown** is enabled. Continuing with the last example, if you created and built a cube containing measure `sum(total_amount)`, KAP can answer queries like `select sum(total_amount) from kylin_sales`. We call it **Explicit Query** on computed columns. 

Or, the your can pretend that computed column is invisible from the table, and still use the expression behind the computed column to query. Continuing with the last example, when your query `select sum(price * item_count) from kylin_sales`, KAP will analyze the query and figure out that expression in `price * item_count` is replacable by an existing computed column named `total_amount`. For better performance KAP will try to translate your original query to `select sum(total_amount) from kylin_sales`. We call it **Implicit Query** on computed columns.

**Implicit Query** is not enabled by default. To enable it you'll need to add `kylin.query.transformers=io.kyligence.kap.query.util.ConvertToComputedColumn` in `KYLIN_HOME/conf/kylin.properties`

## Rule using Computed Column##

· As of KAP 2.4.1, computed column can only be defined on face table (cannot be defined on dimension nor cross tables)

· Under one project, there is one-to-one mapping between computed column and formula defined. That means under different models, computed column with same name can be defined on the condition that two computed columns share the same formula. 

· Under one project, computed column cannot have the duplicated column with column on source table.



## Advanced Function##

Computed column is pushed down to data source to be calculated. Hive is the default data source for KAP, thus the syntax for computed need to follow hive's. 

It is possible to utilize Hive embedded function or Hive User Defined Function in computed column. To learn more about what function Hive SQL offers, please refer to Hive documentation below:

https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-StringFunctions

