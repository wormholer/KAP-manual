## Power BI 集成

Microsoft Power BI 是由微软推出的商业智能的专业分析工具，给用户提供简单且丰富的数据可视化及分析功能。


> Power BI不支持"connect live"模式，请注意并添加where条件在查询超大数据集时候，以避免从服务器拉去过多的数据到本地，甚至在某些情况下查询执行失败。

### Install ODBC Driver
参考页面[Kylin ODBC 驱动程序教程](../driver/odbc.cn.md)，请确保下载并安装Kylin ODBC Driver __v1.5__。如果你安装有早前版本，请卸载后再安装。 

### Start Power BI
1.  启动您已经安装的Power BI桌面版程序，单击`Get data`按钮，并选中ODBC数据源。
    ![](images/powerbi/Picture5.png)

2.  在弹出的`从 ODBC`数据连接向导中选择数据源名称`kylin` （请注意：DSN指向固定的数据库，如需更换连接的数据库请重建ODBC连接）；也可以点击`高级选项`，在连接字符串文本框中输入所需数据库的对应信息。此外，还可以在SQL文本框中输入您想要执行的SQL语句，单击`确定`，SQL的执行结果就会立即加载到Power BI的数据表中。
     ![](images/powerbi/Picture6.png)

3.  如果您选择不输入SQL语句，Power BI将会列出项目中所有的表，您可以根据需要将整张表的数据进行加载。但是，KAP暂不支持原数据的查询，部分表的加载可能因此受限
     ![](images/powerbi/Picture7.png)

4.  现在你可以进一步使用Power BI进行可视化分析：
     ![](images/powerbi/Picture8.png)

5.  单击工具栏的`Refresh`按钮即可重新加载数据并对图表进行更新

     ​

