## Cluster (Load Balance) Deployment
KAP instance is stateless as all state information is stored in HBase or JDBC database. So running KAP on multiple node in a cluster is a good practice for better load balance and higher availability.

![](images/cluster.png)

### KAP Instance Configuration

To organize multiple KAP nodes in a cluster, please pay attention on following points:

* Share the same Hadoop cluster and HBase cluster.


Steps

1. Keep all nodes' `kylin.metadata.url` are same. 

   > If to turn on `High Availability` on job engine. Please refer to the next chapter [Configuration](adv_install_ha.en.md).

  ​

### Load Balance Configuration

A Load Balancer, such as Apache HTTP Server and Nginx Server, is required to distribute requests in cluster. User sends requests to Load Balancer, then Load Balancer redirects requests to nodes according to some strategy. If the node handling the request fails Load Balancer will retry to send the request to other node. A good practice in this case is integrating LDAP in user and role management.

For example, in Nginx we can simply create a configuration file for Apache Kylin site(eg. kylin.conf) with following content:

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
