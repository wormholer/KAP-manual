## ODBC 驱动程序

KAP 提供了 Windows 系统下的 ODBC 驱动程序，支持 ODBC 接口的应用可以通过该驱动程序访问 KAP 进行数据查询。

测试操作系统：Windows 7 和 Windows Server 2008 R2
测试应用：Tableau 8.x、Table 9.x、Power Desktop 和 Excel

在本文中，我们以 Windows 7 和 Microsoft Office Excel 为例，介绍 ODBC 驱动程序的安装和使用步骤。

## 前提条件 

1. 安装 Microsoft Visual C++ 2012 (Redistributable)：

- - [32 位系统下载地址](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe)（推荐）

- - [64 位系统下载地址](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe)

2. ODBC 驱动程序会调用 KAP 的 Rest 服务器，务必先确保 KAP 服务已正常运行。


## 安装

1. 如果已经安装过 KAP ODBC 驱动程序，首先卸载机器上已有的 Apache Kylin 或 KAP ODBC 驱动程序。
2. 从 [Apache Kylin 下载页面](http://kylin.apache.org/download)下载 ODBP 驱动程序，并运行。
* 32 位 Microsoft Office Excel：请安装 KylinODBCDriver \(x86\).exe
* 64 位 Microsoft Office Excel：请安装 KylinODBCDriver \(x64\).exe


## 配置DSN

1. 打开ODBC数据源管理器

* 32位ODBC驱动：请在“开始&gt;运行”中打开C:\Windows\SysWOW64\odbcad32.exe


* 64位ODBC驱动：请直接在“控制面板&gt;管理工具”中找到“ODBC Data Source Administrator”并打开

![](images/odbc_01.png)

2. 切换到系统DSN选项卡，单击“Add”，在弹出的驱动程序选择框中选择“KylinODBCDriver”并单击Finish按钮。

![](images/odbc_02.png)

3. 在弹出的Kylin DSN Configuration Dialog中输入KAP服务器信息，如图所示

![](images/odbc_03.png)

其中，各项参数介绍如下：

* DSN Name: DSN数据源名称
* Server Host: KAP服务器地址
* Port: KAP服务器端口号
* Username: KAP服务登录用户名
* Password: KAP服务登录密码
* Project: 查询所使用的KAP项目名称；输入完上面空白后单击Connect按钮，才可激活Project选择框

4. 单击Done按钮



## 特别提醒

如果用户希望使用ODBC驱动连接其他客户端应用，配置方式和该例类似。更多产品信息，请访问本书[与第三方BI工具集成](../integration/README.md)章节或 Apache Kylin官网。

