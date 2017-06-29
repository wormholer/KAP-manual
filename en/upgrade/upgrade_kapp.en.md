## Upgrading from KAP Plus##

### Upgrading from KAP Plus 2.X to KAP Plus of higher versions###

KAP Plus 2.X shares compatible metadata with other KAP Plus 2.X versions. Thus you could upgrade the system from KAP Plus 2.X to KAP Plus of higher versions by overwriting the software package, updating configuration files, and upgrading HBase coprocessors without upgrading the metadata unnecessarily. Please follow the steps below: 

1. Backup the metadata: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. Stop the running Kylin instance:

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. Unzip the KAP Plus package of new version. Update the value of the environment variable KYLIN_HOME: 

   ```shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. Update the configuration files: 

   If you're upgrading from >=2.4.0 to a newer version, simply replace new versions' `$KYLIN_HOME/conf` with old version's `$KYLIN_HOME/conf`.
  
   Otherwise if you're upgrading from <2.4.0, you need to: 1. manually re-apply all changes in old version's `$KYLIN_HOME/conf` to new version's `$KYLIN_HOME/conf`. 2. manually re-apply all changes in old version's `$KYLIN_HOME/bin/setenv.sh` to new version's `$KYLIN_HOME/conf/setenv.sh`. Watch out: 1. the folder for setenv.sh has changed. 2. Direct file copy-and-replace is not allowed.

5. Upgrade and redeploy coprocessors: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI default all
   ```

6. If you are upgrading from KAP Plus <2.4.0, you are required to migrate ACL data. Run commands below: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.AclTableMigrationCLI MIGRATE
   ```

7. Confirm the License:

   Confirm the license file in the new directory of KAP. 

8. Start the KAP Plus instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```



