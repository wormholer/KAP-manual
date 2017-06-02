## Environment Check

There are a couple of requirements for KAP running on Hadoop cluster, hence it is crutial to have a environment check before running KAP.

```
cd $KYLIN_HOME
bin/check-env.sh
```

Normally "check-env.sh" will be triggered by running "kylin.sh". If "check-env.sh" hasn't been finished before, it can be triggered manually as well.

### Introduction

"check-env.sh" is composed of a series of child checks as follow:

#### **check-hadoop-conf.sh**

Inspect `kap.storage.columnar.spark-env.HADOOP_CONF_DIR` if it is configured correctly in kylin.properties.

#### **check-hbase-classpath.sh**

Check HBase whether it is installed correctly or not.

#### **check-hbase-create-table.sh**

Test current user if it possesses enough permission to operate HBase table(hTable), such as create/delete/update tables.

#### **check-hbase-read-root.sh**

Have a test whether current user possesses permission to read`hbase.rootdir` or not.

#### **check-hdfs-working-dir.sh**

Have a check if current user possesses enough permission to operate hdfs. Specifically, KAP needs to create HDFS dir according to `kylin.env.hdfs-working-dir` defined in kylin.properties as well as create/delete files on this dir.

#### **check-hive-classpath.sh**

Inspect if there are Hive classpath required by KAP, especially there are HCatalog relevant classes.

#### **check-hive-user.sh**

Have a test if current user can read/create/delete/insert hive table, meanwhile running a mapreduce job to check if HCatalog is available.

#### **check-os-commands.sh**

Run a couple of system commands to test if they are supported by current environment.

#### **check-ports.sh**

Detect the ports would be used by KAP if they are available. 

#### **check-snappy.sh**

Check if snappy is supported in current environment.

#### **check-spark.sh**

Check Spark's avaivability, there are two cases:

1. If there is an existing spark installed in the environment, and its version is higher than 1.6.0 (run `spark_shell —version` to get current spark version), it is useful for KAP, i.e. 

   export SPARK_HOME='SPARK_IN_ENV'

2. If there is no Spark installed in the environment or there is a installed spark whose version is lower than 1.6.0, it is recommended to use the Spark in KAP's package, i.e.

   export SPARK_HOME=$KYLIN_HOME/spark

   Meanwhile if kerberos authentication is required, it needs to configure spark relevant items in "kylin.properties". Basically there are two kerberos security items:

   `-Djava.security.auth.login.config`

   `-Djava.security.krb5.conf`

   which should be appended to those items in the "kylin.properties":

   `kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   **Example1:** If there is a Spark installed in the environment and its version is lower than 1.6.0, find the following item from SPARK_DIR/conf/spark-defaults.conf:

   *Item:* spark.yarn.am.extraJavaOptions. Copy the content starts with

   `-Djava.security.auth.login.config` and append to

   `kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions` in "kylin.properties". It should look like:

   **kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions**=-Dhdp.version=current **-Djava.security.auth.login.config**=/opt/huawei/Bigdata/FusionInsight/spark/cfg/jaas-zk.conf-Dzookeeper.server.principal=zookeeper/hadoop.hadoop.com **-Djava.security.krb5.conf**=/opt/huawei/Bigdata/FusionInsight/spark/cfg/kdc.conf

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`	    `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   should have the same modification.

   **Example2:**  If there is no Spark installed in the environment but kerberos authentication is required,  please figure out the correct configurations of following items：

   ​`-Djava.security.auth.login.config`

   ​`-Djava.security.krb5.conf`

   ​and append them to the items in "kylin.properties"：

   ​`kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

   ​`kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   ​`kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

Finally, "check-spark.sh" will retrieve yarn resource manager's information, in order to inspect the spark executor's configurations in kylin.properties, also give the reasonable suggestions.
