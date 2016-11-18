## Power BI Integration

Microsoft Power BI is a business intelligence tool providing rich functionality and experience for data visualization and processing to user.

> Power BI does not support "connect live" model for other ODBC driver yet, please pay attention when you query on a huge dataset, it may pull too many data into your client which will take a while even fail at the end.

### Install ODBC Driver
Refer to this guide: [Kylin ODBC Driver Tutorial](./odbc.html).
Please make sure to download and install Kylin ODBC Driver __v1.5__. If you already installed ODBC Driver in your system, please uninstall it first. 

### Power BI
1.  Run Power BI Desktop, and click `Get Data` button, then select `ODBC` as data source type.
    ![](images/powerbi/Picture5.png)
2.  Same with Excel, just type Database Connection String of KAP Server in the `Connection String` textbox, and optionally type a SQL statement in `SQL statement` textbox. Click `OK`, the result set will come to Power BI as a new data source query.
     ![](images/powerbi/Picture6.png)
3.  If you didn’t input the SQL statement in last step, Power BI will list all tables in the project, which means you can load data from the whole table. But, since KAP
4.  cannot query on raw data currently, this function may be limited.
     ![](images/powerbi/Picture7.png)
5.  Now you can start to enjoy analyzing with Power BI.
     ![](images/powerbi/Picture8.png)
6.  To reload the data and redraw the charts, just click `Refresh` button in `Home` fast tab.

