## Integration with Qlik Sense

Qlik Sense delivers intuitive platform solutions for self-service data visualization, guided analytics applications, embedded analytics and reporting. It is a new player in the Business Intelligence (BI) tools world, with a high growth since 2013. It has connectors with Hadoop Database (Hive and Impala). Now it can integrate with KAP. This article will guide you through the process of connecting KAP with Qlik Sense.  

### Pre-requisites

Before integrating with Qlik Sense, please make sure to download and install the following required applications. If you already installed Kylin ODBC Driver in your machine, please uninstall it first.

- [Kylin ODBC](http://kylin.apache.org/docs15/tutorial/odbc.html) 
- [Qlik Sense Desktop](http://www.qlik.com/us/try-or-buy/download-qlikview) 


You may find the detailed installation and configuration guides as below.

#### Configure Local DSN

Please refer to the guide in the page of [ODBC Driver](http://docs.kyligence.io/v2.5/en/driver/odbc.en.html) to install and configure your local DSN. After going through the steps in this page, you will see the pup-up dialog **Apache Kylin DSN Configuration** as blow, please select the project of **learn_kylin**, then click **Done**.

![apache_kylin_dsn_configuration](images/Qlik/apache_kylin_dsn_configuration.png)
*Figure 1. Apache Kylin DSN Configuration*

#### Install Qlik Sense

For the installation of Qlik Sense, please visit [Qlik Sense Desktop download](https://www.qlik.com/us/try-or-buy/download-qlik-sense).

### Connection with Qlik Sense
After configuring your Local DSN and installing Qlik Sense successfully, you may go through the following steps to connect Kylin with Qlik Sense.

1. From Windows Desktop Shortcut or click **Start -> All Applications -> Qlik Sense -> Qlik Sense Desktop** to open the application **Qlik Sense Desktop**.

2. Input your Qlik account to login, then the following dialog will pop up. Click **Create New Application**.

![welcome_to_qlik_desktop](images/qlik/welcome_to_qlik_desktop.png)
*Figure 2. Create New Application*

You may specify any name different from existing applications and then open this application. In this example, we name it as “Kylinfortesting".

![create_new_application](images/qlik/create_new_application.png)
*Figure 3. Specify a unique name for the new application*

3. There are two choices in the Application View. Please select the bottom **Script Editor**.

![script_editor](images/qlik/script_editor.png)
*Figure 4. Select Script Editor*

The Kylinfortesting | Data Load Editor window shows. Click **Create New Connection** in the upper right of this page.

![create_data_connection](images/qlik/create_data_connection.png)
*Figure 5. Create New Data Connection*

Select **ODBC -> kylin**, ignore the account information, and then click **Create**. 

![odbc_connection](images/qlik/odbc_connection.png)
*Figure 6. ODBC Connection*

4. Change the default scripts of "TimeFormat", "DateFormat" and "TimestampFormat" to:

`SET TimeFormat='h:mm:ss';`
`SET DateFormat='YYYY-MM-DD';`
`SET TimestampFormat='YYYY-MM-DD h:mm:ss[.fff]';`

5. Load columns from KAP to Qlik Sense:

As we have logged in KAP and select the project **learn_kylin** during local DSN configuration, currently we only need to new a query **select * from KYLIN_SALES** in KAP.

![kap_query](images/qlik/kap_query.png)
*Figure 7. Query in KAP*

Export the query results and copy them to the script editor in Qlik Sense Desktop with comma as delimiter. 

![script_run_result](images/qlik/script_run_result.png)
*Figure 8. Script Running*

In the running result, the line count shall be the same as in KAP. In this case, the line count is 10,000.

![load_data](images/qlik/load_data.png)
*Figure 9. Script Running Result*

6. Load data to Qlik Sense:

After the scripts run successfully, click **Load Data** in the upper right of the window Kylinfortesting | Data Load Editor. And then open **Data Manager**.

![open_data_manager](images/qlik/open_data_manager.png)
*Figure 10. Open Data Manager*

Now Data manager opens. Click **Load Data** in the upper right of this page, and click **Edit worksheet** once data loaded.

![data_load2](images/qlik/data_load2.png)
*Figure 11. Load Data in Data Manager*

Select the charts you need, then add dimension and measurement based on your requirements. 

![add_dimension](images/qlik/add_dimension.png)
*Figure 12. Select the required charts, dimension and measure*

You will get your worksheet and the connection is complete. Your KAP data shows in Qlik Sense now.

![final](images/qlik/final.png)
*Figure 13. View KAP data in Qlik Sense*