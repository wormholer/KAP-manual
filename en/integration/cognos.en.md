## Integration with Cognos 10.X

### Install ODBC Driver

Refer to this guide: [Kylin ODBC Driver Tutorial](../driver/odbc.en.md).

Please make sure to download and install Kylin ODBC Driver v1.6 64 bit or above. If you already installed Kylin ODBC Driver in your system, please uninstall it first.  

The Kylin ODBC driver needs to be installed in the machine or virtual environment where your Cognos Server is installed.

### Create Local DSN

Open your window ODBC Data Source Administrator and create a system DSN that point to your KAP instance.

![](images/cognos/0.png)

### Create a Data Source in Cognos

Depending on your business scenario, you may need to create a new project or simply use an existing project to create the data source for KAP. In the example, we will start with a new project. 

![](images/cognos/1.png)

Next, use `Metadata Wizard` create a new `Data Source`.

![](images/cognos/2.png)

In the `New Data Source Wizard`, first fill in data source name, this could be any name you prefer.

![](images/cognos/3.png)

In next step, choose `ODBC` as the connection type. For Isolation Level, choose `Use the default object gateway`  

![](images/cognos/4.png)

Next, in ODBC data source, fill in the DSN name that you created in the previous step.

Check `Unicode ODBC`. For Signon `choose no authorization`.

Then Click `Test the connection`.

![](images/cognos/5.png)

![](images/cognos/6.png)

If everything set up properly, test the connection will finish successfully.

![](images/cognos/7.png)

![](images/cognos/8.png)

Now you have the data source created.

![](images/cognos/9.png)

###Test Connection

Next, you may test the connection with tables.

Click next to finish metadata wizard.

![](images/cognos/10.png)

![](images/cognos/11.png)

Now, this new data source has been imported into the project. You can right click on a table to test the connection.

![](images/cognos/12.png)

On the pop-up window, click `test sample` to test connection against this table. If the connection is correct, test results may return as shown in the screenshot below.

![](images/cognos/13.png)

### Publish Package

In the project viewer, right click on Packages->Create ->Package to publish data source as a package.

![](images/cognos/14.png)

Name the package.

![](images/cognos/15.png)

Choose the table and column that you want to publish.

![](images/cognos/16.png)

Leave the functions lists as default.

![](images/cognos/17.png)

Now you have successfully created a new package. Next, open the `publish package wizard`. 

You may leave the rest of the below settings as default, and finish the wizard.

![](images/cognos/18.png)

![](images/cognos/19.png)

![](images/cognos/20.png)

![](images/cognos/21.png)

![](images/cognos/22.png)

Now you have published the package.

###Create a Simple Chart

Now you can create a simple chart with newly the published package.

Go to your IBM Cognos Web. Launch Report Studio.

![](images/cognos/23.png)

Choose the package you just created in the previous step.

![](images/cognos/32.png)

In Cognos Report Studio, click `Create New`.

![](images/cognos/24.png)

Click `Chart` to create a new chart with the new package.

![](images/cognos/25.png)

Choose a chart type.

![](images/cognos/26.png)

Drag PRICE from Kylin_sales table to Measure, and drag LSTG_FORMAT_NAME to series. 

Then click the `Play` button on the top menu to run this report. 

![](images/cognos/27.png)

Now you have successfully created a new chart with KAP as the data source.

![](images/cognos/28.png)

### 



