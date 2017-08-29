## KAP Quick Start on MapR sandbox

KAP releases a few sample data sets and cubes in its package together. You can import the data set and cube easily by executing the `sample.sh` script. For more details about installation and using instruction, please refer to other related guide documents. 

## Prepare Environment

KAP should run in a Hadoop node to get better stability, we suggest you to deploy it a pure Hadoop client machine, on which the command like *hive*, *hbase*, *hadoop*, *hdfs* already be installed and configured. To make things easier we strongly recommend you try KAP with *All-in-one* sandbox VM, like *MapR Sandbox For Hadoop 5.2.1*. The minimal memory should be 10GB. 

To avoid permission issue in the sandbox, you can use its *root* account through SSH . The password for *MapR 5.2.1* is *`mapr`*. This guide uses *`root`* account as example. 

We also suggest you using *bridged* mode instead of NAT mode in Virtual Box settings. Bridged mode will assign your sandbox an independent IP address so that you could access the KAP web page locally and remotely. 


### Install KAP

To obtain KAP package, please refer to [KAP release notes]((../release/README.md)). Note that KAP and KAP Plus are different on storage, yet you don't need to worry about installation difference of them.

Copy KAP binary package into the server mentioned above, and decompress it to `/usr/local`.

```shell
cd /usr/local
tar -zxvf kap-{version}-{hbase}.tar.gz 
```

Set environment variable `KYLIN_HOME` to KAP home directory.

```shell
export KYLIN_HOME=/usr/local/kap-{version}-{hbase}
```

Create KAP working directory on HDFS and grant read/write permission to KAP.

```shell
hadoop fs -mkdir /kylin
hadoop fs -chown root /kylin
```

Since the sandbox has limited resource, please shift the current configuration to minimal profile.

```shell
cd $KYLIN_HOME/conf

# Use sandbox(min) profile
ln -sfn profile_min profile
```

To use MapR FileSystem, KAP needs to point to MapR-FS(maprfs:///) rather than the default HDFS. Update kylin.properties

```
kylin.env.hdfs-working-dir=maprfs:///kylin
```

### Environment Check

KAP will retrieve Hadoop dependency from environment by reading environment variables. The variables include: HADOOP_CONF_DIR, HIVE_LIB, HIVE_CONF, and HCAT_HOME. The example configuration:

```shell
export HIVE_CONF=/opt/mapr/hive/hive-2.1/conf
export SPARK_HOME=/opt/mapr/spark/spark-2.1.0
```

**Note：in MapR, the file operating command is`hadoop fs`，not`hdfs`，this may block the environment check. You need to modify the shell file`$KYLIN_HOME/bin/check-os-command.sh`and comment out this line：**

```shell
#command -v hdfs                         || quit "ERROR: Command 'hdfs' is not accessible. Please check Hadoop client setup."
```

``bin/check-env.sh`` will check if all environment meet the KAP requirements.

## Import Sample Data and Cube

Run`$KYLIN_HOME/bin/sample.sh`, it will create three hive tables as a sample and import sample data to KAP. Then the sample project metadata will be imported as well, which includes model and cube definiton. 

```shell
cd kap-{version}-{hbase}
bin/sample.sh
```

If execution succeed, the log would be:

> Sample cube is created successfully in project 'learn_kylin'.
> Restart Kylin server or reload the metadata from web UI to see the change.

### Start KAP

Execute command `bin/kylin.sh start`, KAP will start in background. You can track starting progress by watching file `logs/kylin.log` with `tail` command.

```shell
${KYLIN_HOME}/bin/kylin.sh start
tail ${KYLIN_HOME}/logs/kylin.log
```

To confirm KAP is running, check the process by `ps -ef | grep kylin`.

> If hit problem, please confirm all KAP processes are stopped before restart. See "Stop KAP" section for details.

### Open KAP GUI

After initializing KAP, open browser and visit KAP website `http://<host_name>:7070/kylin`. KAP login page shows if everything is good. 

Please replace `host_name` to machine name, ip address or domain name. The default KAP website port is 7070, default username is ADMIN, and password is KYLIN.

Once login to KAP successfully, you can validate the installation by building a sample cube. Please continue to [Install Validation](install_validate.en.md).

### Stop KAP
Execute command `bin/kylin.sh stop` to stop KAP service.

Confirm KAP process is stopped, `ps -ef | grep kylin` should return nothing.