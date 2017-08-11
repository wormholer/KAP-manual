## 配置 Impala 查询引擎

Impala 提高了 Apache Hadoop 上的SQL查询性能，同时保留了熟悉的用户体验。 使用 Impala，您可以实时查询数据，无论是存储在 HDFS 还是 Apache HBase 中，包括 SELECT，JOIN 和聚合函数。 此外，Impala 使用和Hive相同的元数据，SQL 语法（Hive SQL），ODBC 驱动程序和用户界面（Hue Beeswax），为面向批量或实时查询提供了一个熟悉和统一的平台。

> 由于这个原因，Hive 用户可以利用 Impala 几乎没有设置开销。

## Query Pushdown 连接 Impala
* Impala 使用 Hive JDBC接口，支持 JDBC 接口的应用可以通过 Hive JDBC 访问 Impala 进行数据查询。

* Download Hive JDBC Driver
  1. 根据自己 Hadoop 集群 Hive 的版本下载对应版本的[hive-jdbc-version.jar](hive-jdbc.jarhttps://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)，请确保使用的 JDBC 版本不要高于集群的Hive版本。
  2. 下载[httpclient-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient)和[httpcore-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore)。

* 安装 JDBC
  把下载好的 jar 包放到 `$KAP_HOME/ext` 下面，以便让 KAP 在启动时可以加载 JDBC Driver 。

* 修改 `$KAP_HOME/conf/kylin.properties` ，添加以下配置：



1. 配置 Hive JDBC driver 和 Pushdown Runner:
   + ```kylin.query.pushdown.runner-class-name=org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl```

   + ```kylin.query.pushdown.jdbc.driver=org.apache.hive.jdbc.HiveDriver```
2. 配置 JDBC URL
   + 访问没有 kerberos 安全认证的 Impala 集群，例如(访问default库):
   ```
    kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;principal=Impala-Kerberos-Principal```
   + 访问带有 kerberos 安全认证的 Impala
	 * 访问带有kerberos认证的Impala集群需要JDBC Client端包含 Impala(principal=<Impala-Kerberos-Principal>)principal 在 jdbc url 中，例如(访问default库):
	 ```
	 kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;principal=Impala-Kerberos-Principal```  
       
     * 请确保 KAP 能都读取到的 hive-site.xml 中打开了 hive-server2 的 kerberos 认证:
     					
                       <property>
                           <name>hive.server2.authentication</name>
                           <value>kerberos</value>
                       </property>                 
     * 在初始化 hive-jdbc connection 前，KAP 需要具有有效的kerberos ticket，**请确保 klist 中存在有效的 principal** 能够访问 Impala 集群。
      
3.   验证 Thrift server
     + 启动 beeline ```${SPARK_HOME} or ${HIVE_HOME}/bin/beeline```
     + 使用 beeline 连接 Spark Thrift ```!connect  ${kylin.query.pushdown.jdbc.url}```
     + 使用简单SQL测试可用
4. 验证 Query Pushdown
     + 启动 KAP ，在 Insight 界面进行一些简单查询。
     + 在 Impala web 页面中能够找到刚才的查询，表示 KAP 能够正常连接 Impala。

      ![](images/query_pushdown_impala.png)





