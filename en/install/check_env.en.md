## Environment Check

There are a couple of requirements for KAP running on Hadoop cluster, hence it is crucial to have a environment check before running KAP.

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

Check Spark's avaivability, there are two ways and take one of them:

1. **Use Spark from the environment.** It is usally applicable for KAP version < 2.4.

   a. Please make sure Spark has been installed and it's version is higher than 1.6.0(run `spark_shell —version` to get current spark version). 

   b. export SPARK_HOME=SPARK_INSTALLATION_DIR

2. **Use Spark from KAP.** It is recommend by default.

   a. export SPARK_HOME=$KYLIN_HOME/spark

   b. If kerberos authentication is required, it needs to configure spark relevant items in *kylin.properties*. Basically there are two kerberos security items:

   `-Djava.security.auth.login.config`

   `-Djava.security.krb5.conf`

   which should be appended to those items in the *kylin.properties*:

   `kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   **Notice:** If KAP's version is 2.4 or higher, it needs to append extra Kerberos items: 

   `-Dhive.metastore.sasl.enabled=true`

   `-Dhive.metastore.kerberos.principal=hive/XXX@XXX.com`

   to `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   **Example:**

   Use vi to edit kylin.properties, find spark relevant configs and append Kerberos items:

   *kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions*= \

   -Dhdp.version=current \

   -Djava.security.auth.login.config**=/opt/spark/cfg/jaas-zk.conf **\

   -Djava.security.krb5.conf=**/opt/spark/cfg/kdc.conf

   The same modification should be done to another two spark configs:

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`	    `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

Finally, "check-spark.sh" will retrieve yarn resource manager's information, in order to inspect the spark executor's configurations in kylin.properties, also give the reasonable suggestions.
