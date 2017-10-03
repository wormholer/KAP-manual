## Model Advisor

Model design is a key part to use KAP. Because the initial version of model design si so engaging business insight from modeler. To lower the learn curve of modeling, enable business professional to generate models directly from SQLs. We features the model advisor to meet this demand: all you need to do is drag one table into the model designer and set it as fact table, the entering SQLs will complete the rest part of model design. 

### Apply SQL to generate  a desired model

Step 1, sync target tables to the project. 

![sync table](images/auto modeling/auto_sync_table.png)

Step 2, drag one table, which will be the fact table, to the model designer and set it as *fact*. Click ***SQL*** button to pop up a dialog to collect your SQL statements. 

> Note: only fact table will have this button and SQL queries here inputed should be the SQL you would query later.
>

![set fact table](images/auto modeling/fact_table.png)

![collect SQL](images/auto modeling/sql_window.png)

Step 3, enter the SQLs that would cross this model. You should ***check***  these SQLs to make sure which can be used to generate model, because only correctly recoganized ones can work. If all SQLs are right to the model advisor, the dialog would look as follow picture.

![valid SQLs](images/auto modeling/valid_SQL.png)

Step 4, ***submit*** checked SQLs and you will get a initial model from your SQLs as below. Please do modify, check and re-submit SQLs via ***model advisor***.  When you find all good, save it.

![model advisor complete once](images/auto modeling/auto_modeling.png)