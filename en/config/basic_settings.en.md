## Important Configurations
The file *kylin.properties* occupies an important position among all KAP configurations. This section will give detailed explanations of some common properties in it. 

User could put the customized config items into *kylin.properties.override*, the items in this file will override the default value in *kylin.properties* at runtime. The benefit is easy to upgrade. In the system upgrade, put the *kylin.properties.override* together with new version *kylin.properties*. 

### kylin.metadata.url
KAP metadata path is specified by this property. The default value is *kylin_metadata* table in HBase while users can customize it to store metadata into any other table. When deploying multiple KAP instances on a cluster, it's necessary to specify a unique path for each of them to guarantee the isolation among them. For example, the value of this property for Production instance could be `kylin_metadata_prod`, while that for Staging instance could be `kylin_metadata_staging`, so that Production instance wouldn't be interfered by operations on Staging instance. 

### kylin.env.hdfs-working-dir
Working path of KAP instance on HDFS is specified by this property. The default value is `/kylin` on HDFS, with HTable name in metadata path as the sub-directory. For example, suppose the metadata path is ``kylin_metadata@hbase``, the HDFS default path should be `/kylin/kylin_metadata`. Please make sure the user running KAP instance has read/write permissions on that directory. 

### kylin.server.mode
KAP instance running mode is specified by this property. Optional values include `all`, `job` and `query`, among them `all` is the default one. *job* mode means the KAP instance schedules Cube task only; *query* mode means the instance serves SQL queries only; *all* mode means the instance handles both of them.

### kylin.source.hive.database-for-flat-table
This property specifies which Hive database intermediate tables will locate in. The default value is *default*. If the user running KAP doesn't have permission to access *default* database, it's adequate to alter the property to use databases with other names. 

### kylin.storage.hbase.compression-codec
The compression algorithm used in HTables created by KAP is specified by this property. The default value is *none*, which means no compression adopted. Choose appropriate compression algorithm, such as *snappy*, *lzo*, *gzip* or *lz4*, according to support for those algorithms in your situation. 

### kylin.security.profile
KAP instance security profile is specified by this property. Optional values include *ldap*, *saml* and *testing*, among them *testing* is the default one which means testing account enabled. You can alter its value to plug into existing enterprise authentication systems, such as *ldap* and *saml*. For more information, refer to section [Security and Access Control](../security/README.md). 

### kylin.web.timezone
Time zone used for KAP Rest service is specified by this property. The default value is *GMT+8*. You can alter it according to the requirement of your applications. 

### kylin.source.hive.client
Type of hive command line is specified by this property. Option values include  *cli* and *beeline*, among them *cli* is the default one, which means Hive CLI is adopted. You can alter it to *beeline* if Hive beeline is the only supported command line. 

### kylin.source.hive.beeline-params
This configuration is required when *beeline* is set in previous property. For example, if you want to execute SQL with beeline in the way below:
```
beeline -n root -u 'jdbc:hive2://localhost:10000' -f abc.sql
```

Please set the value of the property as:
```
kylin.source.hive.beeline.params=-n root -u 'jdbc:hive2://localhost:10000'
```

### kylin.env
The usage of the KAP instance is specified by this property. Optional values include *DEV*, *PROD* and *QA*, among them *PROD* is the default one. In *DEV* mode some developer functions are enabled. 

### kylin.query.force-limit
Some BI tools always send query like "select \* from fact\_table", but the process may stuck if the table size is extremely large. LIMIT clause helps in this case, and setting the value of this property to a positive integer make KAP append LIMIT clause if there's no one. For instance the value is 1000, query "select \* from fact\_table" will be transformed to "select \* from fact\_table limit 1000".

### kylin.query.disable-cube-noagg-sql
Cube stores the pre-processed data, which is different from original data in most cases. It results in inaccurate answer from Cube if the query has no aggregation function. 
This configuration is used to address the issue. If it's set to *true*, Cube is forbidden to answer queries that contain no aggregation functions, such as query "select \* from fact\_table limit 1000". Table Index or Query Pushdown will answer the query instead of Cube in this case.

## JVM Configuration Setting

In `$KYLIN_HOME/conf/setenv.sh` (for version lower than 2.4, `$KYLIN_HOME/bin/setenv.sh`), two sample settings for `KYLIN_JVM_SETTINGS` environment variable are given. The default setting use relatively less memory. You can comment it and then uncomment the next line to allocate more memory for KAP. The default configuration is: 

```
export KYLIN_JVM_SETTINGS="-Xms1024M -Xmx4096M -Xss1024K -XX:MaxPermSize=128M -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:$KYLIN_HOME/logs/kylin.gc.$$ -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=64M"
# export KYLIN_JVM_SETTINGS="-Xms16g -Xmx16g -XX:MaxPermSize=512m -XX:NewSize=3g -XX:MaxNewSize=3g -XX:SurvivorRatio=4 -XX:+CMSClassUnloadingEnabled -XX:+CMSParallelRemarkEnabled -XX:+UseConcMarkSweepGC -XX:+CMSIncrementalMode -XX:CMSInitiatingOccupancyFraction=70 -XX:+DisableExplicitGC -XX:+HeapDumpOnOutOfMemoryError"
```



## Enable Email Notification

KAP can send email notification on job complete/fail. To enable this, edit `conf/kylin.properties`, set the following parameters: 

```
mail.enabled=true
mail.host=your-smtp-server
mail.username=your-smtp-account
mail.password=your-smtp-pwd
mail.sender=your-sender-address
kylin.job.admin.dls=adminstrator-address
```

Restart KAP server to take effective. 

To disable, set `mail.enabled` back to `false`.

Administrator will get notifications for all jobs. Modeler and Analyst need entering email address into the “Notification List” at the first page of Cube wizard, and then will get notifications for that Cube.
