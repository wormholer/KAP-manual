## impala Integration

Impala raises the bar for SQL query performance on Apache Hadoop while retaining a familiar user experience. With Impala, you can query data, whether stored in HDFS or Apache HBase – including SELECT, JOIN, and aggregate functions – in real time. Furthermore, Impala uses the same metadata, SQL syntax (Hive SQL), ODBC driver, and user interface (Hue Beeswax) as Apache Hive, providing a familiar and unified platform for batch-oriented or real-time queries. (For that reason, Hive users can utilize Impala with little setup overhead.)

### Apply Impala to KAP
* Impala support Hive-JDBC driver, through which applications with JDBC interface can access Impala to query data.

* Download hive JDBC Driver
  1. According to the hive version of hadoop cluster [download](hive-jdbc.jarhttps://mvnrepository.com/artifact/org.apache.hive/hive-jdbc) hive-jdbc-version.jar.Be sure to use the jdbc version not to be higher than the hive version of the cluster.
  2. Download [httpclient-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) and [httpcore-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore)

* Install JDBC
  1. Put the downloaded jar package into ${KAP_HOME}/ext, so that KAP can be loaded at startup JDBC Driver


* Modify kylin.properties, add hive-jdbc configuration


  1. Configure hive jdbc driver and pushdown runner:

     1. ```kylin.query.pushdown.runner-class-name=org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl```

     2. ```kylin.query.pushdown.jdbc.driver=org.apache.hive.jdbc.HiveDriver```


  2. Configure JDBC Url

     1. Access impala clusters without kerberos security certification

          ```kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;auth=noSasl```

     2. Access impala with kerberos security certification
        1. To configure JDBC Clients for Kerberos Authentication with HiveServer2, they must include the principal of HiveServer2 (principal=<HiveServer2-Kerberos-Principal>) in the JDBC connection string. For example::

           ```kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;principal=Impala-Kerberos-Principal```


         2. The Hive JDBC server is configured with Kerberos authentication if the hive.server2.authentication property is set to KERBEROS in the hive-site.xml file.

            ```
                   <property>
                       <name>hive.server2.authentication</name>
                       <value>kerberos</value>
                   </property>
             ```
        3. The KAP must have a valid Kerberos ticket before you initiate a connection to HiveServer2 (use kinit).

  3. Verification test
     1. Start kap, for table query
     2. n the impala web page can be found in the query, said kap can be connected to impala normal
      ![](images/impala/1.png)





