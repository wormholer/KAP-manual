## Spark Engine Integration

Spark is a fast and general engine for big data processing, with built-in modules for streaming, SQL, machine learning and graph processing.

## Apply Spark to Query Pushdown
* Spark Thrift support Hive JDBC driver, through which applications with JDBC interface can access Spark to query data.

* Download Hive JDBC Driver
  1. According to the hive version of hadoop cluster download [hive-jdbc-version.jar](hive-jdbc.jarhttps://mvnrepository.com/artifact/org.apache.hive/hive-jdbc).Be sure to use the jdbc version not to be higher than the hive version of the cluster.
  2. Download [httpclient-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) and [httpcore-version.jar](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore) .

* Install Hive JDBC
  Put the downloaded jar package into `$KAP_HOME/ext`, so that KAP can be loaded at startup JDBC Driver .


* Modify `$KAP_HOME/conf/kylin.properties`, add Hive JDBC configuration.


1. Configure Hive JDBC driver and Pushdown Runner:

	 + ```kylin.query.pushdown.runner-class-name=org.apache.kylin.query.adhoc.PushDownRunnerJdbcImpl```
	
	 + ```kylin.query.pushdown.jdbc.driver=org.apache.hive.jdbc.HiveDriver```

2. Configure JDBC Url
	 + Access Spark Thrift without kerberos security certification
	 ```
	 kylin.query.pushdown.jdbc.url=jdbc:hive2://spark_host:spark_hs2_port/default``` 
	 + Access Spark Thrift with kerberos security certification 
	 
		*  To configure JDBC Clients for Kerberos Authentication with HiveServer2, they must include the principal of HiveServer2 (principal=<HiveServer2-Kerberos-Principal>) in the JDBC. connection string. For example::
		 ```
		 kylin.query.pushdown.jdbc.url=jdbc:hive2://spark_host:spark_hs2_port/default;principal=Spark-Kerberos-Principal```
		 
	  * The Hive JDBC server is configured with Kerberos authentication if the hive.server2.authentication property is set to `kerberos` in the hive-site.xml file:
		    
				         <property>
		                   <name>hive.server2.authentication</name>
		                   <value>kerberos</value>
		               </property>             
       * The **KAP must have a valid Kerberos ticket before you initiate a connection to HiveServer2** (use kinit) .	
3. Verify Thrift
 + Start `${SPARK_HOME} or ${HIVE_HOME}/bin/beeline` .
 + Enter ```!connect ${kylin.query.pushdown.jdbc.url}``` .
 + Test some SQL to ensure its works fine

4. Verify Query Pushdown
 + Start KAP to query loaded tables in the insight page
 + If queries working track can be found in the Spark web page, it means KAP has been integrated with Spark normally

![](images/query_pushdown_spark.png)



