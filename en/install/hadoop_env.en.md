## KAP Required Environment

A stable Hadoop cluster is the basic execution environment for KAP. We recommend deploy KAP on a dedicated edge node (or client node) of Hadoop cluster. The node must have clients of Hive, HBase, Hadoop, HDFS installed and shells are available on command line.

The Linux account running KAP must have required access permissions to Hadoop cluster. These permissions include:
* Read/Write permission of HDFS
* Create/Read/Write permission of Hive table
* Create/Read/Write permission of HBase table
* Execution permission of MapReduce job

### Certificated and Tested Commercial Hadoop Distributions
* Cloudera CDH 5.7 / 5.8
* Hortonworks HDP 2.2 / 2.3 / 2.4 


### Compatible Hadoop Versions
* Hadoop: 2.4 - 2.7
* Hive: 0.13 - 1.2
* HBase: 0.98/0.99, 1.x
* JDK: 1.7+

### Configuration of YARN and MapReduce
KAP requires Hadoop resource to run distributed computation jobs. Especially, memory resource is critical for KAP jobs to run smoothly. The Yarn configuration of least requirement is listed bellow.

- Node Memory Resource (yarn.nodemanager.resource.memory-mb) >= 8192 MB
- Container Memory Maximum (yarn.scheduler.maximum-allocation-mb) >= 8192 MB

In addition, if KAP runs in a sandbox (virtual machine), please assign below resource as minimal.

- Give 10 GB memory and 2 virtual CPU to this virtual machine
- Container Virtual CPU Cores (yarn.nodemanager.resource.cpu-vcores) >= 8

### Configuration of Hive

When using *Beeline* as Hive client, KAP needs more privileges. Please update Hive configuration to add the following item:

```hive.security.authorization.sqlstd.confwhitelist=dfs.replication|hive.exec.compress.output|hive.auto.convert.join.noconditionaltask.*|mapred.output.compression.type|mapreduce.job.split.metainfo.maxsize```

### Recommended Hardware

- 2 Intel Xeon CPU (6 or 8 cores each, 2.3GHz or higher)
- 64GB or larger ECC DDR3 memory
- 1TB SAS Disk (7200 RPM, RAID1 supported) at minimal
- 2 or more 1GbE Ethernet port

### Recommended Linux Distribution
- Red Hat Enterprise Linux 6.4, 6.5
- CentOS 6.4, 6.5
- *Ubuntu has known issues*
