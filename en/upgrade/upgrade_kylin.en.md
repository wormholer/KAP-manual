## Upgrade from Apache Kylin

KAP is built based on Apache Kylin, user could upgrade the system from Apache Kylin to KAP very easily.

#### Upgrade from Apache Kylin 1.5.1+ to KAP 2.X
KAP 2.x metadata is compitible with 1.5.1+, no cube rebuild needed. Since the HBase Coprocessor protocol may change for each version, and the existing HTable has been bounded with old version protocols. So new coprocessor libaries need to redeploy again. Please follow the commands 

```shell
$KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
```

#### Upgrade from prior Kylin 1.5.0 to KAP 2.X
If user wants to upgrade from Apache Kylin 1.5.0 to KAP 2.X, has to upgrade to Apache Kylin 1.5.1 first, and following the above steps to upgrade continuely. 

Apache Kylin 1.5.1 is not backward compatible in terms of metadata, so metadata upgrade is needed. After the upgrade, all exiting cubes are still functional. Please follow the steps:

1)	Backup metadata

To avoid data loss during the upgrade, a backup at the very beginning is always suggested. In case of upgrade failure, you can roll back to original state with the backup.

2)	Stop Kylin instance

3)	Backup conf
Copy current conf directory to other backup path.

4)	Install Kylin 1.5.1
Download the new Kylin v1.5.1 binary package from Kylin’s download page; Extract it to a different folder other than current KYLIN_HOME; Before copy back the “conf” folder, do a compare and merge between the old and new kylin.properties to ensure newly introduced property will be kept.

5)	Automaticaly upgrade metadata
Kylin v1.5.1 package provides a tool for metadata automaticly upgrade. In this upgrade, all cubes’ metadata will be updated to v1.5.1 compatible format. The treatment of empty cubes and non-empty cubes are different though. For empty cubes, we’ll upgrade the cube’s storage engine and cubing engine to the latest, so that new features will be enabled for the new coming cubing jobs. But those non-empty cubes carries legacy cube segments, so we’ll remain its old storage engine and cubing engine. In other word, the non-empty cubes will not enjoy the performance and storage wise gains released in 1.5.x versions. Check the last section to see how to deal with non-empty cubes.

```shell
export KYLIN_HOME="<path_of_1_5_0_installation>"
$KYLIN_HOME/bin/kylin.sh  org.apache.kylin.cube.upgrade.entry.CubeMetadataUpgradeEntry_v_1_5_1 <path_of_BACKUP_FOLDER>
```
6)	Upload metadata
The above commands will first copy the BACKUP_FOLDER to ${BACKUP_FOLDER}_workspace, and perform the upgrade against the workspace folder at local disk. Check the output, if no error happened, then you have a 1.5.1 compatible metadata saved in the workspace folder now. Otherwise the upgrade process is not successful, please don’t take further actions. 
The next thing to do is to override the metatdata store with the new metadata in workspace:

```shell
$KYLIN_HOME/bin/metastore.sh reset
$KYLIN_HOME/bin/metastore.sh restore <path_of_workspace>
```
7)	Redeploy HBase Coprocessor Jar
Like all version upgrade, the coprocessor need to redeploy for each upgrade. Please follow the steps at the beginning of this article. 

8)	Start Kylin Instance

### Rollback if the upgrade failed
If the new version couldn’t startup normally, you need to roll back to orignal version. The steps are as followed:

1)	Stop Kylin instance

```
$KYLIN_HOME/bin/kylin.sh stop

```

2)	Restore metadata from backup folder

```shell
export KYLIN_HOME="<path_of_old_installation>"
$KYLIN_HOME/bin/metastore.sh reset
$KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
```

3)	Deploy coprocessor 

Since coprocessor of used HTable are upgraded, you need to manually downgrade them with this command.

```shell
$KYLIN_HOME/bin/kylin.sh org.apache.kylin.job.tools.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor*.jar all
```

4)	Start Kylin instance

```shell
$KYLIN_HOME/bin/kylin.sh start
```