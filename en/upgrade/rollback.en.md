## Rollback in case of upgrading failure##

If the new version couldn’t startup normally, you need to rollback to the original version. Please follow the steps below: 

1. Stop the new Kylin, KAP or KAP Plus instance. 

2. Update the value of the environment variable KYLIN_HOME to the original directory. 

3. Restore metadata form backup folder: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh reset 
   $KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
   ```

4. Redeploy coprocessors. 

5. Start the original Kylin, KAP or KAP Plus instance. 