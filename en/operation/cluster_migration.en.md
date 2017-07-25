## **KAP Across-Hadoop-Cluster Migration**

Please notice that this guide is only available for **KAP Enterprise Plus**. 

**The migration job contains two tasks:**

+ Dump metadata from Source Hadoop Cluster (SHC) and put it to HDFS.
+ Distcp KAP instance data from SHC to Destination Hadoop Cluster (DHC).

It leverages script `cluster-migration.sh` to complete these two tasks.

**Requirements:**

The two clusters are connected and command: **hadoop distcp** is available. 

**Usages:**

*Step1*:

KAP instance has been deployed to both of SHC and DHC with the same `kylin.metadata.url` and `kylin.env.hdfs-working-dir`. Make sure KAP in SHC is stopped.

*Step2*:

Copy `cluster-migration.sh`(if not exist) to both of SHC and DHC’s `KAP_DIR/bin`, and make sure they are executable.

*Step3*:

In SHC, run `bin/cluster-migration.sh backup`

*Step4*:

In DHC, run `bin/cluster-migration.sh restore hdfs://SHC-namenode/kylin_working_dir`
