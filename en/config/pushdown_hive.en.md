## Hive Integration

KAP uses Hive as cube's data source. Meanwhile, Hive can also be configed as Query Pushdown's backup query engine. For cases sqls which cannot be anwsered by cube, or no response time required, KAP can push down such sqls to hive when no cubes are match.

### Query Pushdown config

1. In KAP's installation directory, uncomment configuration item `kylin.query.pushdown.runner-class-name` of config file `kylin.properties`, and set it to `org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl`

2. Add configuration items below in config file `kylin.properties`. If not set, default value will be used

   - *kylin.query.pushdown.jdbc.url*: Hive Jdbc's url, default value is `jdbc:hive2://sandbox:10000/default`



- *kylin.query.pushdown.jdbc.driver*: Hive Jdbc's driver class name, default value is `org.apache.hive.jdbc.HiveDriver`



- *kylin.query.pushdown.jdbc.username*: Hive Jdbc's user name, default value is `hive`



- *kylin.query.pushdown.jdbc.password*: Hive Jdbc's password, default value is empty string



- *kylin.query.pushdown.jdbc.pool-max-total*: Hive Jdbc's connection pool's max connected connection number, default value is 8



- *kylin.query.pushdown.jdbc.pool-max-idle*: Hive Jdbc's connection pool's max waiting connection number, default value is 8



- *kylin.query.pushdown.jdbc.pool-min-idle*: Hive Jdbc's connection pool's min connected connection number, default value is 0

3. Restart KAP
