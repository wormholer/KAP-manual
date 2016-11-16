# Integration with Tableau 8.x

> There are some limitations of Kylin ODBC driver with Tableau, please read carefully this instruction before you try it.
>
> * Only support "managed" analysis path, Kylin engine will raise exception for unexpected dimension or metric
> * Please always select Fact Table first, then add lookup tables with correct join condition (defined join type in cube)
> * Do not try to join between fact tables or lookup tables;
> * You can try to use high cardinality dimensions like seller id as Tableau Filter, but the engine will only return limited seller id in Tableau's filter now.

### Install Kylin ODBC Driver
Refer to this guide: [Kylin ODBC Driver Tutorial](./odbc.html).

Please make sure to download and install Kylin ODBC Driver __v1.5__. If you already installed ODBC Driver in your system, please uninstall it first.

### Connect to Kylin Server
> We recommended to use Connect Using Driver instead of Using DSN.

Connect Using Driver: Select "Other Database(ODBC)" in the left panel and choose KylinODBCDriver in the pop-up window. 

![](images/tableau_8/1 odbc.png)

Enter your Sever location and credentials: server host, port, username and password.

![](images/tableau_8//2 serverhost.jpg)

Click "Connect" to get the list of projects that you have permission to access. See details about permission in [Kylin Cube Permission Grant Tutorial](./acl.html). Then choose the project you want to connect in the drop down list. 

![](images/tableau_8//3 project.jpg)

Click "Done" to complete the connection.

![](images/tableau_8//4 done.jpg)

### Using Single Table or Multiple Tables
> Limitation
>
> * Must select FACT table first
> * Do not support select from lookup table only
>    * The join condition must match within cube definition

**Select Fact Table**

Select `Multiple Tables`.

![](images/tableau_8//5 multipleTable.jpg)

Then click `Add Table...` to add a fact table.

![](images/tableau_8/6 facttable.jpg)

![](images/tableau_8//6 facttable2.jpg)

**Select Look-up Table**

Click `Add Table...` to add a look-up table. 

![](images/tableau_8/7 lkptable.jpg)

Set up the join clause carefully. 

![](images/tableau_8/8 join.jpg)

Keep add tables through click `Add Table...` until all the look-up tables have been added properly. Give the connection a name for use in Tableau.

![](images/tableau_8/9 connName.jpg)

**Using Connect Live**

There are three types of `Data Connection`. Choose the `Connect Live` option. 

![](images/tableau_8/10 connectLive.jpg)

Then you can enjoy analyzing with Tableau.

![](images/tableau_8/11 analysis.jpg)

**Add additional look-up Tables**

Click `Data` in the top menu bar, select `Edit Tables...` to update the look-up table information.

![](images/tableau_8/12 edit tables.jpg)

### Using Customized SQL
To use customized SQL resembles using Single Table/Multiple Tables, except that you just need to paste your SQL in `Custom SQL` tab and take the same instruction as above.

![](images/tableau_8/19 custom.jpg)

### Publish to Tableau Server
Suppose you have finished making a dashboard with Tableau, you can publish it to Tableau Server.
Click `Server` in the top menu bar, select `Publish Workbook...`. 

![](images/tableau_8/14 publish.jpg)

Then sign in your Tableau Server and prepare to publish. 

![](images/tableau_8/16 prepare-publish.png)

If you're Using Driver Connect instead of DSN connect, you'll need to additionally embed your password in. Click the `Authentication` button at left bottom and select `Embedded Password`. Click `Publish` and you will see the result.

![](images/tableau_8/17 embedded-pwd.png)

### Tips
* Hide Table name in Tableau

* Tableau will display columns be grouped by source table name, but user may want to organize columns with different structure. Using "Group by Folder" in Tableau and Create Folders to group different columns.

![](images/tableau_8//18 groupby-folder.jpg)
