## 在HDP沙箱快速安装HDP

KAP发布了一些数据包，其中包含一些数据集样例和Cube。用户可以通过执行示例脚本轻松地导入数据集和Cube。欲悉更多关于安装和使用工具的详细信息，请参考其他相关指南。

### 环境准备

KAP需要在Hadoop节点上运行，为了获得更好的稳定性，我们建议您部署一个纯Hadoop客户机， 将一些指令，例如*hive, hbase, hadoop, hdfs*，提前安装配置好。为了使准备事宜更简便，我们建议您尝试将KAP和 *All-in-one* sandbox VM这类沙盒软件配合使用，比如*Hortonworks Sandbox（HDP）* 和*Cloudera QuickStart VM（CDH）*。虚拟机需要至少*10G*内存。

> 由于不同的沙盒软件配套于不同的HBase版本，请安装KAP相应的发行版。
>
> 在*HDP 2.2*上，请使用*HBase 0.98* 对应发行版；*HDP 2.3+*上，请使用*HBase 1.x* 对应发行版；
>
> 在*CDH 5.7+*上，请使用*CDH*对应发行版；

为了避免权限问题，我们建议使用*root*账号通过SSH的方式登录虚拟机，*HDP 2.2*的默认密码是*hadoop*， *HDP 2.3+* 请参考[Hortonworks文档](http://zh.hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)了解账号密码。

以下指南以*root*账户为例。

我们建议采用网桥（bridged）模式配置虚拟机网络，网桥模式将为虚拟机分配独立IP地址，方便本机访问KAP Web页面。

登录Ambari管理门户, [http://{hostname}:8080](http://{hostname}:8080)(默认登录用户名admin, 密码admin)确认HDFS/MapReduce2/YARN/Hive/HBase/ZooKeeper都处于正常运行状态，没有警告信息，可以针对每个服务运行*Service Check*以确认服务状态，特别是HBase服务，需要运行Service Check验证读写权限。

![](images/hdp_22_status.jpg)

以下配置需要修改，以配合KAP的资源需求

1. 针对*HDP 2.2*，找到YARN-Configs，修改*yarn.nodemanager.resource.memory-mb*为*8192*，*yarn.scheduler.maximum-allocation-mb*为*4096*；针对*HDP 2.3/2.4*，找到YARN-Configs->Settings，修改*Memory Node*为*8192*
2. 针对*HDP 2.3+*，找到MapReduce2-Configs->Advanced，修改*MR Map Java Heap Size*及*MR Reduce Java Heap Size*为 *-Xmx3072m*
3. 如果遇到*org.apache.hadoop.hbase.security.AccessDeniedException: Insufficient permissions for user 'root (auth:SIMPLE)'*这样的异常，表示没有写HBase的权限，可以将*hbase.coprocessor.region.classes*和*hbase.coprocessor.master.classes*设置为空，*hbase.security.authentication*设置为*simple*，*hbase.security.authorization*设置为*false*，以关闭HBase的权限验证。

### 启动 Hadoop

用ambari来启动hadoop

```shell
ambari-agent start
ambari-server start
```

命令成功之后，登录ambari  [http://{hostname}:8080](http://{hostname}:8080) (默认登录用户名admin, 密码admin)来检查各项状态。

默认Hbase是禁用的，需要在ambari 主页启动Hbase服务。

 ![kap_quickstart_hbase](images/kap_quickstart_hbase.png)

### 安装KAP

获取KAP安装包请参考[KAP发行说明](../release/README.md)。以下步骤针对KAP，KAP Plus步骤略有不同。

拷贝KAP二进制包至安装机器，并解压至安装目录，本文以*/usr/local*为例

```shell
cd /usr/local
tar -zxvf kap-{version}-{hbase}.tar.gz
```

设置环境变量`KYLIN_HOME`为KAP的解压缩目录。

```shell
export KYLIN_HOME=/usr/local/kap-{version}-{hbase}
```

在HDFS上创建KAP工作目录，并授权KAP启动用户读写权限。

```shell
hdfs dfs -mkdir /kylin
hdfs dfs -mkdir /user/root
```

> 如果遇到没有HDFS写权限问题，可以先切换到hdfs账号，创建目录，再授权给*root*账户。
>

```shell
su hdfs
hdfs dfs -mkdir /kylin
hdfs dfs -chown root /kylin
hdfs dfs -mkdir /user/root
hdfs dfs -chown root /user/root
```

由于沙箱环境资源有限，可以首先切换到最小化资源配置模版。

```shell
cd $KYLIN_HOME/conf

# Use sandbox(min) profile
ln -sfn profile_min profile
```

### 环境检查

KAP运行时需要依赖环境信息，通过环境变量读取，这些变量包括：HADOOP_CONF_DIR，HIVE_LIB，HIVE_CONF，和HCAT_HOME。样例配置如下

```shell
export HADOOP_CONF_DIR=/etc/hadoop/conf
export HIVE_LIB=/usr/hdp/current/hive-client/lib/
export HIVE_CONF=/etc/hive/conf
export HCAT_HOME=/usr/hdp/current/hive-webhcat/
```

可通过执行bin/check-env.sh 验证环境是否符合KAP运行需求。

### 启动KAP

执行`bin/kylin.sh start`，KAP将在后台开始启动，您可以用`tail`等命令观察`logs/kylin.log`文件，了解启动详细进度。

```shell
${KYLIN_HOME}/bin/kylin.sh start
```

要确认KAP进程正在运行，可以执行`ps -ef | grep kylin`查看进程。

> 如果遇到问题，请确认所有KAP都已经停止，再重试启动。请参阅"停止KAP"。

### 打开KAP GUI

在KAP启动完成后，可以打开Web浏览器，访问此KAP服务器的GUI界面`http://<host_name>:7070/kylin`。

请替换*host_name*为具体的机器名、IP地址或域名。默认KAP启动在*7070*端口，默认用户名ADMIN，默认密码KYLIN。

成功登录KAP后，可以通过构建Sample Cube验证安装的正确性。请继续阅读[安装验证](install/install_validate.cn.md)。

### 停止KAP

执行`bin/kylin.sh stop`命令，停止KAP进程。

要确认KAP进程已经停止，请执行`ps -ef | grep kylin`确认没有活跃进程。
