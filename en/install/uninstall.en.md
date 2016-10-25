## Uninstall KAP
KAP runs in non-invasive manner, so stop KAP instance processes stops all KAP cluster activities.

To uninstall KAP completely and erase all data, please follow these steps:

1. Stop KAP instance on all nodes: `$KYLIN_HOME/bin/kylin.sh stop`
2. Back up metadata on one KAP server and copy it to other reliable storage: `$KYLIN_HOME/bin/metastore.sh backup`
3. Delete data in HBase. If you only host one KAP environment in HBase cluster, following commands can be used to delete all built Cubes, otherwise refer to section [Storage Cleanup](../operation/storage_cleanup.en.md).

  ```
  hbase shell
  disable_all "KYLIN.*"
  drop_all "KYLIN.*"
  ```

4. Delete KAP directory on HDFS. First check file `${KYLIN_HOME}/conf/kylin.properites` and find out working directory. For example `kylin.hdfs.working.dir=/kylin`, execute following command to delete it:

  ```
  hdfs fs -rmr /kylin
  ```

5. Delete KAP metadata table. First check file `conf/kylin.properties` and find out metadata table name. For example `kylin.metadata.url=kylin_metadata@hbase`, metadata table name is `kylin_metadata`, also there are two supporting tables: `kylin_metadata_user` and `kylin_metadata_acl`. execute following command to delete them:

  ```
  hbase shell
  disable_all "kylin_metadata.*"
  drop_all "kylin_metadata.*"

  ```

6. Delete all KAP binary on all KAP nodes:
```
rm -rf $KYLIN_HOME
```

Uninstallation completes now.
