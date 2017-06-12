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

Prepare a working directory on HDFS for KAP user. The directory is `/kylin` by default and is configurable in `conf/kylin.properties`.

```shell
hdfs dfs -mkdir /kylin
# and grant permission to the user who will run KAP process
hdfs dfs -mkdir /kylin/root
```
If no write permission on HDFS, please switch to hdfs account first, create the directory, and grant the privileges. 

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown root /kylin
hdfs dfs -mkdir /user/root
hdfs dfs -chown root /user/root
```

> KAP Plus will start Spark Executor, so need more YARN resource. For sandbox testing, please lower the Executor resource. Append (or update) the following parameters to `conf/kylin.properties`
>
> kap.storage.columnar.conf.spark.driver.memory=512m
> kap.storage.columnar.conf.spark.executor.memory=512m
> kap.storage.columnar.conf.spark.executor.cores=1
> kap.storage.columnar.conf.spark.executor.instances=1
>
> Or run `bin/setup.sh` to setup the Spark Executor related parameters.

### Environment Check

Make sure the user that runs KAP has permissions to access Hadoop services. If not sure, please run script `check-env.sh` to have a check. Errors will be printed to the console by this script if there is any misconfiguration.

```shell
${KYLIN_HOME}/bin/check-env.sh
```

> Optionally, if want to install multiple KAP instances in a Hadoop cluster, you must specify different metadata URL for each instance. In `conf/kylin.properties`, set `kylin.metadata.url` to different values for each instance, for example `kylin_default_instance@hbase` (the default value), or `kylin_prod@hbase`, or `kylin_qa@hbase` etc.

If environment check hits error, most likely it is because certain Hadoop dependency is not detected automatically. To overcome, you can specify Hadoop dependencies explicitly by setting environment variables, including `HADOOP_CONF_DIR`, `HIVE_LIB`, `HIVE_CONF`, and `HCAT_HOME`. Examples:

```shell
export HADOOP_CONF_DIR=/etc/hadoop/conf
export HIVE_LIB=/usr/lib/hive
export HIVE_CONF=/etc/hive/conf
export HCAT_HOME=/usr/lib/hive-hcatalog
```


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
