## 环境检测

KAP运行在Hadoop集群上，对各个组件的版本，访问权限及CLASSPATH等都有一定的要求。因此KAP在运行时，为了避免遇到各种环境问题，提供了环境检测的功能。

```
cd $KYLIN_HOME
bin/check-env.sh
```

当然用户也可不必手动触发check-env.sh，当运行kylin.sh时，如果没做过环境检测，check-env.sh将自动运行。

## **脚本说明**

#### **check-hadoop-conf.sh**

检查kylin.properties里的配置项: kap.storage.columnar.spark-env.HADOOP_CONF_DIR是否配置正确。

#### **check-hbase-classpath.sh**

检查环境里的HBase是否正确安装，

#### **check-hbase-create-table.sh**

检查当前用户是否对HBase的表有操作权限。

#### **check-hbase-read-root.sh**

检查当前用户是否有权限访问`hbase.rootdir`

#### **check-hdfs-working-dir.sh**

检查当前用户是否对hdfs有操作权限。具体来说，KAP需要创建hdfs目录`kylin.env.hdfs-working-dir`，并进行文件读写操作。

#### **check-hive-classpath.sh**

检查环境里是否有需要的Hive相关的classpath, 特别是HCatInputFormat相关类是否存在。

#### **check-hive-user.sh**

检查当前用户是否对Hive有读写权限。包括Hive表的创建，插入数据，删除等，同时检查是否可以通过HCatalog的HCatInputFormat作为Mapreduce Job的输入。

#### **check-os-commands.sh**

检查当前环境是否支持需要的系统命令。

#### **check-ports.sh**

检查当前环境下，KAP需要的端口是否可用。如被占用，释放被占用端口或者修改KAP需要的默认端口。

#### **check-snappy.sh**

检测当前环境是否支持snappy压缩，主要是测试Snappy是否在Maprecude Job中可用。

#### **check-spark.sh**

检查Spark是否可用，这里分为两种情况：

1. 环境中有自带Spark并且版本高于1.6.0（通过运行spark_shell —version, 查看当前环境spark的版本），则可以使用环境自带的spark，即：export SPARK_HOME='SPARK_IN_ENV'

2. 环境中没有自带Spark或者Spark版本低于1.6.0，则需要使用KAP自带的spark，即：export SPARK_HOME=$KYLIN_HOME/spark。同时，环境要求Kerberos安全认证，则需要对KAP的kylin.properties进行相关配置，主要是将环境中Kerberos如下两个配置项：

   `-Djava.security.auth.login.config`

   `-Djava.security.krb5.conf`

   添加到kylin.properties里对应的：

   `kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   **Example1:** 如果环境中有Spark，且版本低于1.6，从其客户端conf目录下的spark-defaults.conf里找到配置项：

   spark.yarn.am.extraJavaOptions，将`-Djava.security.auth.login.config`开始的内容拼接到 kylin.properties对应的配置项`kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`如：	

   **kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions**=-Dhdp.version=current **-Djava.security.auth.login.config**=/opt/huawei/Bigdata/FusionInsight/spark/cfg/jaas-zk.conf-Dzookeeper.server.principal=zookeeper/hadoop.hadoop.com **-Djava.security.krb5.conf**=/opt/huawei/Bigdata/FusionInsight/spark/cfg/kdc.conf

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`	    `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   作类似修改。

​	**Example2:** 如果环境中没有Spark且需要Kerberos认证，请根据具体环境中的Kerberos配置：

​	`-Djava.security.auth.login.config`

​	`-Djava.security.krb5.conf`

​	修改kylin.properties的配置项：

​	`kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

​	`kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

​	`kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

同时，check-spark.sh读取环境yarn的资源信息，用来检查kylin.properties里关于spark executor的资源配置是否合理，并给出相关合理建议。