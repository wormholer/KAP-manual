## 集群（负载均衡）部署
KAP实例是无状态的服务，所有的状态信息，都存储在MetaStore（如HBase，JDBC）中；启用多个KAP节点的拒载均衡集群，让这些节点分担查询压力，互为备份，提供服务的高可用性。

![](images/cluster.png)

### KAP节点配置

将多个KAP节点组成集群，需要确保它们：

* 使用在同一Hadoop集群和HBase集群 

步骤：
* 保持各节点`kylin.metadata.url`值相同，即使用同一张HBase metadata表
* 更新其中一个KAP实例为任务引擎（`kylin.server.mode＝all`)，其它KAP实例都是查询引擎(`kylin.server.mode=query`)。如果启用任务引擎High Availability，参考下一章的[配置方法](adv_install_ha.cn.md).
* 将所有KAP实例的地址和端口更新到每个节点的``kylin.server.cluster-servers``，将被用于KAP多实例之间同步元数据状态，如`kylin.server.cluster-servers=1.1.1.1:7070,1.1.1.2:7070`

### 负载均衡配置

为了将外部请求发给集群，而不是单个节点，需要部署一个负载均衡器（Load Balancer），如Apache HTTP Server或Nginx。负载均衡器通过一定策略决定将某个请求转给某个节点，并且在节点失效时重试其它节点。终端用户通过负载均衡器的地址来访问KAP。为了便于用户和角色的管理，通常此时会启用LDAP集成的安全验证。

以Nginx为例，需要为Apache Kylin站点新建一个配置文件（如kylin.conf），内容如下：

```
upstream kylin {
    server 1.1.1.1:7070; #Kylin Server 1
    server 1.1.1.2:7070; # Kylin Server 2
}
server {
    listen       8080;
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



### 单节点多实例部署

KAP支持在单节点上运行多个实例，实例运行查询引擎以实现更好的负载均衡。

在此部署模式中，要关注以下几点：

- 保证多个实例无端口冲突。为每个实例重新配置端口，配置文件位于`${KYLIN_HOME}/tomcat/conf/server.xml`。

步骤：

- 保持各节点`kylin.metadata.url`值相同，即使用同一张HBase metadata表
- 更新其中一个KAP实例为任务引擎（`kylin.server.mode＝all`)，其它KAP实例都是查询引擎(`kylin.server.mode=query`)。如果启用任务引擎High Availability，参考[服务发现及任务引擎高可用](adv_install_ha.cn.md).
- 将所有KAP实例的地址和端口更新到每个节点的``kylin.server.cluster-servers``，将被用于KAP多实例之间同步元数据状态，如`kylin.server.cluster-servers=127.0.0.1:7070,127.0.0.1:17070`