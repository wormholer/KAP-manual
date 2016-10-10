## Metadata Restore

With the metadata backups, users can restore the metadata. Similar to the backup, KAP provides a  recovery tool, the usage is as follows:

```
$KYLIN_HOME/bin/metastore.sh restore /path/to/backup
```
If you recover from the previous example backup, run command:
```
cd $KYLIN_HOME
bin/metastore.sh restore meta_backups/meta_2016_06_10_20_24_50
```
When the restore operation is finished, click the "Reload Metadata" button on Web UI to refresh the cache, and then can see the latest metadata.

Note that the recover operation will overwrite the whole remote metadata with the local one, so you'd better make sure the KAP service is in a stopped or inactive state (no building task) when doing restore; otherwise some changes in remote metastore might be lost. If you want to minimal the impact, please use the "Selective Metadata Recover" method.

## Selective Metadata Recover (Recommended)

If only changes a couple of metadata files, the administrator can just pick these files to restore, without having to cover all the metadata. Compared to the full recovery, this approach is more efficient, less impaction to users, so it is recommended.

* Create a new empty directory, and then create subdirectories in it according to the location of the metadata files to restore; for example, to restore a Cube instance, you should create a "cube" subdirectory：
```
mkdir /path/to/restore_new
mkdir /path/to/restore_new/cube

```

* Copy the metadata file to be restored to this new directory
```
cp meta_backups/meta_2016_06_10_20_24_50/cube/kylin_sales_cube.json /path/to/restore_new/cube/

```
At this point you can use modify/fix the metadata manually.

* Restore from this directory
```
cd $KYLIN_HOME
bin/metastore.sh restore /path/to/restore_new
```

Similarly, after the recovery is finished, click "Reload Metadata" button on the Web UI to flush cache.