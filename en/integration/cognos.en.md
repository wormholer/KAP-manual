## Integration with Cognos 10.X

### Install ODBC Driver

Refer to this guide : [Kylin ODBC Driver Tutorial](../driver/odbc.en.md).

Please make sure to download and install Kylin ODBC Driver v1.6 64 bit or above. If you already installed Kylin ODBC Driver in your system, please uninstall it first.  

The kylin ODBC driver needs to be installed in the machine or virtual environment where your Cognos Server is installed.

### Create Local DSN

Open your window ODBC Data Source Administrator and create a system DSN that point to your KAP instance.

![](images/cognos/0.png)

### Create a Data Sourcein Cognos

Depending on your business scenario, you may need to create a new project or simply use existing project to create data source for KAP. In the example, we will start with a new project. 

![](images/cognos/1.png)

Next, create a new Data Sources

![](images/cognos/2.png)

In the New Data Source Wizard, first fill in data source name, this could be any name you prefer.

![](images/cognos/3.png)

In next step, choose `ODBC` as the connection type. For Isolation Level, choose `Use the default object geteway`  

![](images/cognos/4.png)

Next in ODBC data source, fill in the DSN name that you created in the previous step.

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

Next you may test the connection with tables.

![](images/cognos/10.png)