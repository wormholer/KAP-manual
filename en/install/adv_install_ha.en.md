## Service Discovery and Job Engine HA

### Service Discovery

Multiple KAP instances can work together as a cluster. When a KAP instance is started, stopped or lost, other instances in the cluster can aware and then update themselves automatically, this is called service discovery.

KAP supports service discovery from 2.0. Now KAP 2.4 has a new implementation based on Apache Curator framework, which is handier and stabler. Service Discovery based on Curator is enabled by default.


When HBase metadata store is NOT chosen (You can tell it by running `bin/get-properties.sh kylin.metadata.url` and check whether returned value ends with `@hbase`), you'll need to additionally configure zookeeper address for KAP:

- Configure Zookeeper address in `kylin.properties`
  ```
  kylin.env.zookeeper-connect-string=host1:2181,host2:2181,host3:2181
  ```

- Restart KAP

After restart, each KAP will register itself in Zookeeper, and each one will discover other instances from Zookeeper. 

By default, KAP will use `hostname -f` as the node name and the port in `tomcat/conf/server.xml` as the service port, and then broadcast this to cluster, for example: `salve1:7070`. This requires your cluster can resolve the hostname on each nodes. If not, it will cause connection error as they couldn't connect with each other. In this case, you needed manually specify the REST address when start KAP, for example:

```
export KYLIN_REST_ADDRESS=10.0.0.100:7070
$KYLIN_HOME/bin/kylin.sh start
```

### Job Engine HA

KAP job engine executes Cube building tasks and notify user when tasks finished. By default there is only one job engine in a KAP cluster. When you enable service discovery, the job engine role will be assigned dynamically among the instances which can run job engine (`kylin.server.mode=all` or `kylin.server.mode=job`). When job engine instance is stopped in one instance, other instances will take over.If you don't expect job engine be executed on some KAP instances, just configure them to run as *query* mode:

```
kylin.server.mode=query
```
