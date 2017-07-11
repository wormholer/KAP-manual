## Environment Check

There are a couple of requirements for KAP running on Hadoop cluster, hence it is crucial to have a environment check before running KAP.

```
cd $KYLIN_HOME
bin/check-env.sh
```

Normally "check-env.sh" will be triggered by running "kylin.sh". If "check-env.sh" hasn't been finished before, it can be triggered manually as well.

### Introduction

"check-env.sh" is composed of a series of child checks as follow:

#### **Checking Hadoop Configuration (check-hadoop-conf.sh)**

Inspect `kap.storage.columnar.spark-env.HADOOP_CONF_DIR` if it is configured correctly in kylin.properties.

#### **Checking Permission of HBase's Table (check-hbase-classpath.sh)**

Check HBase whether it is installed correctly or not.

#### **Checking Permission of HBase's Root Dir (check-hbase-create-table.sh)**

Test current user if it possesses enough permission to operate HBase table(hTable), such as create/delete/update tables.

#### **Checking Permission of HDFS Working Dir (check-hbase-read-root.sh)**

Have a test whether current user possesses permission to read`hbase.rootdir` or not.

#### **Checking Hive Classpath (check-hdfs-working-dir.sh)**

Have a check if current user possesses enough permission to operate hdfs. Specifically, KAP needs to create HDFS dir according to `kylin.env.hdfs-working-dir` defined in kylin.properties as well as create/delete files on this dir.

####  **Checking Hive Classpath (check-hive-classpath.sh)**

Inspect if there are Hive classpath required by KAP, especially there are HCatalog relevant classes.

#### **Checking Hive Usages (check-hive-user.sh)**

Have a test if current user can read/create/delete/insert hive table, meanwhile running a mapreduce job to check if HCatalog is available.

#### **Checking Java Version (check-java.sh)**

Detect Java Version and make sure it is higher than 1.7

#### **Checking JDBC Usages (check-jdbc.sh)**

From KAP 2.4, it supports metadata stores in Databaes via JDBC, if it is in jdbc mode, this script checks jdbc relevant requirements.

#### **Checking License (check-license.sh)**

Detect if there is a license file in KAP_DIR and verify its format if it exists.

#### **Checking ACL Migration Status (check-migration-acl.sh)**

From KAP 2.4, it refactor the metadata in HBase. Therefore it needs migrate metadata from KAP 2.3 to KAP 2.4. This check helps figure out if the current metadata needs migration. Meanwhile it illustrates the migration method.

#### **Checking OS Commands  (check-os-commands.sh)**

Run a couple of system commands to test if they are supported by current environment.

#### **Checking Ports Availability (check-ports.sh)**

Detect the ports would be used by KAP if they are available. 

#### **Checking Snappy Availability (check-snappy.sh)**

Check if snappy is supported in current environment.

#### **Checking Spark Availablity (check-spark.sh)**

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

   ```
   Notice:
   If KAP's version is 2.4 or higher, it needs to append extra Kerberos items: 
   -Dhive.metastore.sasl.enabled=true
   -Dhive.metastore.kerberos.principal=hive/XXX@XXX.com
   to kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
   OR
   Copy hive-site.xml to KAP_DIR/spark/conf. It's better to check spark_client.out after starting KAP, if there are some exceptions like: /tmp/hive-scratch is not writeable, please change its mode to 777, i.e. hadoop fs -chmod -R 777 /tmp/hive-scratch. 
   ```

   **Example:**

   Use vi to edit kylin.properties, find spark relevant configs and append Kerberos items:

   *kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions*= \

   -Dhdp.version=current \

   -Djava.security.auth.login.config**=/opt/spark/cfg/jaas-zk.conf **\

   -Djava.security.krb5.conf=**/opt/spark/cfg/kdc.conf

   The same modification should be done to another two spark configs:

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`	    `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

Finally, "check-spark.sh" will retrieve yarn resource manager's information, in order to inspect the spark executor's configurations in kylin.properties, also give the reasonable suggestions.
