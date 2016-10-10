---
layout: docs15-cn
title:  ODBC 驱动程序教程
categories: 教程
permalink: /cn/docs15/tutorial/odbc.html
version: v1.2
since: v0.7.1
---

## ODBC 驱动程序

KAP提供了Windows系统下的ODBC驱动程序，支持ODBC接口的应用可以通过该驱动程序访问KAP进行数据查询。

测试操作系统：Windows 7, Windows Server 2008 R2
测试应用：Tableau 8.x, Table 9.x, Power Desktop, Excel

在本文中，我们以Windows 7和Microsoft Office Excel为例，介绍ODBC驱动的安装和使用步骤。

## 前提条件
1. 安装Microsoft Visual C++ 2012（Redistributable）
   * 32位：下载：[32bit version](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe) 
   * 64位：下载：[64bit version](http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe)

2. ODBC驱动程序会调用KAP的Rest服务器，请提前确保KAP服务已正常运行。

## 安装
1. 如果你已经安装过KAP ODBC驱动，首先卸载已存在的Apache Kylin或KAP ODBC驱动。
2. 从[下载链接](http://kylin.apache.org/download)下载附件驱动安装程序，并运行。
   * 32位Microsoft Office Excel：请安装KylinODBCDriver (x86).exe
   * 64位Microsoft Office Excel：请安装KylinODBCDriver (x64).exe

## 配置DSN
1.打开ODBC数据源管理器
   * 32位ODBC驱动：请在“开始>运行”中打开C:\Windows\SysWOW64\odbcad32.exe
   * 64位ODBC驱动：请直接在“控制面板>管理工具”中找到“ODBC Data Source Administrator”并打开
  
![](/images/driver/odbc/01.png)

2.切换到系统DSN选项卡，单击“Add”，在弹出的驱动程序选择框中选择“KylinODBCDriver”并单击Finish按钮

![](/images/driver/odbc/02.png)

3.在弹出的Kylin DSN Configuration Dialog中输入KAP服务器信息，如图所示

![](/images/driver/odbc/03.png)

其中，各项参数介绍如下：

* DSN Name: DSN数据源名称
* Server Host: KAP服务器地址
* Port: KAP服务器端口号
* Username: KAP服务登录用户名
* Password: KAP服务登录密码
* Project: 查询所使用的KAP项目名称；输入完上面空白后单击Connect按钮，才可激活Project选择框

4.单击Done按钮

## 配置Excel
1.从Microsoft网站下载并安装Power Query
2.打开Excel，切换到Power Query的FastTab，单击From Other Sources并展开菜单，选择From ODBC项

![](/images/driver/odbc/04.png)

3.在弹出的“从ODBC”输入框中，首先选择前面创建的DSN作为数据源（如kylin），然后在下方输入希望执行的SQL语句

![](/images/driver/odbc/05.png)

4.这时，SQL的查询结果就显示到Excel表格中了。

![](/images/driver/odbc/06.png)

5.如果KAP中有数据更新，用户可以直接单击Excel上的Refresh按钮对表格中的数据进行刷新。

## 特别提醒
如果用户希望使用ODBC驱动连接其他客户端应用，配置方式和该例类似。如果用户希望使用Tableau，可以访问Apache Kylin官网查看相关文档。
