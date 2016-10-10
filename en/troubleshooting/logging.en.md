KAP will create a logs directory in $KYLIN_HOME after service started up, which contains log files from KAP.

## Log files
KAP log files are consist of following files;
#### kylin.log
This is kylin's main log file, whose default logging level is DEBUG, rolling by days.
#### kylin.out
This file stores the redirect of standard output of KAP process. For example, echo from tomcat, hive will redirect to this file.
#### kylin.gc
This file stores GC events of KAP JVM. To avoid file overwritten, pid is used as filename suffix.
#### spark_client.out
(Only for KAP Plus) This file stores output of Spark Query Engine.

## Logging Analysis
Take query as an example, to introduce how to get more detail information of one query.
Submit a query on Web UI, and tail kylin.log at same time: 

```
2016-06-10 10:03:03,800 INFO  [http-bio-7070-exec-10] service.QueryService:251 :
==========================[QUERY]===============================
SQL: select * from kylin_sales
User: ADMIN
Success: true
Duration: 2.831
Project: learn_kylin
Realization Names: [kylin_sales_cube]
Cuboid Ids: [99]
Total scan count: 9840
Result row count: 9840
Accept Partial: true
Is Partial Result: false
Hit Exception Cache: false
Storage cache used: false
Message: null
==========================[QUERY]===============================
```

Here lists some primary field of this content:

* SQL: The SQL statement
* User: The username of submitter
* Success: Whether this query succeeded
* Duration: The duration of this query, in seconds
* Project: The project of this query
* Realization Names: The cube used for this query

## Logging Configuration
KAP leverages log4j for logging configuration. User can edit conf/kylin-server-log4j.properties to modify logging level, log files etc.
Reminder, it's required to restart KAP to take effect.