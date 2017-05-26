## 重要参数配置
kylin.properties在KAP的配置文件中占据重要位置。本节内容将对一些常用的配置项进行详细介绍。

### kylin.metadata.url
该参数指定KAP元数据库路径。默认值为HBase中的*kylin_metadata*表，用户可以手动修改参数值以自定义的名称命名元数据库。在同一个集群上部署多个KAP实例时，可以为每个KAP实例配置一个独有的元数据库路径，以实现多个KAP实例间的隔离。例如，Production实例设置该值为*kylin\_metadata\_prod*，Staging实例设置该值为*kylin\_metadata\_staging*，那么在Staging实例中的操作不会对Production实例产生任何影响。
### kylin.env.hdfs-working-dir
该参数指定KAP服务所用的HDFS路径。默认值为HDFS上的`kylin/`，以元数据库路径中的HTable表名为子目录。例如，假设元数据库路径参数值为kylin\_metadata@hbase，那么该HDFS路径默认值就是`/kylin/kylin_metadata`。请预先确保启动KAP实例的用户有读写该目录的权限。
### kylin.server.mode
该参数指定KAP实例的运行模式，参数值可以是*all*，*job*，*query*中的一个，默认值为*all*。*job*模式表示该服务仅用于Cube任务调度，不用于SQL查询；*query*模式表示该服务仅用于SQL查询，不用于Cube构建任务的调度；*all*模式表示该服务同时用于任务调度和SQL查询。
### kylin.source.hive.database-for-flat-table
该参数指定Hive中间表保存在哪个Hive数据库中，默认值为*default*。如果执行KAP的用户没有操作*default*数据库的权限，可以修改此参数的值以使用其他数据库。
### kylin.storage.hbase.compression-codec
该参数指定KAP创建的HTable所采用的压缩算法。默认值为*none*，即不启用压缩。请根据实际对压缩算法的支持情况，选择合适的压缩算法，如*snappy*、*lzo*、*gzip*、*lz4*等。
### kylin.security.profile
该参数指定KAP服务启用的安全授权方案，可以是*ldap*、*saml*、*testing*。默认值为*testing*，即使用固定的测试账号进行登录。用户可以修改此参数以接入已有的企业级认证体系，如*ldap*、*saml*。具体设置可以参考[安全控制](../security/README.md)章节。
### kylin.web.timezone
该参数指定KAP的REST服务所使用的时区。默认值为*GMT+8*。用户可以根据具体应用的需要修改此参数。
### kylin.source.hive.client
该参数指定Hive命令行类型，可以是*cli*或*beeline*。默认值为*cli*，即hive cli。如果系统只支持beeline作为Hive命令行，可以修改此配置为*beeline*。
### kylin.source.hive.beeline-params
当使用*beeline*做为hive的client工具时，需要配置此参数，以提供更多信息给*beeline*。举例，如果您需要这样使用*beeline*来执行一个sql文件的话：
```
beeline -n root -u 'jdbc:hive2://localhost:10000' -f abc.sql
```

那么，请设置此参数为：
```
kylin.hive.beeline.params=-n root -u 'jdbc:hive2://localhost:10000' -f abc.sql
```
### kylin.env
该参数指定KAP部署的用途，可以是*DEV*、*PROD*、*QA*。出厂默认值为*PROD*。在*DEV*模式下一些开发者功能将被启用。
