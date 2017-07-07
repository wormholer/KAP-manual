## 读写分离部署
通常，KAP连接一个Hadoop集群(HBase也运行在这个集群上)，所有的负载，包括Cube构建和Cube查询都在这个集群上进行，相互影响，容易导致性能不稳定，特别是及时性要求很高的查询负载，要尽可能地独立运行。

对于KAP企业版，采用Hadoop与HBase分开部署，将Cube运算递交到Hadoop集群，而Cube查询递交到HBase集群，可以完全隔离这两种工作负载。Cube构建过程中有很多写操作，而Cube查询则主要是只读操作，这样的部署也称为读写分离的部署。

对于KAP Plus版，同样可以将构建和查询分开，保证相互之间没有因资源竞争而导致的效率下降。

读写分离的部署架构，参考下图。

![](images/rw_separated.png)

部署通常有以下步骤：

- 分布部署Hadoop（计算）集群和HBase（存储）集群；两者的Hadoop核心版本要一致，分别有各自的HDFS、Zookeeper等组件。
- 在准备运行KAP的服务器上，安装和配置Hadoop（计算）集群的客户端；通过`hadoop`, `hdfs`, `hive`, `mapred`等命令，可以访问Hadoop集群上的服务和资源。
- 在准备运行KAP的服务器上，安装和配置HBase（存储）集群的HBase客户端；通过`hbase`命令，可以访问和操作HBase集群。
- 确保Hadoop（计算）集群和HBase（存储）集群的网络互通，且无需额外验证；可以从Hadoop（计算）集群的任一节点上，拷贝文件到HBase（存储）集群的任一节点。
- 确保在准备运行KAP的服务器上，通过HDFS命令行加HBase集群NameNode地址的方式，可以访问和操作存储集群的HDFS。
- 准备运行KAP的服务器，在网络上应靠近HBase集群，以确保密集查询时的网络低延迟。
- 编辑conf/kylin.properties，设置`kylin.hbase.cluster.fs`为HBase集群HDFS的url，例如：`kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020`。
- 重启KAP


