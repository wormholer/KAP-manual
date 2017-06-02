## 单节点安装KAP

### 部署架构

通常部署单个KAP节点，已经能够满足中小规模(QPS<50)的查询需求；单节点部署具有简单、快速的特点。部署过程即上一节的安装过程，下图是一个单节点部署的示意图。

![](images/single_node.png)

在单节点部署中，尤其是部署在沙箱中，以下的配置项值得注意。其中`yarn.nodemanager.resource.cpu-vcores`涉及CPU资源的分配，其他的配置项则是关于内存资源的分配。关于配置项的具体解释，请参考[Hadoop的官方网站](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)。

- yarn.nodemanager.resource.cpu-vcores

- yarn.scheduler.maximum-allocation-mb

- yarn.nodemanager.resource.memory-mb

- mapreduce.map.memory.mb

- mapreduce.reduce.memory.mb

- mapreduce.map.java.opts

- mapreduce.reduce.java.opts

  ​

### 安装KAP

请访问 [KAP release notes](../release/README.md)获取KAP软件包。以下步骤针对KAP，KAP Plus步骤略有不同。

拷贝KAP二进制包至安装机器，并解压至安装目录，本文以*/usr/local*为安装路径，以*root*为安装用户。

```shell
cd /usr/local
tar -zxvf kap-{version}-{hbase}.tar.gz
```

设置环境变量`KYLIN_HOME`为KAP的主目录。

```shell
export KYLIN_HOME=/usr/local/kap-{version}-{hbase}
```

> KAP Plus需要启动Spark Executor，会占用更多YARN资源，作为单机测试，需要调低Executor占用资源数，请添加（或替换）以下参数：
>
> kap.storage.columnar.conf.spark.driver.memory=512m
>
> kap.storage.columnar.conf.spark.executor.memory=512m
>
> kap.storage.columnar.conf.spark.executor.cores=1
>
> kap.storage.columnar.conf.spark.executor.instances=1

在HDFS中创建KAP的工作目录，并授予KAP启动用户读写权限。该目录默认为`/kylin`，也可以在`conf/kylin.properties`中修改目录的位置。

```shell
hdfs dfs -mkdir /kylin
# 并授权给KAP启动用户，能读写该目录
hdfs dfs -mkdir /kylin/root
```

> 如果在HDFS上没有读写权限，请先转至hdfs账户，然后创建目录，再授予权限。 

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown root /kylin
hdfs dfs -mkdir /user/root
hdfs dfs -chown root /user/cloudera
```
### 环境检查

确保运行KAP的系统用户已经获得访问Hadoop各项服务的权限。如果不确定，可以执行`check-env.sh`快速检测，如果有问题，警告或错误信息将会输出在控制台。

```shell
${KYLIN_HOME}/bin/check-env.sh
```

> **可选项**：如果要在一个Hadoop集群上安装多个KAP实例，请为每个实例配置不同的元数据URL。即在`conf/kylin.properties`中设置`kylin.metadata.url`为不同的值，例如`kylin_metadata@hbase`（缺省值），或`kylin_prod@hbase`，或`kylin_qa@hbase`等。

如果环境检查提示异常，通常因为无法有效获取环境依赖信息造成。KAP运行时需要依赖环境信息，通过环境变量读取，这些变量包括：HADOOP_CONF_DIR，HIVE_LIB，HIVE_CONF，和HCAT_HOME。样例配置如下

```shell
export HADOOP_CONF_DIR=/etc/hadoop/conf
export HIVE_LIB=/usr/lib/hive
export HIVE_CONF=/etc/hive/conf
export HCAT_HOME=/usr/lib/hive-hcatalog
```

### 运行Setup脚本

```shell
{KYLIN_HOME}/bin/setup.sh 
#输入集群可用的vcore数目。Setup脚本会自动配置kylin.engine.spark-conf.spark.executor.cores和kylin.engine.spark-conf.spark.executor.instances属性。
```

### 启动KAP

执行`bin/kylin.sh start`，KAP将在后台开始启动，您可以用`tail`等命令观察`logs/kylin.log`文件，了解启动详细进度。

```
${KYLIN_HOME}/bin/kylin.sh start
```

要确认KAP进程正在运行，可以执行`ps -ef | grep kylin`查看进程。

> **对Plus版本**: 上述命令还会启动一个额外的Spark客户进程。它的日志在 `logs/spark_client.out`，同时 `ps -ef | grep kylin`应当返回2个进程。

> **故障排除**: 如果遇到问题，请确认所有KAP都已经停止，再重试启动。请参阅"停止KAP"。

### 打开KAP GUI

在KAP启动完成后，可以打开Web浏览器，访问此KAP实例的GUI界面`http://<host_name>:7070/kylin`。

请替换*host_name*为具体的机器名、IP地址或域名。默认KAP启动在*7070*端口，默认用户名ADMIN，默认密码KYLIN。

### 停止KAP
执行`bin/kylin.sh stop`命令，停止KAP进程。

要确认KAP进程已经停止，请执行`ps -ef | grep kylin`确认没有活跃进程。
