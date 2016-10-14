## KAP Quick Start

KAP releases a few sample data sets and cubes in its package together. User could import the data set and cube easily by executing the sample script. For more detail information about installation and usage instruments, please refer to other related guide documents. 

## Prepare Environment

KAP need run in a Hadoop node, to get better stability, we suggest you to deploy it a pure Hadoop client machine, on which it command like *hive*, *hbase*, *hadoop*, *hdfs* already be installed and configured. To make things easier we strongly recommend you try KAP with *All-in-one* sandbox VM, like *Hortonworks Sandbox 2.2* and *Cloudera QuickStart VM 5.7*. The minimal memory should be 10GB. 

To avoid permission issue in the sandbox, you can use its *root* account through SSH . The password for *Hortonworks Sandbox 2.2* is *hadoop*, for *Cloudera QuickStart VM 5.7* is *cloudera*. 

We also suggest you using *bridged* mode instead of NAT mode in Virtual Box settings. Bridged mode will assign your sandbox an independent IP address so that you could access the KAP web page locally and remotely. 

### Install KAP

Copy KAP binary package into the server mentioned above, and decompress to /usr/local

```shell
cd /usr/local
tar -zxvf kap-{version}.tar.gz 
```

Set environment variable `KYLIN_HOME` to KAP home directory.

```shell
export KYLIN_HOME=/usr/local/kap-{version}
```

## Import Sample Data and Cube

`bin/sample.sh` will create three sample hive tables and import sample data. After the data uploaded into Hive, the sample project metadata will be imported also, which includes model and cube definiton. 

```shell
cd kap-{version}
bin/sample.sh
```



## **Start Hadoop**

use ambari to launch hadoop

```shell
ambari-agent start
amber-server start
```

With both command successfully run you can go to ambari homepage at [http://{hostname}:8080](http://your_sandbox_ip:8080/) (username:admin,password:admin)to check everything’s status. 

By default hortonworks ambari disables Hbase, you need manually start the Hbase service at ambari homepage.

 ![Screen Shot 2016-10-13 at 5.23.10 PM](/Users/zhangqi/Desktop/Screen Shot 2016-10-13 at 5.23.10 PM.png)

## Start KAP

Enter KAP home directory，and run the script`bin/kylin.sh start`。

```shell
cd kap-{version}
bin/kylin.sh start
```

When KAP is started successfully, the web portal is ready to access. The default address http://{hostname}:7070/kylin, default username ADMIN, and password KYLIN.

## Build Cube

Logon KAP web, select project *learn_kylin* in the project dropdown list(left upper corner). 

At the **Model** page, select the sample cube Cube *kylin_sales_cube*, click *Action* -> *Build*, pick up a end date later than *2014-01-01*(to cover all 10000 sample records), and submmit the build job.

At the **Monitor** page, click *Refresh* to check the build progress, until 100%.

## Execute SQL

When the cube is built successfully, at the **Insight** page, thress sample hive tables would be shown at the left panel. User could input query statements against these tables. For example: 

```sql
select part_dt, sum(price) as total_selled, count(distinct seller_id) as sellers from kylin_sales group by part_dt order by part_dt
```

The query result will be displayed at the **Insight** page also. User could check the query results between KAP and Hive, including accuracy and response time. 
