## JDBC Driver
KAP provides JDBC driver, through which applications with JDBC interface can access KAP to query data.  

Under KAP's installation directory's subdirectory "lib", users would find a jar package named as kylin-jdbc-kap-*.jar, which is the JDBC driver of KAP.

#### Connecting Steps
KAP JDBC driver follows the JDBC standard interface, users can specify the KAP service to which the JDBC connection is made via the URL and the URL form is:

```
jdbc:kylin://<hostname>:<port>/<project_name>
```
* If KAP service start SSL, then JDBC should use the HTTPS port of the KAP service 
* If port has not been specified, then JDBC driver would use default port of HTTP and HTTPS 
* project_name is required, users have to ensure the project exist in KAP service 

Besides, users need to specify username, password and whether SSL would be true for connection, these properties are as follow: 

* user: username to login KAP service
* password: password to login KAP service
* ssl: true/false. Default is false, if it is true, all accesses to KAP are based on HTTPS

Here lists an example of Connection: 

```
Driver driver = (Driver) Class.forName("org.apache.kylin.jdbc.Driver").newInstance();
Properties info = new Properties();
info.put("user", "ADMIN");
info.put("password", "KYLIN");
Connection conn = driver.connect("jdbc:kylin://localhost:7070/kylin_project_name", info);
```

#### Query based on Statement 
Here lists an example of Query based on Statement：
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


#### Query based on Prepared Statement 
Here lists an example of Query based on Prepared Statement： 

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

Among them, Prepared Statement supports assignment for the following types: 

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
