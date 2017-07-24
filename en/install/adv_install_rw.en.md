## Read/Write Deployment

Since there are a lot of diffences during Cube building and storage between KAP Enterprise and KAP Enterprise Plus. Therefore the Read/Write Deployments on these two KAP versions are also different. 

**Read/Write Deployment on KAP Enterprise**

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

**Read/Write Deployment on KAP Enterprise Plus**

Read/Write Deployment on KAP Enterprise Plus is also supported currenly but still risky. Thus it strongly recommands to contact [Kyligence's export](../introduction/get_support.cn.md) for help.