## Settings
`kylin.properties` is the most important one in all KAP configurations. The section includes detailed explanations of some common settings in that file.

### kylin.metadata.url
KAP metadata URL. The default value is kylin_metadata table in HBase while users can customize it to store metadata to other tables. To deploy multiple KAP instances on an HBase cluster, please specify different URL for each one to make sure they don't affect each other. For example, the URL for Production instance should be `kylin_metadata_prod`, and the URL for Staging instance should be `kylin_metadata_staging`, then operation on Staging instance won't affect Production instance.

### kylin.hdfs.working.dir
KAP instance working HDFS path. The default HDFS directory is `/kylin`, the sub-directory names are metadata HTable names. For instance, the metadata URL is `kylin_metadata@hbase`, the HDFS default path is `/kylin/kylin_metadata`. Please make sure the user running KAP instance has that directory read/write permissions.

### kylin.server.mode
KAP instance running mode. The optional values are `all`, `job` and `query` and the default value is `all`. Job mode means the KAP instance only executes Cube task scheduling. Query mode means the instance only serves SQL query. All mode means the instance handles both of them.

### kylin.job.hive.database.for.intermediatetable
Specify which Hive database intermediate table locates, the default value is `default`. If the user running KAP doesn't have `default` database access permission, please change it to other database name.

### kylin.hbase.default.compression.codec
The compression algorithm used in KAP creating HTable, the default value is `snappy`. If the deployed environment doesn't support `snappy` algorithm, modify the value to others, such as `lzo`, `gzip` and `lz4`. The configuration missing means no compression algorithm is taken.

### kylin.security.profile
KAP instance security profile. The optional values are `ldap`, `saml`, `testing`, and the default value is `testing`. `testing` means enable testing account. Users can integrate existing enterprise authentication system, such as LDAP and SAML. Please refer to other sections for detailed explanation.

### kylin.rest.timezone
Timezone of KAP Rest service, the default value is `GMT+8`.

### kylin.hive.client
Type of hive command line, and the optional values are `cli` and `beeline`. The default value is `cli` that means Hive CLI. User can modify it to `beeline` if environment only support Hive beeline.

### kylin.hive.beeline.params
The configuration is required when `beeline` is set in previous one. For example, if you want to execute SQL with beeline in this way:
```
beeline -n root -u 'jdbc:hive2://localhost:10000' -f abc.sql
```

Please set the following configuration:
```
kylin.hive.beeline.params=beeline -n root -u 'jdbc:hive2://localhost:10000'
```

### deploy.env
The usage of the KAP instance, the optional values are `DEV`, `PROD` and `QA`. Default value is `QA`. In `DEV` mode some developer functions are enabled. In `PROD` mode creating cube is forbidden.
