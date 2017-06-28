## MicroStrategy 10.X 集成

### 安装 ODBC Driver

参考页面[KAP ODBC 驱动程序教程](../driver/odbc.cn.html)安装KAP ODBC Driver v1.6。如果你安装有早前版本，请卸载后再安装。

Kylin ODBC Driver需要安装在MicroStrategy Intelligence Server所在的服务器上。

### 连接 KAP Server

用ODBC Driver连接Kylin数据源：打开MicroStrategy Developer，使用有管理员权限的账户登录到需要连接Kylin的项目源。登陆后，点击菜单->Administration->Configuration manager->Database Instance，创建新的database instance来连接到上文创建的本地Kylin ODBC连接。数据库的连接类型请选用Generic DBMS。

![](images/microstrategy_10_4/picture1.png)

### 配置Database Instance

根据你的项目环境的不同，你可能会需要将该database instance设置为已有项目的主数据源或非主数据源，或者你可以创建一个新的项目并设置该database instance为之数据源。你可以通过右键点击一个项目，选择project configuration->database instance来设置。

![](images/microstrategy_10_4/picture5.png)

![](images/microstrategy_10_4/picture6.png)

### 导入Logical Table

打开你的项目，选择菜单->Schema-> Warehouse Catalog导入你需要的Kylin数据源的表。

![](images/microstrategy_10_4/picture7.png)

![](images/microstrategy_10_4/picture8.png)

### 建立Schema Objects 和Public Objects

建立一些Attribute，Fact和Metric用以建立测试报表

![](images/microstrategy_10_4/picture2.png)

![](images/microstrategy_10_4/picture3.png)

### 创建一个简单的测试报表

现在你可以创建一张简单的报表测试你的Kylin数据源了

![](images/microstrategy_10_4/picture4.png)

### 使用MicroStrategy连接Kylin的最佳实践

以下是经笔者测试的MicroStrategy连接Kylin的最佳实践，建议按照下面操作配置你的MicroStrategy环境以优化和Kylin的连接并避开Kylin不支持的一些功能。

1. Kylin目前不支持多段SQL，也不支持创建临时表，请在MicroStrategy环境中修改VLDB 配置，设置report intermediate table类型为derived来避免MicroStrategy生成临时表。修改方法为在MicroStrategy的report中菜单选择 Data-> VLDB property-> Tables-> Intermediate Table Type设置为Derived即可。

2. 避免使用以下MicroStrategy功能，因为下面的功能可能会生成多段SQL语句，这些多段的SQL语句无法通过VLDB配置规避。

   1. 使用Datamarts
   2. 使用partitioned tables
   3. 使用custom groups
   4. 使用percentage ranking

3. 字段名和Kylin保留字段冲突会导致SQL报错，具体Kylin的保留字段请参见以下链接：

   https://calcite.apache.org/docs/reference.html#keywords

4. 如果Kylin数据模型中有事实表向维度表的左连接，MicroStrategy生成的SQL也需要产生相同的左连接来击中Cube，默认情况下MicroStrategy仅会生成内连接，可以参照以下MicroStrategy技术文档来修改MicroStrategy的VLDB配置实现左连接：

   https://community.microstrategy.com/s/article/ka1440000009GrQAAU/KB17514-Using-the-Preserve-all-final-pass-result-elements-VLDB