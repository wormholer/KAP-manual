## KAP依赖的环境

KAP需要一个状态良好的Hadoop集群做为运行环境。为获得更好的稳定性，建议将KAP单独运行在一个或多个Hadoop客户机上。该客户机上已经配置了各个组件的客户端，如`hive`, `hbase`, `hadoop`, `hdfs`。

运行KAP的Linux账户，已经具备访问Hadoop集群的权限，包括
* 读写HDFS
* 创建和读取Hive表
* 创建和操作HBase表
* 递交MapReduce任务

## 认证的和已测试的Hadoop企业版
* Cloudera CDH 5.7 / 5.8
* Hortonworks HDP 2.2 / 2.3 / 2.4 / 2.5


## 兼容的Hadoop版本
* Hadoop: 2.4 - 2.7
* Hive: 0.13 - 1.2
* HBase: 0.98/0.99, 1.1.x
* JDK: 1.7+

## YARN和MapReduce配置
KAP提交任务到Hadoop集群进行计算，需一定的内存资源。请保证YARN的配置满足如下最小条件：

- Node Memory Resource (yarn.nodemanager.resource.memory-mb) >= 8192 MB
- Container Memory Maximum (yarn.scheduler.maximum-allocation-mb) >= 8192 MB

如果使用虚拟机的Hadoop Sandbox环境运行KAP，请确保如下资源：

- 虚拟机分配至少10GB内存和2个处理器
- Container Virtual CPU Cores (yarn.nodemanager.resource.cpu-vcores) >= 8

## 推荐硬件配置
- 两路Intel至强处理器，6核（或8核）CPU, 主频2.3GHz或以上
- 64GB ECC DDR3以上
- 至少1个1TB的SAS硬盘(3.5寸), 7200RPM，RAID1
- 至少两个1GbE以太网电口

## 推荐的Linux发行版
* Red Hat Enterprise Linux 6.4, 6.5
* CentOS 6.4, 6.5
