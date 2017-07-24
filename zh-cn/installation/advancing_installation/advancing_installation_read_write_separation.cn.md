## 读写分离部署
由于KAP企业版和KAP PLUS版在Cube的构建和存储上都有很大区别，所以各自读写分离部署也不尽相同，我们将分别进行介绍。

**KAP企业版的读写分离部署**

KAP 在使用 Hadoop 集群进行 Cube 构建等任务的同时，使用 HBase 集群处理 Cube 查询。前者中存在很多写操作，而后者中则以只读操作为主。如果您需要完全隔离上述两种工作负载，让它们各自独立运行，减少甚至避免它们之间的相互影响及其可能引发的性能不稳定，以响应即时性较高的查询，可以将 Hadoop 和 HBase 集群分开部署，即读写分离部署，其部署架构如图所示：

![](advancing_installation_images/advancing_installation_read_write_separation.png)

对于 KAP Plus，您同样可以将构建和查询所使用的集群分开，以使得它们相互之间不会因资源竞争而导致效率下降。

进行读写分离部署的步骤如下：

1. 确认您需要部署的 Hadoop 集群和 HBase 集群的 Hadoop 核心版本一致，且分别有各自的 Zookeeper 等必要组件；

2. 在您需要运行 KAP 的服务器上安装和配置 Hadoop 集群的客户端，并确认`hdfs`、`mapred`、`hive`等命令能够正常使用，且Hadoop 集群上的服务和资源能够正常访问；

3. 在您需要运行 KAP 的服务器上安装和配置 HBase 集群的 HBase 客户端，并确认`hbase`命令可以正常使用，且 HBase 集群上的服务和资源能够正常访问；

4. 确认 Hadoop 集群和 HBase 集群的网络可以在无须额外验证的情况下互相连通；

   > 提示：您可以尝试重 Hadoop 集群的任一节点上拷贝文件至 HBase 集群的任一节点，以验证两个集群之间的网络连通状况。

5. 确认在您需要运行 KAP 的服务器上，通过为`hdfs`命令指定 HBase 集群 NameNode 地址的方式，能够访问 HBase 集群的 HDFS；

6. 尽可能使得您需要运行 KAP 的服务器在网络上靠近 HBase 集群，以降低密集查询时的网络延迟；

7. 在`$KYLIN_HOME/conf/kylin.properties`文件中，将`kylin.hbase.cluster.fs`配置项的值设为 HBase 集群的 HDFS 的 URL，例如：

   ```properties
   kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020
   ```

8. 重新启动 KAP。


**KAP Plus版的读写分离部署**

KAP Plus目前也是支持读写分离部署，但存在一定的风险，不推荐用户自己部署，可以联系[Kyligence Inc](../../introduction/get_support.cn.md). 的专家进行咨询。