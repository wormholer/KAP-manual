## Use jdbc to connect other database as metastore
KAP Plus uses KyStorage to store cube data, therefor hbase only works as a storage for metadata. From KAP 2.4.x, KAP supports storing metadata to other databases with jdbc

### Config metadata in the form of jdbc

Steps below are with the case of mysql:
1. Install KAP Plus
2. Create database `kylin` in mysql 
3. In KAP's installation directory, set configuration item `kylin.metadata.url` of configuration file `$KYLIN_HOME/conf/kylin.properties` to `{metadata_name}@jdbc`,
replace `{metadata_name}` as user's metadata name, for example, `{metadata_name}@jdbc`. And set configuration items for jdbc, for example, `kylin_default_instance@jdbc,url=jdbc:mysql://localhost:3306/kylin,username=root,password=,maxActive=10,maxIdle=10`. 
All items are as below, configs for `url`, `username` and `password` must be set. For others, if not set, default values will be used:

    *url*: jdbc's url;
    
    *username*: jdbc's username;
    
    *password*: jdbc's password;
    
    *driverClassName*: jdbc's driver class name, default value is `com.mysql.jdbc.Driver`;

    *maxActive*: max number of database's connection number, default value is `5`;

    *maxIdle*: max number of database's waiting connection number, default value is `5`;

    *maxWait*: max waiting milliseconds for connecting, default value is `1000`;

    *removeAbandoned*: whether remove timeout connection automatically, default value is `true`;

    *removeAbandonedTimeout*: timeout milliseconds, default value is `300`;

5. Copy the jdbc connector jar to $KYLIN_HOME/ext

6. For metadta doesn't depend on hbase, user is required to add configuration item `kylin.env.zookeeper-connect-string` of configuration file `$KYLIN_HOME/conf/kylin.properties` to zookeeper's url and port. If the server of KAP installs zookeeper as well, it can be set as `kylin.env.zookeeper-connect-string=localhost:2181`

7. Start KAP

### How to migrate metadata from hbase to jdbc
1. Set configuration item `kylin.metadata.url` of configuration file `$KYLIN_HOME/conf/kylin.properties` to the hbase metadata to be migrated
2. Run `$KYLIN_HOME/bin/metastore.sh backup` to backup metadata, and get the backup path
3. Set the metadata's settings to jdbc
4. Run `$KYLIN_HOME/bin/metastore.sh restore /path/to/backup` to restore metadata, for example `metastore.sh restore meta_backups/meta_2016_06_10_20_24_50`
