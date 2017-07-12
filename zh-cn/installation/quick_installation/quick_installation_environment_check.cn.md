## 检查运行环境

KAP 运行在 Hadoop 集群上，对各个组件的版本、访问权限及 CLASSPATH 等都有一定的要求。因此 KAP 在运行时，为了避免遇到各种环境问题，提供了环境检测的功能。

```shell
cd $KYLIN_HOME
bin/check-env.sh
```

当然用户也可不必手动触发`check-env.sh`，当运行`kylin.sh`时，如果没做过环境检测，`check-env.sh`将自动运行。

### 脚本说明

**Checking Hadoop Configuration** `check-hadoop-conf.sh`

检查kylin.properties里的配置项: kap.storage.columnar.spark-env.HADOOP_CONF_DIR是否配置正确。

**Checking Permission of HBase's Table** `check-hbase-classpath.sh`

检查环境里的HBase是否正确安装。

**Checking Permission of HBase's Root Dir** `check-hbase-create-table.sh`

检查当前用户是否对HBase的表有操作权限。

**Checking Permission of HDFS Working Dir** `check-hbase-read-root.sh`

检查当前用户是否有权限访问`hbase.rootdir`。

**Checking Hive Classpath** `check-hdfs-working-dir.sh`

检查当前用户是否对hdfs有操作权限。具体来说，KAP需要创建hdfs目录`kylin.env.hdfs-working-dir`，并进行文件读写操作。

 **Checking Hive Classpath** `check-hive-classpath.sh`

检查环境里是否有需要的Hive相关的classpath, 特别是HCatInputFormat相关类是否存在。

**Checking Hive Usages** `check-hive-user.sh`

检查当前用户是否对Hive有读写权限。包括Hive表的创建，插入数据，删除等，同时检查是否可以通过HCatalog的HCatInputFormat作为Mapreduce Job的输入。

**Checking Java Version** `check-java.sh`

检查Java版本，并确定版本高于1.7。

**Checking JDBC Usages**  `check-jdbc.sh`

从KAP 2.4开始支持元数据通过JDBC存储在数据库上，如果元数据存储模式设置成JDBC，这个脚本将检查当前环境的JDBC是否可用。

**Checking License**  `check-license.sh`

检查KAP的安装目录是否有LICENSE文件，如果存在，检查其格式是否合法。

**Checking ACL Migration Status**  `check-migration-acl.sh`

在KAP 2.4里进行了metadata在HBase存储结构的重构，所有KAP 2.4在使用KAP 2.3及更早版本的metadata时，需要进行metadata的迁移。该脚本一方面检查metadata时候需要迁移，并给出迁移的方法。

**Checking OS Commands** `check-os-commands.sh`

检查当前环境是否支持需要的系统命令。

**Checking Ports Availability** `check-ports.sh`

检查当前环境下，KAP需要的端口是否可用。如被占用，释放被占用端口或者修改KAP需要的默认端口。

**Checking Snappy Availability** `check-snappy.sh`

检测当前环境是否支持snappy压缩，主要是测试Snappy是否在Maprecude Job中可用。

**Checking Spark Availablity**  `check-spark.sh`

检查Spark是否可用，这里分为两种情况：

+ **使用环境的Spark.** <u>适用于KAP版本低于2.4</u>，步骤如下：

  确认环境中已经安装Spark并且版本高于1.6.0（通过运行`spark_shell —version`, 查看当前环境spark的版本）。

  `export SPARK_HOME=SPARK_INSTALLATION_DIR`

+ **使用KAP的Spark.**  默认情况推荐使用这种方式，步骤如下:

  `export SPARK_HOME=$KYLIN_HOME/spark`

  如果环境要求Kerberos安全认证，则需要对KAP的`kylin.properties`进行相关配置，主要是将环境中Kerberos如下两个配置项：

  ```
  -Djava.security.auth.login.config
  -Djava.security.krb5.conf
  ```

  分别添加到`kylin.properties`里对应的：

  ```
  kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions
  ```

  **注意：**对于KAP版本2.4及以上版本，需要额外给
  `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`
  添加以下HIVE的Kerberos配置项：

  ```
  -Dhive.metastore.sasl.enabled=true
  -Dhive.metastore.kerberos.principal=hive/XXX@XXX.com
  ```

  另外一种解决方案：将`hive-site.xml`文件拷贝到`KAP_DIR/spark/conf`目录下，KAP启动后请检查`KAP_DIR/spark_clinet.out`日志，如果遇到类似HDFS目录，比如：`/tmp/hive-scratch`没有写权限的错误，通过执行`hadoop fs -chmod -R 777 /tmp/hive-scratch`。

  **Example:**

  vi编辑打开`kylin.properties`，找到如下配置项，并添加kerberos配置：

  ```
  kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions= \
  -Dhdp.version=current \
  -Djava.security.auth.login.config=/opt/spark/cfg/jaas-zk.conf \
  -Djava.security.krb5.conf=/opt/spark/cfg/kdc.conf
  ```

  同样

  ```
  kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions
  ```

  作类似修改。

  同时，`check-spark.sh`读取环境yarn的资源信息，用来检查`kylin.properties`里关于spark executor的资源配置是否合理，并给出相关合理建议。