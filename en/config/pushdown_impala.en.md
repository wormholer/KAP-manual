## Impala Engine Integration

Impala raises the bar for SQL query performance on Apache Hadoop while retaining a familiar user experience. With Impala, you can query data, whether stored in HDFS or Apache HBase – including SELECT, JOIN, and aggregate functions – in real time. Furthermore, Impala uses the same metadata, SQL syntax (Hive SQL), ODBC driver, and user interface (Hue Beeswax) as Apache Hive, providing a familiar and unified platform for batch-oriented or real-time queries. (For that reason, Hive users can utilize Impala with little setup overhead.)

## Apply Impala to Query Pushdown
* Impala supports Hive JDBC driver, through which applications with JDBC interface can access Impala to query data.

* Download Hive JDBC Driver
  1. According to the Hive version of Hadoop cluster download [hive-jdbc-version.jar](hive-jdbc.jarhttps://mvnrepository.com/artifact/org.apache.hive/hive-jdbc). Be sure the JDBC version is not higher than the Hive version of the cluster.
  2. Download [httpclient-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) and [httpcore-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore)

* Install Hive JDBC
  Put the downloaded jar package into `$KAP_HOME/ext`, so that KAP can be loaded at startup JDBC Driver


* Modify `$KAP_HOME/conf/kylin.properties`, add Hive JDBC configuration


1. Configure Hive JDBC driver and Pushdown Runner:

    + ```kylin.query.pushdown.runner-class-name=org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl```

    + ```kylin.query.pushdown.jdbc.driver=org.apache.hive.jdbc.HiveDriver```
    
2. Configure JDBC Url
	 + Access Spark Thrift without kerberos security certification:
	 ```
	 kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;auth=noSasl```
	 +  Access Impala with kerberos security certification
  
		* To configure JDBC Clients for Kerberos Authentication with HiveServer2, they must include the principle of HiveServer2 (principal=<HiveServer2-Kerberos-Principal>) in the JDBC connection string. For example::
       ```
       kylin.query.pushdown.jdbc.url=jdbc:hive2://impala_host:impala_hs2_port/default;principal=Impala-Kerberos-Principal```
		* The Hive JDBC server is configured with Kerberos authentication if the hive.server2.authentication property is set to `kerberos` in the hive-site.xml file:

                   <property>
                       <name>hive.server2.authentication</name>
                       <value>kerberos</value>
                   </property>
		* The **KAP must have a valid Kerberos ticket before you initiate a connection to HiveServer2** (use kinit).
3. Verify thrift
 + Start `${SPARK_HOME} or ${HIVE_HOME}/bin/beeline`
 + Enter ``!connect ${kylin.query.pushdown.jdbc.url}``
 + Test some SQL queries and ensure they work correctly
  
4. Verify Query Pushdown
 + Start KAP to query loaded tables in the insight page
 + If queries working track can be found in the Impala web page, it means KAP has been integrated with Impala normally
     ![](images/query_pushdown_impala.png)





