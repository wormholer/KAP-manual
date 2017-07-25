## Read/Write Deployment

KAP Enterprise and KAP Enterprise Plus have some difference of cube build and storage settings, therefore please note that the Read/Write Deployments on these two KAP distributions are different. Following sections would introduce you specific settings for each distribution.

**Read/Write Deployment on KAP Enterprise**

In general KAP leverages all computing resource of Hadoop cluster (Hbase runs on the same cluster) for Cube build and query. While these jobs runing at the same time, they affect each other, causing performance degrading. It's not acceptable especially when users want low-latency query. Read/Write splitting deployment provides a solution in this case.

For KAP Enterprise, after deploying KAP in Read/Write Splitting mode, Cube building job is submitted to Hadoop cluster and Cube query job is submitted to HBase cluster. They're two separated clusters, which should not affect each other any more. Hadoop cluster only handles write operations, and HBase cluster only handles read operations.

For KAP Enterprise Plus, it is also meaningful to separate cube build and cube query，and make sure it will not reduce efficiency by resource competition.

The following figure shows the architecture of Read/Write splitting deployment.

![](images/rw_separated.png)

Deployment usually includes following steps:

- Deploy Hadoop cluster (computing) and HBase cluster (storage), their Hadoop core version must be the same and equipped with necessary Hadoop components, including HDFS, Zookeeper, etc.
- Install Hadoop clients on the server that installed KAP instance (called `KAP server` later). These clients can access previous mentioned Hadoop cluster.
- Install HBase client on `KAP server`. The client can access previous mentioned HBase cluster.
- Make sure Hadoop and HBase clusters can connect to each other without additional certification, which means any node in Hadoop cluster can copy file to any node in HBase cluster.
- Make sure the `KAP server` can access HDFS in the cluster by adding HBase cluster NameNode in HDFS CLI.
- Make sure `KAP server` locates near to HBase cluster, guaranteeing low network latency.
- Edit file `${KYLIN_HOME}/conf/kylin.properties` to set `kylin.hbase.cluster.fs` HBase cluster's HDFS URL. For example: `kylin.hbase.cluster.fs＝hdfs://hbase-cluster-nn01.example.com:8020`.
- Restart KAP

**Read/Write Deployment on KAP Enterprise Plus**

Read/Write Deployment on KAP Enterprise Plus is supported but might have potential issues. Thus it strongly recommands to contact us [Kyligence's export](../introduction/get_support.cn.md) for corresponding tech support.
