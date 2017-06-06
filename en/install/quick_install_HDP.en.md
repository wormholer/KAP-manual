## KAP Quick Start on HDP sandbox

KAP releases a few sample data sets and cubes in its package together. User could import the data set and cube easily by executing the sample script. For more detail information about installation and usage instruments, please refer to other related guide documents. 

### Prepare Environment

KAP need run in a Hadoop node, to get better stability, we suggest you to deploy it a pure Hadoop client machine, on which it command like *hive*, *hbase*, *hadoop*, *hdfs* already be installed and configured. To make things easier we strongly recommend you try KAP with *All-in-one* sandbox VM, like *Hortonworks Sandbox (HDP)* and *Cloudera QuickStart VM (CDH)*. The minimal memory should be 10GB. 

> Since different Sandbox have different HBase version, please install the corresponding KAP distribution.
>
> Please use *HBase 0.98* distribution on *HDP 2.2*; Please use HBase 1.x distribution on *HDP 2.3+* 
>
> Please use CDH distribution on *CDH 5.7+*

To avoid permission issue in the sandbox, you can use its *root* account through SSH . The password for *Hortonworks Sandbox 2.2* is *hadoop*, password for *Horonworks Sandbox 2.3+*, please refer to the [Hortonworks Documents]((http://zh.hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)). 

This guide uses *root* as example. 

We also suggest you using *bridged* mode instead of NAT mode in Virtual Box settings. Bridged mode will assign your sandbox an independent IP address so that you could access the KAP web page locally and remotely. 

Make sure the following services running in normal state, (login Ambari Portal  [http://{hostname}:8080](http://{hostname}:8080) ,default username *admin*, password *admin*) HDFS/MapReduce2/YARN/Hive/HBase/ZooKeeper, without warning information. The *Service Check* in Ambari is recommended, especially HBase service.

![](images/hdp_22_status.jpg)

The following parameters should be updated, to meet the KAP resource requirement.

1. For *HDP 2.2*, update *yarn.nodemanager.resource.memory-mb* to *8192*, *yarn.scheduler.maximum-allocation-mb* to 4096; for *HDP 2.3/2.4*, update YARN-Configs->Settings->Memory Node to *8192*
2. For *HDP 2.3+*, update MapReduce2-Configs->Advanced->*MR Map Java Heap Size* and *MR Reduce Java Heap Size* to *-Xmx3072*
3. If meet *org.apache.hadoop.hbase.security.AccessDeniedException: Insufficient permissions for user 'root (auth:SIMPLE)'*, that means no enough HBase write permission. If you want to disable HBase permission check, please update *hbase.coprocessor.region.classes* and *hbase.coprocessor.master.classes* to empty, and *hbase.security.authentication* to *simple*.


### Start Hadoop

use ambari to launch hadoop

```shell
ambari-agent start
ambari-server start
```

With both command successfully run you can go to ambari homepage at [http://{hostname}:8080](http://your_sandbox_ip:8080/) (username:admin,password:admin)to check everything’s status. 

By default hortonworks ambari disables Hbase, you need manually start the Hbase service at ambari homepage.

   ![kap_quickstart_hbase](images/kap_quickstart_hbase.png)

### Install KAP

To obtain KAP package, please refer to [KAP release notes]((../release/README.md)). There may some minor differences between KAP and KAP plus. 

Copy KAP binary package into the server mentioned above, and decompress to /usr/local

```shell
cd /usr/local
tar -zxvf kap-{version}-{hbase}.tar.gz 
```

Set environment variable `KYLIN_HOME` to KAP home directory.

```shell
export KYLIN_HOME=/usr/local/kap-{version}-{hbase}
```

Create KAP working directory on HDFS, and grant privileges to KAP, with read/write permission.

```shell
hdfs dfs -mkdir /kylin
hdfs dfs -mkdir /user/root
```

> If no write permission on HDFS, please switch to hdfs account first, create the directory, and grant the privileges. 

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown root /kylin
hdfs dfs -mkdir /user/root
hdfs dfs -chown root /user/root
```

Since the sandbox has limited resource, please shift the current configuration to minimal profile.

```shell
cd $KYLIN_HOME/conf

# Use sandbox(min) profile
ln -sfn profile_min profile
```

### Environment Check

KAP will retrieve Hadoop dependency from environment by reading environment variables. The variables includes: HADOOP_CONF_DIR, HIVE_LIB, HIVE_CONF, and HCAT_HOME. The example configuration:

```shell
export HADOOP_CONF_DIR=/etc/hadoop/conf
export HIVE_LIB=/usr/hdp/current/hive-client/lib/
export HIVE_CONF=/etc/hive/conf
export HCAT_HOME=/usr/hdp/current/hive-webhcat/
```

``bin/check-env.sh`` will check if all environment meets the KAP requirements. 

### Import Sample Data and Cube

`bin/sample.sh` will create five sample hive tables and import sample data. After the data uploaded into Hive, the sample project metadata will be imported also, which includes model and cube definiton. 

```shell
cd kap-{version}-{hbase}
bin/sample.sh
```

After the successful execution, the log would be:

> Sample cube is created successfully in project 'learn_kylin'.
> Restart Kylin server or reload the metadata from web UI to see the change.

### Start KAP

Enter KAP home directory，and run the script`bin/kylin.sh start`。

```shell
cd kap-{version}-{hbase}
bin/kylin.sh start
```

When KAP is started successfully, the web portal is ready to access. The default address http://{hostname}:7070/kylin, default username ADMIN, and password KYLIN.

### Build Cube

Logon KAP web, select project *learn_kylin* in the project dropdown list(left upper corner). 

![](images/kap_learn_kylin.jpg)

At the **Model** page, select the sample cube Cube *kylin_sales_cube*, click *Action* -> *Build*, pick up a end date later than *2014-01-01*(to cover all 10000 sample records), and submmit the build job.

![](images/kap_build_cube.jpg)

At the **Monitor** page, click *Refresh* to check the build progress, until 100%.

### Execute SQL

When the cube is built successfully, at the **Insight** page, three sample hive tables would be shown at the left panel. User could input query statements against these tables. For example: 

```sql
select part_dt, sum(price) as total_selled, count(distinct seller_id) as sellers from kylin_sales group by part_dt order by part_dt
```

The query result will be displayed at the **Insight** page also. User could check the query results between KAP and Hive, including accuracy and response time. 

![](images/kap_query_result.jpg)
