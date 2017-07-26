## 使用Hive作为Query Pushdown查询引擎

KAP使用Hive作为cube的数据源。同时，也可以将其配置为Query Pushdown的备用查询引擎，以根据需求，在未建cube时，对查询频率不高或者对响应时耗无太高要求的查询语句也能给出相应结果。

### 配置方法
1. 修改配置文件`kylin.properties`打开Query Pushdown注释掉的配置项`kylin.query.pushdown.runner-class-name`，设置为`org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl`

2. 在配置文件`kylin.properties`添加如下配置项，若不设置，将使用默认配置项

   *kylin.query.pushdown.jdbc.url*：Hive Jdbc的url，默认值为`jdbc:hive2://sandbox:10000/default`
   
   *kylin.query.pushdown.jdbc.driver*：Hive Jdbc的driver类名，默认值为`org.apache.hive.jdbc.HiveDriver`
   
   *kylin.query.pushdown.jdbc.username*：Hive Jdbc对应数据库的用户名，默认值为`hive`
   
   *kylin.query.pushdown.jdbc.password*：Hive Jdbc对应数据库的密码，默认为空字符串
   
   *kylin.query.pushdown.jdbc.pool-max-total*：Hive Jdbc连接池的最大连接数，默认值为8
   
   *kylin.query.pushdown.jdbc.pool-max-idle*：Hive Jdbc连接池的最大等待连接数，默认值为8
   
   *kylin.query.pushdown.jdbc.pool-min-idle*：Hive Jdbc连接池的最小连接数，默认值为0

3. 重启KAP