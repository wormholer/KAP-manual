## Kyligence ODBC Driver

Kyligence ODBC Driver is developed by Kyligence independently. It only supports the connection with KAP, instead of with Apache Kylin. It supports all functions of the original Apache Kylin ODBC Driver. The applications with ODBC interface can access KAP to query data through this driver.

Kyligence ODBC Driver is a part of KAP business release. Users with KAP license can use Kyligence ODBC Driver directly without extra payment. The valid period of Kyligence ODBC Driver depends on that of KAP. 

Currently, Kyligence ODBC Driver is only supported in Windows, and will be available in other systems in the near future. 

In this article, we take Windows 7 as an example to introduce ODBC driver's installation and configuration steps. 

## Prerequisites

1. Uninstall Microsoft Visual C++ 2015 Redistributable, if installed. Microsoft Visual C++ 2015 Redistributable is embedded in the install package. During the installation of Kyligence ODBC Driver, it will be installed first.

2. Kyligence ODBC Driver would invoke KAP's Rest server, so make sure the KAP service is working properly.

## Installation
1. If you have installed such ODBC driver, please uninstall them first.
2.  [Download](http://account.kyligence.io) Kyligence ODBP drivers, and run.

   32-bit Microsoft Office Excel: Please download and install kyligence_odbc.x86.exe

   64-bit Microsoft Office Excel: Please download and install kyligence_odbc.x64.exe

## Configure the DSN

1. Open ODBC Data Source Manager:

   32-bit ODBC driver: click **start -> operation** to open C:\Windows\SysWOW64\odbcad32.exe

   64-bit ODBC driver: select **Control Panel -> Administrative Tools** to open **ODBC Data Source Administrator**

![ODBC Data Source Administrator](images/kyligence_odbc_01_en.png)

2. Switch to **System DSN** tab, click **Add** and select **KyligenceODBCDriver** in the pop-up driver selection box, then click **Finish**.


![Add Kyligence ODBC Driver](images/kyligence_odbc_02_en.png)

3. In the pop-up Kyligence ODBC Driver 2.0 Beta - DSN Setup dialog, input the KAP server information, as shown in the figure: 

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

If you want to use Kyligence ODBC driver to connect to other client applications, the configuration is similar to this example. For more information, please see [Integration with 3rd party BI tools](../integration/README.md) chapter of KAP manual.