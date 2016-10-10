## 重要配置项
KAP配置文件中，最重要就是*kylin.properties*了。在本节，将对一些最常用的配置项作详细介绍。

### kylin.metadata.url
指定KAP元数据库的URL。默认为HBase中*kylin_metadata*表，用户可以手动修改表名以使用HBase中的其他表保存元数据。在同一个HBase集群上部署多个KAP服务时，可以为每个KAP服务配置一个元数据库URL，以实现多个KAP服务间的隔离。例如，Production实例设置该值为*kylin\_metadata\_prod*，Staging实例设置该值为*kylin\_metadata\_staging*，在Staging实例中的操作不会对Production环境产生影响。
### kylin.hdfs.working.dir
指定KAP服务所用的HDFS路径。默认为HDFS上/kylin的目录下，以元数据库URL中的HTable表名为子目录。例如，如果元数据库URL设置为kylin\_metadata@hbase，那么该HDFS路径默认值就是*/kylin/kylin\_metadata*。请预先确保启动KAP的用户有读写该目录的权限。
### kylin.server.mode
指定KAP服务的运行模式，值可以是“all”，“job”，“query”中的一个，默认是“all”。Job模式指该服务仅用于Cube任务调度，不用于SQL查询。Query模式表示该服务仅用于SQL查询，不用于Cube构建任务的调度。“all“模式指该服务同时用于任务调度和SQL查询。
### kylin.job.hive.database.for.intermediatetable
指定Hive中间表保存在哪个Hive数据库中，默认是*default*。如果执行KAP的用户没有操作*default*数据库的权限，可以修改此参数以使用其他数据库。
### kylin.hbase.default.compression.codec
KAP创建的HTable所采用的压缩算法，配置文件中默认使用了*snappy*。如果实际环境不支持*snappy*压缩，可以修改该参数以使用其他压缩算法，如*lzo*、*gzip*、*lz4*等，删除该配置项即不启动任何压缩算法。
### kylin.security.profile
指定KAP服务启用的安全方案，可以是*ldap*、*saml*、*testing*。默认值是*testing*，即使用固定的测试账号进行登录。用户可以修改此参数以接入已有的企业级认证体系，如*ldap*、*saml*。具体设置可以参考其他章节。
### kylin.rest.timezone
指定KAP的Rest服务所使用的时区，默认是*GMT+8*。用户可以根据具体应用的需要修改此参数。
### kylin.hive.client
指定Hive命令行类型，可以是*cli*或*beeline*。默认是*cli*，即hive cli。如果实际系统只支持beeline作为Hive命令行，可以修改此配置为*beeline*。
### kylin.hive.beeline.params
当使用*beeline*做为hive的client工具时，需要配置此参数，以提供更多信息给*beeline*；举例，如果您需要这样使用*beeline*来执行一个sql文件的话：
```
beeline -n root -u 'jdbc:hive2://localhost:10000' -f abc.sql
```

那么，请设置此参数：
```
kylin.hive.beeline.params=beeline -n root -u 'jdbc:hive2://localhost:10000'
```
### deploy.env
指定KAP部署的用途，可以是*DEV*、*PROD*、*QA*。默认是*QA*; 在*DEV*模式下一些开发者功能将被启用, 在*PROD*模式下禁止创建新Cube。
