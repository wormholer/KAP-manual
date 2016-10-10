## Single Node Deployment
In general KAP deployed on single node can serve small-scale (QPS<50) query and deployment process is easy and quick. The guide of single node KAP deployment is in [previous section](./install_guide.en.md). The architecture of this deployment is shown in following figure.

![]( /images/install/single_node.png)

Please take care of following configurations in Single Node Deployment especially deployed in Sandbox. `yarn.nodemanager.resource.cpu-vcores` relates to CPU resources, and others are memory related configurations. Please refer to [Hadoop official site](https://hadoop.apache.org/docs/r2.7.3/hadoop-yarn/hadoop-yarn-common/yarn-default.xml) for detailed info.

 * yarn.nodemanager.resource.cpu-vcores
 * yarn.scheduler.maximum-allocation-mb
 * yarn.nodemanager.resource.memory-mb
 * mapreduce.map.memory.mb
 * mapreduce.reduce.memory.mb
 * mapreduce.map.java.opts
 * mapreduce.reduce.java.opts

## Multi-Node (Cluster) Deployment
KAP instance is stateless as all state information is stored in HBase. So running KAP on multiple node in a cluster is a good practice for better load balance and higher availability.

To organize multiple KAP nodes in a cluster, please pay attention on following points:

 * Share the same Hadoop cluster and HBase cluster.
 * No port conflict. Better to deploy on separated server to make sure they don't affect each.
 * Use the same HBase metadata table, which means the same value of `kylin.metadata.url`
 * Only one KAP instance runs as job engine (`kylin.server.mode＝all`), all others run as query engine (`kylin.server.mode＝query`). Another option is turn on `High Availability` on job engine. Please refer to next chapter [Configuration](../config/jobengine_ha.en.md).

A Load Balancer, such as Apache HTTP Server and Nginx Server, is required to distribute requests in cluster. User sends requests to Load Balancer, then Load Balancer redirects requests to nodes according to some strategy. If the node handling the request fails Load Balancer will retry to send the request to other node. A good practice in this case is integrating LDAP in user and role management.

![]( /images/install/cluster.png)
	
## Read/Write Separated Deployment
In general KAP leverage all computing resource of Hadoop cluster (Hbase runs on the same cluster) for Cube building and querying. When these jobs run at the same time, they affect each other, causing performance degrading. It's not acceptable especially when user want low-latency query. Read/Write splitting deployment provides a solution in this case.

After deploying KAP in Read/Write Splitting mode, Cube building job is submitted to Hadoop cluster and Cube query job is submitted to HBase cluster. They're two separated clusters, can not affect each other any more. Hadoop cluster only handles write operations, and HBase cluster only handles read operations.

The following figure shows the architecture of Read/Write splitting deployment.

![]( /images/install/rw_separated.png)

Deployment usually includes following steps:

- Deploy Hadoop cluster (computing) and HBase cluster (storage), their Hadoop core version must be the same and equipped with necessary Hadoop components, including HDFS, Zookeeper, etc.

- Install Hadoop clients on the server that installed KAP instance (called `KAP server` later). These clients can access previous mentioned Hadoop cluster.

- Install HBase client on `KAP server`. The client can access previous mentioned HBase cluster.

- Make sure Hadoop and HBase clusters can connect to each other without additional certification, which means any node in Hadoop cluster can copy file to any node in HBase cluster.

- Make sure the `KAP server` can access HDFS in the cluster by adding HBase cluster NameNode in HDFS CLI.

- Make sure `KAP server` locates near to HBase cluster, guaranteeing low network latency.

- Edit file `${KYLIN_HOME}/conf/kylin.properties`, set `kylin.hbase.cluster.fs` HBase cluster's HDFS URL. For example: `kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020`.

- Restart KAP