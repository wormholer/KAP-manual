## 环境检测

KAP运行在Hadoop集群上，对各个组件的版本，访问权限及CLASSPATH等都有一定的要求。因此KAP在运行时，为了避免遇到各种环境问题，提供了环境检测的功能。

```shell
cd $KYLIN_HOME
bin/check-env.sh
```

当然用户也可不必手动触发check-env.sh，当运行kylin.sh时，如果没做过环境检测，check-env.sh将自动运行。

### **脚本说明**

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

1. **使用环境的Spark.** <u>适用于KAP版本低于2.4</u>，步骤如下：

   a. 确认环境中已经安装Spark并且版本高于1.6.0（通过运行spark_shell —version, 查看当前环境spark的版本）。

   b. 导出SPARK_HOME，即：export SPARK_HOME=SPARK_INSTALLATION_DIR

2. **使用KAP的Spark.**  默认情况推荐使用这种方式，步骤如下:

   a. 导出SPARK_HOME, 即：export SPARK_HOME=$KYLIN_HOME/spark。

   b. 如果环境要求Kerberos安全认证，则需要对KAP的kylin.properties进行相关配置，主要是将环境中Kerberos如下两个配置项：

   `-Djava.security.auth.login.config`

   `-Djava.security.krb5.conf`

   分别添加到kylin.properties里对应的：

   `kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`

   `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   ```
   注意：对于KAP版本2.4及以上版本，需要额外给
   kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
   添加以下HIVE的Kerberos配置项：
   -Dhive.metastore.sasl.enabled=true
   -Dhive.metastore.kerberos.principal=hive/XXX@XXX.com
   或者：
   将hive-site.xml文件拷贝到KAP_DIR/spark/conf目录下，KAP启动后请检查KAP_DIR/spark_clinet.out日志，如果遇到类似HDFS目录，比如：/tmp/hive-scratch没有写权限的错误，通过执行hadoop fs -chmod -R 777 /tmp/hive-scratch。
   ```

   **Example:**

   vi编辑打开kylin.properties，找到如下配置项，并添加kerberos配置：

   *kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions*= \

   -Dhdp.version=current ** \

   -Djava.security.auth.login.config**=/opt/spark/cfg/jaas-zk.conf **\

   -Djava.security.krb5.conf=**/opt/spark/cfg/kdc.conf**

   同样

   `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`	    `kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions`

   作类似修改。


同时，check-spark.sh读取环境yarn的资源信息，用来检查kylin.properties里关于spark executor的资源配置是否合理，并给出相关合理建议。