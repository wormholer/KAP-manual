## Impala 集成

Impala提高了Apache Hadoop上的SQL查询性能，同时保留了熟悉的用户体验。 使用Impala，您可以实时查询数据，无论是存储在HDFS还是Apache HBase中，包括SELECT，JOIN和聚合函数。 此外，Impala使用和hive相同的元数据，SQL语法（Hive SQL），ODBC驱动程序和用户界面（Hue Beeswax），为面向批量或实时查询提供了一个熟悉和统一的平台。 （由于这个原因，Hive用户可以利用Impala几乎没有设置开销。）

### KAP如何连接Impala
* impala使用Hive-jdbc接口,支持jdbc接口的应用可以通过Hive-jdbc访问Impala进行数据查询

* Download ive JDBC Driver
  1. 根据自己hadoop集群hive的版本[下载](hive-jdbc.jarhttps://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)对应版本的hive-jdbc-version.jar,请确保使用的jdbc版本不要高于集群的hive版本
  2. 下载[httpclient-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient)和[httpcore-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore)

* Install JDBC
  1. 把下载好的jar包放到${KAP_HOME}/ext下面,以便让KAP在启动时可以加载JDBC Driver

* 修改kylin.properties,添加hive-jdbc配置

  1. 配置hive jdbc driver 和pushdown runner:

     1. ```kylin.query.pushdown.runner-class-name=org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl```

     2. ```kylin.query.pushdown.jdbc.driver=org.apache.hive.jdbc.HiveDriver```


  2. 配置JDBC Url

     1.访问没有kerberos安全认证的impala集群,例如(访问default库):

        ```kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;auth=noSasl```

     2. 访问带有kerberos安全认证的impala
        1. 访问带有kerberos认证的impala集群需要JDBC Client端包含impala(principal=<Impala-Kerberos-Principal>)principal在jdbc url中,例如(访问default库):

           ```kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;principal=Impala-Kerberos-Principal```


         2. 请确保kap能都读取到的hive-site.xml中打开了hive-server2的kerberos认证
            ```
                   <property>
                       <name>hive.server2.authentication</name>
                       <value>kerberos</value>
                   </property>
             ```
        3. 在初始化hive-jdbc connection前,kap需要具有有效的kerberos ticket,再启动kap之前请确保klist中存在有效的principal能够访问impala集群

  3. 验证测试
     1. 启动kap,进行表查询
     2. 在impala web页面中能够找到刚才的查询,表示kap能够正常连接impala
      ![](images/impala/1.png)





