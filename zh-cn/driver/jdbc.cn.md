## JDBC 驱动程序
KAP提供了JDBC驱动程序，支持JDBC接口的应用可以通过该驱动程序访问KAP进行数据查询。

用户可以在KAP安装目录的lib子目录下，找到名称为kylin-jdbc-kap-*.jar的jar包，这就是KAP的JDBC驱动。

#### 连接
KAP JDBC驱动程序遵循了JDBC标准接口，用户可通过URL制定JDBC所连接的KAP服务，这个URL格式为：
```
jdbc:kylin://<hostname>:<port>/<project_name>
```
* 如果KAP服务启用了SSL，则端口号应该使用KAP服务的HTTPS端口
* 如果端口未指定，则JDBC驱动将使用HTTP和HTTPS的默认端口
* project_name必须制定，用户必须保证该项目在KAP服务中存在

此外，用户需要为连接制定登陆的用户名、密码，以及SSL是否开启的标志，这些属性是：

* user: 登陆KAP服务的用户名
* password: 登陆KAP服务的密码
* ssl: true/false. 默认为false，如果是true，所有对KAP的访问都将基于HTTPS

以下给出一个建立Connection的例子：

```
Driver driver = (Driver) Class.forName("org.apache.kylin.jdbc.Driver").newInstance();
Properties info = new Properties();
info.put("user", "ADMIN");
info.put("password", "KYLIN");
Connection conn = driver.connect("jdbc:kylin://localhost:7070/kylin_project_name", info);
```

#### 基于Statement的查询
这里给出一个直接基于Statement进行查询的例子：
```
Driver driver = (Driver) Class.forName("org.apache.kylin.jdbc.Driver").newInstance();
Properties info = new Properties();
info.put("user", "ADMIN");
info.put("password", "KYLIN");
Connection conn = driver.connect("jdbc:kylin://localhost:7070/kylin_project_name", info);
Statement state = conn.createStatement();
ResultSet resultSet = state.executeQuery("select * from test_table");
while (resultSet.next()) {
    System.out.println(resultSet.getString(1));
    System.out.println(resultSet.getString(2));
    System.out.println(resultSet.getString(3));
}
```


#### 基于PreparedStatement的查询
这里给出一个基于PreparedStatement进行查询的例子：

```
Driver driver = (Driver) Class.forName("org.apache.kylin.jdbc.Driver").newInstance();
Properties info = new Properties();
info.put("user", "ADMIN");
info.put("password", "KYLIN");
Connection conn = driver.connect("jdbc:kylin://localhost:7070/kylin_project_name", info);
PreparedStatement state = conn.prepareStatement("select * from test_table where id=?");
state.setInt(1, 10);
ResultSet resultSet = state.executeQuery();
while (resultSet.next()) {
    System.out.println(resultSet.getString(1));
    System.out.println(resultSet.getString(2));
    System.out.println(resultSet.getString(3));
}
```

其中，对PreparedStatement支持对以下类型的赋值操作：

* setString
* setInt
* setShort
* setLong
* setFloat
* setDouble
* setBoolean
* setByte
* setDate
* setTime
* setTimestamp
