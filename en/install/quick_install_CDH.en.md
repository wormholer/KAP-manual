## KAP Quick Start on CDH sandbox

KAP releases a few sample data sets and cubes in its package together. User could import the data set and cube easily by executing the sample script. For more detail information about installation and usage instruments, please refer to other related guide documents. 

### Prepare Environment

KAP need run in a Hadoop node, to get better stability, we suggest you to deploy it a pure Hadoop client machine, on which it command like *hive*, *hbase*, *hadoop*, *hdfs* already be installed and configured. To make things easier we strongly recommend you try KAP with *All-in-one* sandbox VM, like *Hortonworks Sandbox(HDP)* and *Cloudera QuickStart VM(CDH)*. The minimal memory should be 10GB. 

> Since different Sandbox have different HBase version, please install the corresponding KAP distribution.
>
> Please use *HBase 0.98* distribution on *HDP 2.2*; Please use HBase 1.x distribution on *HDP 2.3+* 
>
> Please use CDH distribution on *CDH 5.7+*

To avoid permission issue in the sandbox, you can use its *cloudera* account through SSH . 

This guide uses *cloudera* as example. 

We also suggest you using *bridged* mode instead of NAT mode in Virtual Box settings. Bridged mode will assign your sandbox an independent IP address so that you could access the KAP web page locally and remotely. 

Make sure the following services running in normal state, (login Cloudera Manager[http://{hostname}:7180](http://{hostname}:7180) ,default username *cloudera*, password *cloudera*) HDFS/YARN/Hive/HBase/ZooKeeper, without warning information. 

![](images/cdh_57_status.jpg)

The following parameters should be updated, to meet the KAP resource requirement.

1. For *CDH 5.7+*, update *yarn.nodemanager.resource.memory-mb* to 8192 (or 8 GB on *cloudera manager*), *yarn.scheduler.maximum-allocation-mb* to 4096 (or 4 GB on *cloudera manager*), *mapreduce.reduce.memory.mb* to 700, *mapreduce.reduce.java.opts* to 512.
2. If meet *org.apache.hadoop.hbase.security.AccessDeniedException: Insufficient permissions for user 'root (auth:SIMPLE)'*, that means no enough HBase write permission. If you want to disable HBase permission check, please update *hbase.coprocessor.region.classes* and *hbase.coprocessor.master.classes* to *empty*, and *hbase.security.authentication* to *simple*.



### Install KAP

To obtain KAP package, please refer to [KAP release notes](../release/README.md). There may have some minor differences between KAP and KAP plus. 

Copy KAP binary package into the server mentioned above, and unpack to /usr/local

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
hdfs dfs -mkdir /user/cloudera
```

> If no write permission on HDFS, please switch to hdfs account first, create the directory, and grant the privileges. 

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown cloudera /kylin
hdfs dfs -mkdir /user/cloudera
hdfs dfs -chown cloudera /user/cloudera
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
export HIVE_LIB=/usr/lib/hive
export HIVE_CONF=/etc/hive/conf
export HCAT_HOME=/usr/lib/hive-hcatalog
```

``bin/check-env.sh`` will check if all environment meets the KAP requirements.

### Start KAP

Execute command `bin/kylin.sh start`, KAP will start in background. You can track starting progress by watching file `logs/kylin.log` with `tail` command.

```shell
${KYLIN_HOME}/bin/kylin.sh start
```

To confirm KAP is running, check the process by `ps -ef | grep kylin`.

> If hit problem, please confirm all KAP processes are stopped before restart. See "Stop KAP" section for details.

### Open KAP GUI

After starting KAP, open browser and visit KAP website `http://<host_name>:7070/kylin`. KAP login page shows if everything is good.

Please replace `host_name` to machine name, ip address or domain name. The default KAP website port is 7070.

Once login to KAP successfully, you can validate the installation by building a sample cube. Please continue to [Install Validation](install_validate.en.md).

### Stop KAP
Execute command `bin/kylin.sh stop` to stop KAP service.

Confirm KAP process is stopped, `ps -ef | grep kylin` should return nothing.
