## KAP 安装前置条件

KAP 需要一个状态良好的 Hadoop 集群作为其运行环境，以便为您提供更加稳定的使用体验。我们建议您将 KAP 单独运行在一个 Hadoop 集群上。该集群中的每一台服务器应当配置的组件包括：Hadoop、Hive、HBase、Kafka 等。其中，Hadoop 和 Hive 是必需组件。

下面我们将为您介绍 KAP 安装的前置条件。

### 账户权限

您在运行 KAP 时所使用的的 Linux 账户，应当具备访问 Hadoop 集群的权限，包括：

+ 读写 HDFS 上的文件
+ 创建和读取 Hive 表
+ 创建和操作 HBase 表（如果您使用 JDBC 连接元数据存储，该项可忽略）
+ 提交 MapReduce 任务

> 如果您使用 Beeline 连接 Hive，需要进行如下配置已使得 KAP 能够获取相应的操作权限：
>
> ```properties
> hive.security.authorization.sqlstd.confwhitelist=dfs.replication|hive.exec.compress.output|hive.auto.convert.join.noconditionaltask.*|mapred.output.compression.type|mapreduce.job.split.metainfo.maxsize
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
