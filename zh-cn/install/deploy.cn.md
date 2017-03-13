### 单节点部署
通常部署单个KAP节点，已经能够满足中小规模(QPS<50)的查询需求；单节点部署具有简单、快速的特点。部署过程即上一节的安装过程，下图是一个单节点部署的示意图。

![](images/single_node.png)

在单节点部署中，尤其是部署在沙箱中，以下的配置项值得注意。其中`yarn.nodemanager.resource.cpu-vcores`涉及CPU资源的分配，其他的配置项则是关于内存资源的分配。关于配置项的具体解释，请参考[Hadoop的官方网站](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-common/yarn-default.xml)。

* yarn.nodemanager.resource.cpu-vcores
* yarn.scheduler.maximum-allocation-mb
* yarn.nodemanager.resource.memory-mb
* mapreduce.map.memory.mb
* mapreduce.reduce.memory.mb
* mapreduce.map.java.opts
* mapreduce.reduce.java.opts

### 单节点多实例部署

KAP支持在单节点上运行多个实例，实例运行查询引擎以实现更好的负载均衡。

在此部署模式中，要关注以下几点：

+ 实例必须运行查询模式（`kylin.server.mode＝query`），参考下一章的[配置方法](../config/jobengine_ha.cn.md)。
+ 保证多个实例无端口冲突。为每个实例重新配置端口，配置文件位于`${KYLIN_HOME}/tomcat/conf/server.xml`。

### 多节点（集群）部署
KAP实例是无状态的服务，所有的状态信息，都存储在HBase中；启用多个KAP节点的集群部署，让这些节点分担查询压力，互为备份，提供服务的高可用性。

![](images/cluster.png)

将多个KAP节点组成集群，需要确保它们：

* 使用在同一Hadoop集群和HBase集群 
* 没有端口冲突; 最好分开在不同服务器上，以做到互不影响。
* 使用同一张HBase metadata表，即`kylin.metadata.url`值相同
* 只有一个KAP实例运行任务引擎（`kylin.server.mode＝all`)，其它KAP实例都是查询引擎(`kylin.server.mode=query`)模式。或者启用任务引擎High Availability，参考下一章的[配置方法](../config/jobengine_ha.cn.md).

为了将外部请求发给集群，而不是单个节点，需要部署一个负载均衡器（Load Balancer），如Apache HTTP Server或Nginx。负载均衡器通过一定策略决定将某个请求转给某个节点，并且在节点失效时重试其它节点。终端用户通过负载均衡器的地址来访问KAP。为了便于用户和角色的管理，通常此时会启用LDAP集成的安全验证。

以Nginx为例，需要为Apache Kylin站点新建一个配置文件（如kylin.conf），内容如下：

```
upstream kylin {
    server 127.0.0.1:7070; #Kylin Server 1
    server 127.0.0.1:17070; # Kylin Server 2
}
server {
    listen       18080;
    location / {
        proxy_pass http://kylin;
    }
}
```
默认情况下，Nginx以轮询的方式转发请求，如果一个实例失效，会自动将其剔除。但是，默认情况下，Apache Kylin的用户Session信息是保存在本地的，当同一个用户的多个请求发送给不同Apache Kylin实例时，并不是所有的实例都能识别用户的登陆信息。因此，可以简单地配置Nginx使用ip_hash方式，使每个请求按照客户端ip的hash结果固定地访问一个Kylin实例。

但是ip_hash可能导致Kylin实例的负载不平衡，特别是只有少量应用服务器频繁访问Kylin时会导致大部分查询请求分发给个别Kylin实例。为解决这个问题，可以通过配置Kylin将Session信息保存到Redis集群中（或MySQL、MemCache等），实现多个Kylin实例的Session共享，简单修改Tomcat配置文件即可完成配置，具体步骤如下：
1.下载Redis相关的Jar包，并放置在$KYLIN_HOME/tomcat/lib目录下：

```
wget http://central.maven.org/maven2/redis/clients/jedis/2.0.0/jedis-2.0.0.jar
wget http://central.maven.org/maven2/org/apache/commons/commons-pool2/2.2/commons-pool2-2.2.jar
wget https://github.com/downloads/jcoleman/tomcat-redis-session-manager/tomcat-redis-session-manager-1.2-tomcat-7-java-7.jar
```
2.修改$KYLIN_HOME/tomcat/context.xml，增加如下项目：

```
<Valve className="com.radiadesign.catalina.session.RedisSessionHandlerValve" />
<Manager className="com.radiadesign.catalina.session.RedisSessionManager" host="localhost" port="6379" database="0" maxInactiveInterval="60"/>
```
其中，host和port指向所使用的Redis集群地址。
​	
### 读写分离的部署
通常，KAP连接一个Hadoop集群(HBase也运行在这个集群上)，所有的负载，包括Cube构建和Cube查询都在这个集群上进行，相互影响，容易导致性能不稳定，特别是及时性要求很高的查询负载，要尽可能地独立运行。

采用Hadoop与HBase分开部署，将Cube运算递交到Hadoop集群，而Cube查询递交到HBase集群，可以完全隔离这两种工作负载。Cube构建过程中有很多写操作，而Cube查询则主要是只读操作，这样的部署也称为读写分离的部署。

读写分离的部署架构，参考下图。

![](images/rw_separated.png)

部署通常有以下步骤：

- 分布部署Hadoop（计算）集群和HBase（存储）集群；两者的Hadoop核心版本要一致，分别有各自的HDFS、Zookeeper等组件；
- 在准备运行KAP的服务器上，安装和配置Hadoop（计算）集群的客户端；通过`hadoop`, `hdfs`, `hive`, `mapred`等命令，可以访问Hadoop集群上的服务和资源；
- 在准备运行KAP的服务器上，安装和配置HBase（存储）集群的HBase客户端；通过`hbase`命令，可以访问和操作HBase集群；
- 确保Hadoop（计算）集群和HBase（存储）集群的网络互通，且无需额外验证；可以从Hadoop（计算）集群的任一节点上，拷贝文件到HBase（存储）集群的任一节点。
- 确保在准备运行KAP的服务器上，通过hdfs命令行加HBase集群NameNode地址的方式，可以访问和操作存储集群的HDFS。
- 准备运行KAP的服务器，在网络上应靠近HBase集群，以确保密集查询时的网络低延迟；
- 编辑conf/kylin.properties，设置`kylin.hbase.cluster.fs`为HBase集群HDFS的url，例如：`kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020`
- 重启KAP

