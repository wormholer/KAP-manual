## Excel 2016 Integration

Microsoft Excel is one of the most famous data tool on Windows platform, and has plenty of data analyzing functions. With Power Query installed as plug-in, excel can easily read data from ODBC data source and fill spreadsheets. 

### Install ODBC Driver
Refer to this guide: [Kylin ODBC Driver Tutorial](./odbc.html).
Please make sure to download and install Kylin ODBC Driver __v1.5__. If you already installed ODBC Driver in your system, please uninstall it first.  

### Connect Excel to KAP
1. Run Excel 2016(with built-in power query), switch to `Power Query` fast tab, click `From Other Sources` dropdown list, and select `ODBC` item.
   ![](images/excel_2016/excel_1.PNG)

2. You will see`From ODBC`dialog, you can choose DSN`kylin` (Noticed：DSN referring to a specific data source. If you want to change data source, please rebuild connection with Kylin ODBC driver); or choose`Advanced options`, and type Database Connection String of KAP Server in the `Connection String` textbox. Optionally you can type a SQL statement in `SQL statement` textbox. Click `OK`, result set will run to your spreadsheet now.

   ![](images/excel_2016/excel_3.PNG)
   ![](images/excel_2016/excel_4.PNG)

> Tips: In order to simplify the Database Connection String, DSN is recommended, which can shorten the Connection String like `DSN=[YOUR_DSN_NAME]`. Details about DSN, refer to [https://support.microsoft.com/en-us/kb/305599](https://support.microsoft.com/en-us/kb/305599).


3. If you didn’t input the SQL statement in last step, Power Query will list all tables in the project, which means you can load data from the whole table. But, since KAP cannot query on raw data currently, this function may be limited.
   ![](images/excel_2016/excel_6.png)
4. Hold on for a while, the data is lying in Excel now.
   ![](images/excel_2016/excel_7.png)
5. If you want to sync data with KAP Server, just right click the data source in right panel, and select `Refresh`, then you’ll see the latest data.
