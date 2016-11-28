## ODBC Driver

KAP provides an ODBC driver for Windows, through which applications with ODBC interface can access KAP to query data. 

Test the operating system: Windows 7, Windows Server 2008 R2

Test applications: Tableau 8.x, Table 9.x, Power Desktop, Excel

In this article, we take Windows 7 as an example to introduce ODBC driver's installation and use steps. 

## Prerequisites

1. Install Microsoft Visual C++ 2012（Redistributable）
   * 32 bits: download [32bit version](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe) 
   * 64 bits: download[64bit version](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe)
2. The ODBC driver would call KAP's Rest server, so make sure the KAP service is working properly.

## Installation
1. 如果你已经安装过KAP ODBC驱动，首先卸载已存在的Apache Kylin或KAP ODBC驱动。
2. 从[下载链接](http://kylin.apache.org/download)下载附件驱动安装程序，并运行。
   * 32位Microsoft Office Excel：请安装KylinODBCDriver (x86).exe
   * 64位Microsoft Office Excel：请安装KylinODBCDriver (x64).exe

## Configure the DSN

1. Open ODBC Data Source Manager

* 32 bits ODBC driver：click "start>operation" to open C:\Windows\SysWOW64\odbcad32.exe
* 64 bits ODBC driver：select "Control Panel> Administrative Tools" to open "ODBC Data Source Administrator" 


![](images/odbc/01.png)



2. Switch to the System DSN tab, click `Add` and select "KylinODBCDriver" in the pop-up driver selection box, then click `finish`.

![](images/odbc/02.png)

3. In the pop-up Kylin DSN Configuration Dialog, input the KAP server information, as shown in the figure: 

![](images/odbc/03.png)

Among them, the parameters are described below: 

* DSN Name: DSN----data source name
* Server Host: KAP server address
* Port: KAP server port number
* Username: username to login KAP 
* Password: password to login KAP 
* Project: the name of the KAP project to use for the query; after entering the blank space above, click `Connect` to activate the Project selection box 

4. Click `Done` 



## Special Reminder

If you want to use the ODBC driver to connect to other client applications, the configuration is similar to this example. More information please visit the [Integration with 3rd party BI tools](integration/README.md) chapter of KAP manual or Kylin's official website.