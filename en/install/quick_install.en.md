## Install KAP

### Deployment Architecture

In general KAP deployed on a single node can serve small-scale (QPS<50) query and deployment process is easy and quick. The guide of single node KAP deployment is in [previous section](./install_guide.en.md). The architecture of this deployment is shown in the following figure.

![](images/single_node.png)

Please take care of following configurations in Single Node Deployment especially deployed in Sandbox. `yarn.nodemanager.resource.cpu-vcores` relates to CPU resources, and others are memory related configurations. Please refer to [Hadoop official site](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-common/yarn-default.xml) for detailed info.

- yarn.nodemanager.resource.cpu-vcores

- yarn.scheduler.maximum-allocation-mb

- yarn.nodemanager.resource.memory-mb

- mapreduce.map.memory.mb

- mapreduce.reduce.memory.mb

- mapreduce.map.java.opts

- mapreduce.reduce.java.opts

  ​

### Install KAP

To obtain KAP package, please refer to [KAP release notes](../release/README.md). There may have some minor differences between KAP and KAP plus. 

Copy KAP binary package into the server mentioned above, and unpack to /usr/local

```shell
cd /usr/local
tar -zxvf kap-{version}-{hbase}.tar.gz
```

Set environment variable `KYLIN_HOME` to the KAP extracted directory.

```shell
export KYLIN_HOME=/usr/local/kap-{version}-{hbase}
```

> KAP Plus will start Spark Executor, so need more YARN resource. For sandbox testing, please lower the Executor resource. Append(or update) the following parameters to Kylin.properties
>
> kap.storage.columnar.conf.spark.driver.memory=512m
>
> kap.storage.columnar.conf.spark.executor.memory=512m
>
> kap.storage.columnar.conf.spark.executor.cores=1
>
> kap.storage.columnar.conf.spark.executor.instances=1
>

Prepare a working directory on HDFS for KAP user. The directory is `/kylin` by default and is configurable in `conf/kylin.properties`.

```shell
hdfs dfs -mkdir /kylin
# and grant permission to the user who will run KAP process
hdfs dfs -mkdir /kylin/root
```
> If no write permission on HDFS, please switch to hdfs account first, create the directory, and grant the privileges. 

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown root /kylin
hdfs dfs -mkdir /user/root
hdfs dfs -chown root /user/root
```

### Environment Check

Make sure the user that runs KAP has permissions to access Hadoop services. If not sure, please run script `check-env.sh` to have a check. Errors will be printed to the console by this script if there is any misconfiguration.

```shell
${KYLIN_HOME}/bin/check-env.sh
```

> **Optional**：If want to install multiple KAP instances in a Hadoop cluster, you must specify different metadata URL for each instance. In `conf/kylin.properties`, set `kylin.metadata.url` to different values for each instance, for example `kylin_metadata@hbase` (the default value), or `kylin_prod@hbase`, or `kylin_qa@hbase` etc.

### Run Setup Script
	{KYLIN_HOME}/bin/setup.sh 
	#Enter available vcore number of your cluster. Setup script will config kylin.engine.spark-conf.spark.executor.cores and kylin.engine.spark-conf.spark.executor.instances automatically.


### Start KAP

Execute command `bin/kylin.sh start`, KAP will start in background. You can track starting progress by watching file `logs/kylin.log` with `tail` command.

```
${KYLIN_HOME}/bin/kylin.sh start
```

To confirm KAP is running, check the process by `ps -ef | grep kylin`.

> **For Plus Version**: The command also starts a Spark client process in the background. Check its log `logs/spark_client.out`, also `ps -ef | grep kylin` should report 2 processes.

> **Troubleshooting**: If had problem, please confirm all KAP process(es) are stopped before starting again. See "Stop KAP" section for details.

### Open KAP GUI

After starting KAP, open browser and visit KAP website `http://<host_name>:7070/kylin`. KAP login page shows if everything is good.

Please replace `host_name` to machine name, ip address or domain name. The default KAP website port is 7070.

### Stop KAP
Execute command `bin/kylin.sh stop` to stop KAP service.

Confirm KAP process is stopped, `ps -ef | grep kylin` should return nothing.
