## Install KAP

Copy KAP binary to local and unpack.

```
cd /usr/local
tar -zxvf kylin-kap-{version}-bin.tar.gz
```

Set environment variable `KYLIN_HOME` to the KAP extracted directory.

```
export KYLIN_HOME=/usr/local/kylin-kap-{version}-bin
```

Prepare a working directory on HDFS for KAP user. The directory is `/kylin` by default and is configurable in `conf/kylin.properties`.

```
hdfs dfs -mkdir /kylin
# and grant permission to the user who will run KAP process
```

Make sure the user that runs KAP has permissions to access Hadoop services. If not sure, please run script `check-env.sh` to have a check. Errors will be printed to the console by this script if there is any misconfiguration.
```
${KYLIN_HOME}/bin/check-env.sh
```

> **Optional**：If want to install multiple KAP instances in a Hadoop cluster, you must specify different metadata URL for each instance. In `conf/kylin.properties`, set `kylin.metadata.url` to different values for each instance, for example `kylin_metadata@hbase` (the default value), or `kylin_prod@hbase`, or `kylin_qa@hbase` etc.

## Start KAP

Execute command `bin/kylin.sh start`, KAP will start in background. You can track starting progress by watching file `logs/kylin.log` with `tail` command.

```
${KYLIN_HOME}/bin/kylin.sh start
```

To confirm KAP is running, check the process by `ps -ef | grep kylin`.

> **For Plus Version**: The command also starts a Spark client process in the background. Check its log `logs/spark_client.out` and process `ps -ef | grep spark`.

> **Troubleshooting**: If had problem, please confirm all KAP process(es) are stopped before starting again. See "Stop KAP" section for details.

## Open KAP GUI

After starting KAP, open browser and visit KAP website `http://<host_name>:7070/kylin`. KAP login page shows if everything is good.

Please replace `host_name` to machine name, ip address or domain name. The default KAP website port is 7070.

## Stop KAP
Execute command `bin/kylin.sh stop` to stop KAP service.

Confirm KAP process is stopped, `ps -ef | grep kylin` should return nothing.

> **For Plus Version**: Also check the Spark client process is stopped, `ps -ef | grep spark`.