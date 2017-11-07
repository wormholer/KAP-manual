## Kyligence ODBC Driver

Kyligence ODBC Driver is a ODBC Driver that developed by Kyligence. It only supports the connection with KAP. It supports all functions of the Apache Kylin ODBC Driver. Applications with ODBC interface can access KAP through this driver.

Kyligence ODBC Driver is a part of KAP Product line. Users with KAP license can use Kyligence ODBC Driver directly without extra payment. The valid period of Kyligence ODBC Driver depends on that of KAP. 

Currently, Kyligence ODBC Driver is only supported in Windows. 

Next, we will take Windows 7 as an example to introduce ODBC driver's installation and configuration steps. 

## Prerequisites

1. Microsoft Visual C++ 2015 Redistributable is embedded in the install package. During the installation of Kyligence ODBC Driver, it will be installed first. If Microsoft Visual C++ 2015 Redistributable is already installed on your machine, this step will be skipped during installation.

2. Kyligence ODBC Driver woule connect to KAP's server, so make sure the KAP is running properly.

## Installation
1. If you have previously installed Kyligence ODBC driver, please uninstall it first.
2.  Apply and [Download](http://account.kyligence.io) Kyligence ODBC driver, and install it.

   For 32-bit application, please install and use kyligence_odbc.x86.exe.

   For 64-bit application, Please install and use kyligence_odbc.x64.exe.

## Configure the DSN

1. Open ODBC Data Source Manager:

   32-bit ODBC driver: click **start -> operation** to open C:\Windows\SysWOW64\odbcad32.exe

   64-bit ODBC driver: select **Control Panel -> Administrative Tools** to open **ODBC Data Source Administrator**

![ODBC Data Source Administrator](images/kyligence_odbc_01_en.png)

2. Switch to **System DSN** tab, click **Add** and select **KyligenceODBCDriver** in the pop-up driver selection box, then click **Finish**.


![Add Kyligence ODBC Driver](images/kyligence_odbc_02_en.png)

3. In the pop-up window, input the KAP server information, as shown in the figure: 

![DSN setting](images/kyligence_odbc_03_en.png)

Where, the parameters are described as below: 

* Data Source Name: name of data source
* Host: KAP server address
* Port: KAP server port number
* Username: username to login KAP
* Password: password to login KAP 
* Project: the name of the KAP project to use for the query

4. Click **Test**

   Once it connects to the data source successfully, the following dialog will appear.


![Connect Successfully](images/kyligence_odbc_04_en.png)


## Special Reminder

If you want to use Kyligence ODBC driver to connect to KAP in other client applications, the configuration is similar to this example. For more information, please see [Integration with 3rd party BI tools](../integration/README.md) chapter of KAP manual.
