## Single Node Deployment
In general KAP deployed on a single node can serve small-scale (QPS<50) query and deployment process is easy and quick. The guide of single node KAP deployment is in [previous section](./install_guide.en.md). The architecture of this deployment is shown in the following figure.

![](images/single_node.png)

Please take care of following configurations in Single Node Deployment especially deployed in Sandbox. `yarn.nodemanager.resource.cpu-vcores` relates to CPU resources, and others are memory related configurations. Please refer to [Hadoop official site](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-common/yarn-default.xml) for detailed info.

* yarn.nodemanager.resource.cpu-vcores
* yarn.scheduler.maximum-allocation-mb
* yarn.nodemanager.resource.memory-mb
* mapreduce.map.memory.mb
* mapreduce.reduce.memory.mb
* mapreduce.map.java.opts
* mapreduce.reduce.java.opts

## Multi-Instance on Single Node Deployment

KAP support running multiple query engine on a single node for better load balance.

Several points deserve attention:

+ Run KAP instance in query engine mode (`kylin.server.mode＝query`). Please refer to the next chapter [Configuration](../config/jobengine_ha.en.md).
+ No port conflict. Reconfigure the ports in file `${KYLIN_HOME}/tomcat/conf/server.xml` and make sure different instances don't affect each other.

## Multi-Node (Cluster) Deployment
KAP instance is stateless as all state information is stored in HBase. So running KAP on multiple node in a cluster is a good practice for better load balance and higher availability.

![](images/cluster.png)

To organize multiple KAP nodes in a cluster, please pay attention on following points:

* Share the same Hadoop cluster and HBase cluster.
* No port conflict. Better to deploy on separated server to make sure they don't affect each.
* Use the same HBase metadata table, which means the same value of `kylin.metadata.url`
* Only one KAP instance runs as job engine (`kylin.server.mode＝all`), all others run as query engines (`kylin.server.mode＝query`). Another option is to turn on `High Availability` on job engine. Please refer to the next chapter [Configuration](../config/jobengine_ha.en.md).

A Load Balancer, such as Apache HTTP Server and Nginx Server, is required to distribute requests in cluster. User sends requests to Load Balancer, then Load Balancer redirects requests to nodes according to some strategy. If the node handling the request fails Load Balancer will retry to send the request to other node. A good practice in this case is integrating LDAP in user and role management.

For example, in Nginx we can simply create a configuration file for Apache Kylin site(eg. kylin.conf) with following content:

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

By default, Nginx dispatches the requests to servers one by one. If one server crashed, Nginx will remove it automatically. In this case, requests from one client may reach different servers, where user sessions are not shared by all servers. As a result, some requests will fail due to unauthentication. Simply using ip_hash can save this, by dispatch requests according to client's IP address.

By the other side, ip_hash will bring workload imbalances to Kylin servers, especially when fixed applications visit Kylin frequently. To solve this, we can save user sessions to Redis cluster(or MySQL, MemCache), to share sessions among all kylin servers. Simply updating tomcat configuration files will make this work:

1.Download Redis Jar, and put in $KYLIN_HOME/tomcat/lib:

```
wget http://central.maven.org/maven2/redis/clients/jedis/2.0.0/jedis-2.0.0.jar
wget http://central.maven.org/maven2/org/apache/commons/commons-pool2/2.2/commons-pool2-2.2.jar
wget https://github.com/downloads/jcoleman/tomcat-redis-session-manager/tomcat-redis-session-manager-1.2-tomcat-7-java-7.jar
```

2.Edit $KYLIN_HOME/tomcat/context.xml, and add these items:

```
<Valve className="com.radiadesign.catalina.session.RedisSessionHandlerValve" />
<Manager className="com.radiadesign.catalina.session.RedisSessionManager" host="localhost" port="6379" database="0" maxInactiveInterval="60"/>
```

host and port are to specify your Redis cluster.

​	
## Read/Write Separated Deployment
In general KAP leverages all computing resource of Hadoop cluster (Hbase runs on the same cluster) for Cube building and querying. While these jobs runing at the same time, they affect each other, causing performance degrading. It's not acceptable especially when users want low-latency query. Read/Write splitting deployment provides a solution in this case.

For KAP enterprise, after deploying KAP in Read/Write Splitting mode, Cube building job is submitted to Hadoop cluster and Cube query job is submitted to HBase cluster. They're two separated clusters, which can not affect each other any more. Hadoop cluster only handles write operations, and HBase cluster only handles read operations.

For KAP Plus, it is also meaningful to separate cube build and cube query，and make sure it will not reduce  efficiency by resource competition.

The following figure shows the architecture of Read/Write splitting deployment.

![](images/rw_separated.png)

Deployment usually includes following steps:

- Deploy Hadoop cluster (computing) and HBase cluster (storage), their Hadoop core version must be the same and equipped with necessary Hadoop components, including HDFS, Zookeeper, etc.

- Install Hadoop clients on the server that installed KAP instance (called `KAP server` later). These clients can access previous mentioned Hadoop cluster.

- Install HBase client on `KAP server`. The client can access previous mentioned HBase cluster.

- Make sure Hadoop and HBase clusters can connect to each other without additional certification, which means any node in Hadoop cluster can copy file to any node in HBase cluster.

- Make sure the `KAP server` can access HDFS in the cluster by adding HBase cluster NameNode in HDFS CLI.

- Make sure `KAP server` locates near to HBase cluster, guaranteeing low network latency.

- Edit file `${KYLIN_HOME}/conf/kylin.properties`, set `kylin.hbase.cluster.fs` HBase cluster's HDFS URL. For example: `kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020`.

- Restart KAP
