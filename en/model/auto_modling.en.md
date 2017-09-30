## Auto-Modeling

模型设计与创建是使用KAP的关键步骤。模型从0到1的部分，最为考验用户对业务逻辑和查询需求的理解。许多用户希望能够依据SQL查询语句，自动地创建好一个可用的模型，方便地进入后续的模型优化或调整中。从KAP 2.5开始，我们提供您根据指定的SQL查询语句与一张指定的事实表，自动的创建模型的能力。

### Apply SQL to generate model

Step one, sync target tables to the project. 

![auto_sync_table](images/auto modeling/auto_sync_table.png)

第二步，将需要作为事实表的表拖入中央，并指定为事实表。点击事实表工具栏的“SQL”，将弹出一个可供输入查询语句的窗口。

![fact_table](images/auto modeling/fact_table.png)

![sql_window](images/auto modeling/sql_window.png)

第三步，输入你希望创建出来的模型能满足的查询语句，然后点击“检测”来确认这些SQL可以被正确的识别并输入KAP的计算引擎。如果全部正确识别，则如下图所示。

![valid_SQL](images/auto modeling/valid_SQL.png)

第四步，提交已经通过检测的SQL，之后你将得到一个已经被补全的模型，如下图所示。如果您对这个模型还有更多的修改，需要添加SQL语句，则可以酌情重复前面三步直至您满意。

![auto_modeling](images/auto modeling/auto_modeling.png)