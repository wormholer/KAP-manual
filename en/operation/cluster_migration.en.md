## **KAP Across-Hadoop-Cluster Migration**

Please notice that this guide is only available for **KAP Enterprise Plus**. It includes whole KAP instance migration and single Cube migration.

**The whole KAP instance migration job contains two tasks:**

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



**The single Cube migration job also contains two tasks:**

- Dump Cube related metadata and  its cuing data, and put them to /tmp on HDFS .
- Distcp Cube related metadata and  its cuing data to DHC.

It leverages script `bin/cluster-migration.sh` to complete these two tasks.

**Requirements:**

The two clusters are connected and command: **hadoop distcp** is available. 

**Usages:**

*Step1*:

In SHC, run `bin/cluster-migration.sh backup-cube --cubeName someCube --onlyMetadata true`

*Step2*:

In DHC, run `bin/cluster-migration.sh restore-cube --cubeName someCube --project someProject --namenode hdfs://someip --overwrite true`

If overwrite set to ture, it will overwrite the cube no mater if there is already a cube in dest project. Otherwise it will throw exception like: already exists on target metadata store.