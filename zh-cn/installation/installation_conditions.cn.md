## KAP 安装前置条件

KAP 需要一个状态良好的 Hadoop 集群作为其运行环境，以便为您提供更加稳定的使用体验。我们建议您将 KAP 单独运行在一个 Hadoop 集群上。该集群中的每一台服务器应当配置的组件包括：Hadoop、Hive、HBase、Kafka 等。其中，Hadoop 和 Hive 是必需组件。

下面我们将为您介绍 KAP 安装的前置条件。

### 账户权限

您在运行 KAP 时所使用的的 Linux 账户，应当具备访问 Hadoop 集群的权限，包括：

+ 读写 HDFS 上的文件
+ 创建和读取 Hive 表
+ 创建和操作 HBase 表（如果您使用 JDBC 连接元数据存储，该项可忽略）
+ 提交 MapReduce 任务

> 如果您使用 Beeline 连接 Hive，需要进行如下配置已使得 KAP 能够获取相应的操作权限：(更多[beeline命令说明](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-BeelineCommandOptions))
>
> “-n root” 指用root账户连接；
>
> “-u jdbc:hive2://localhost:10000” 指将jdbc连接到以下host；
>
> ```properties
> kylin.source.hive.beeline-params=-n root --hiveconf hive.security.authorization.sqlstd.confwhitelist.append='mapreduce.job.*|dfs.*' -u jdbc:hive2://localhost:10000
> ```



### 支持的企业级平台

下述企业级数据管理平台及其相应版本已经过我们的认证和测试：

+ Cloudera CDH 5.7+
+ Hortonworks HDP 2.2+
+ 华为 FusionInsight C60+


### 支持的组件版本

KAP 兼容的 Hadoop 集群上各个组件及 JDK 的版本如下：

+ Hadoop: 2.x
+ Hive: 0.13+
+ HBase: 0.98 / 0.99 或 1.x
+ JDK: 1.7+

### 资源分配

为了使 KAP 能够高效地完成提交的任务，请您确保使用的 Hadoop 集群的配置满足如下条件：

+ `yarn.nodemanager.resource.memory-mb`配置项的值不小于 8192MB
+ `yarn.scheduler.maximum-allocation-mb`配置项的值不小于 4096MB
+ `mapreduce.reduce.memory.mb`配置项的值不小于 700MB
+ `mapreduce.reduce.java.opts`配置项的值不小于 512MB

如果您需要在沙箱等虚拟机环境中运行 KAP，请您确保该虚拟机环境能够获取如下资源：

+ 处理器个数不小于2
+ 内存不小于 10GB
+ `yarn.nodemanager.resource.cpu-vcores`配置项的值不小于8

### 推荐的硬件配置

我们推荐您使用下述硬件配置：

+ 两路 Intel 至强处理器，6核（或8核）CPU，主频 2.3GHz 或以上
+ 64GB ECC DDR3 以上
+ 至少1个 1TB 的 SAS 硬盘（3.5寸），7200RPM，RAID1
+ 至少两个 1GbE 的以太网电口

### 推荐的Linux版本

我们推荐您使用下述版本的 Linux 操作系统：

+ Red Hat Enterprise Linux 6.4+ 或 7.x
+ CentOS 6.4+ 或 7.x

### 配置KAP Plus的Spark

KAP Plus的查询需要使用Spark，如果环境中配置了Kerberos或者已存在Spark则需要添加相应的配置，这里分为两种情况：

- **使用环境的Spark.** KAP版本低于2.4（可选），步骤如下：

  确认环境中已经安装Spark并且版本高于1.6.0（通过运行`spark_shell —version`, 查看当前环境spark的版本）。

  `export SPARK_HOME=SPARK_INSTALLATION_DIR`

- **使用KAP的Spark.**  默认情况推荐使用这种方式，步骤如下:

  `export SPARK_HOME=$KYLIN_HOME/spark`

  如果环境要求Kerberos安全认证，则需要对KAP的`kylin.properties`进行相关配置，主要是将环境中Kerberos如下两个配置项：

  ```properties
  -Djava.security.auth.login.config
  -Djava.security.krb5.conf
  ```

  分别添加到`kylin.properties`里对应的：

  ```properties
  kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions
  ```

  > **注意：**对于KAP版本2.4及以上版本，需要额外给
  > `kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions`
  > 添加以下HIVE的Kerberos配置项：
  >
  > ```properties
  > -Dhive.metastore.sasl.enabled=true
  > -Dhive.metastore.kerberos.principal=hive/XXX@XXX.com
  > ```
  >
  > 另外一种解决方案：将`hive-site.xml`文件拷贝到`KAP_DIR/spark/conf`目录下，KAP启动后请检查`KAP_DIR/spark_clinet.out`日志，如果遇到类似HDFS目录，比如：`/tmp/hive-scratch`没有写权限的错误，通过执行`hadoop fs -chmod -R 777 /tmp/hive-scratch`。

  **Example:**

  vi编辑打开`kylin.properties`，找到如下配置项，并添加kerberos配置：

  ```properties
  kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions= \
  -Dhdp.version=current \
  -Djava.security.auth.login.config=/opt/spark/cfg/jaas-zk.conf \
  -Djava.security.krb5.conf=/opt/spark/cfg/kdc.conf
  ```

  同样

  ```properties
  kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions
  kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions
  ```

  作类似修改。