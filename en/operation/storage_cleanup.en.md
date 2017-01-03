## Clean Garbage

While KAP running for a period of time, there are tons of data becoming useless, yet they still occupied a lot of HDFS/HBase source, accumulation of useless data would influence performance of cluster at some degree.  The garbage data mainly including: 

- Original Cube data of purged Cube
- Original Cube Segment data of merged Cube
- Temporary files are not cleaned properly in the jobs
- Cube build logs and job history occured long time ago

KAP provides two common tools to clean garbage data. 

```
Please be noticed that data cannot be restored once removed. So it is essential to back up metadata and check the target resources carefully before using the tools. 
```

### Clean Metadata
The first one is a metadata tool, it has a delete parameter, which defaults to be false. When delete parameter turns to be true, this tool would start to delete metadata. The executing method is shown as below:

```$KYLIN_HOME/bin/metastore.sh clean [--delete true]```

During the first implementation of the tool, it is recommended to omit the delete parameter, which means only list all the resources need to be cleaned up for users to check, rather than actually start delete operation. After users confirm all listed resources are meant to be cleaned, then add the delete parameter and execute command. By default, the target resource list would be as following: 

- Invalid lookup table mirror, dictionary, Cube statistics created 2 days ago
- Action information and output of Cube build job had finished 30 days ago

### Clean Storage
The second tool is storage cleaning tool. As its name, this tool aims to clean HBase and HDFS to release more resource. Similarly, this tool has a delete parameter as well, which defaults to be false. Only when it becomes true, the tool would start to delete resource from storage devices. The executing method is as below: 

```$KYLIN_HOME/bin/kylin.sh io.kyligence.kap.tool.storage.KapStorageCleanupCLI [--delete true] [--force true]```

During the first implementation of the tool, it is recommended to omit the delete parameter, which means only list all the resources need to be cleaned up for users to check, rather than actually start delete operation. After users confirm all listed resources are meant to be cleaned, then add the delete parameter and execute command. By default, the target resource list would be as following: 

- Invalid HTable created 2 days ago
- The Hive intermediate table, HDFS temporary file which were created in the Cube building period but were not cleaned properly

If the force parameter is true, then all Hive tables are deleted.
