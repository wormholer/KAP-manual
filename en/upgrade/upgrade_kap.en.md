## Upgrading from KAP ##

### Upgrading from KAP 2.X to KAP of higher versions ###

KAP 2.X shares compatible metadata with other KAP 2.X versions. Thus you could upgrade the system from KAP 2.X to KAP of higher versions by overwriting the software package, updating configuration files, and upgrading HBase coprocessors without upgrading the metadata unnecessarily. 

> Before upgrading from the older version, please ensure that all automated **metadata clean** and **storage cleanup CLI** tools are turned off to avoid the impact of the upgrade.

Please follow the steps below: 

1. Backup the metadata: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. Stop the running Kylin instance:

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. Unzip the KAP package of new version. Update the value of the environment variable KYLIN_HOME: 

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

6. If you are upgrading from KAP <2.4.0, you are required to migrate ACL data. Run commands below: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.AclTableMigrationCLI MIGRATE
   ```

7. Confirm the License:

   Confirm the license file in the new directory of KAP. 

8. Start the KAP instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```




### Upgrading from KAP to KAP Plus ###

The most mentionable difference of KAP Plus and KAP is the adoption of KyStorage, a new storage engine into the former. Thus you could upgrade the system from KAP to KAP Plus in the same way as upgrading KAP followed by operations related to storage engine. Please follow the steps below: 

1. Follow the step 1 to 6 in last section "Upgrading from KAP 2.X to KAP of higher versions", but replace KAP package with that of KAP Plus. 

2. Since the KAP Query Driver in KAP Plus would occupy some system resources, please adjust corresponding configuration to make sure KAP Plus could obtain enough resources, if you are going to run KAP in environments with shortage of resources, such as sandboxes. 

3. Start the KAP Plus instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```

4. Cubes built after upgrading use KyStorage as their storage engine, while old cubes use HBase. If it was not necessary for you to reserve data of built cubes, just follow steps below to rebuild old cubes: 

   - Save the current metadata into `meta_backups/`: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh backup --noSeg
     ```

   - Upgrade metadata in `meta_backups/` to adjust it to KyStorage: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh promote $KYLIN_HOME/meta_backups
     ```

   - Restore metadata in `meta_backups/` to overwrite the old metadata: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh restore $KYLIN_HOME/meta_backups
     ```

   - Reload metadata on KAP Plus GUI, purge all cubes manually, and rebuild them. 

5. If it was necessary for you to upgrade the storage engine to KyStorage while reserving data of built cubes, Hybrid upgrading might help. Suppose the cube you are going to upgrade in a hybrid way is *kylin_sales_cube*, and it is in a project called *learn_kylin* and is based on a model called *kylin_sales_model*. You might need to:  

   - Clone *kylin_sales_cube* in KAP Plus GUI to obtain a new cube, say, *kylin_sales_cube_plus*.

   - Save the metadata of *kylin_sales_cube_plus* to a directory, say, `cube_backups/`: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh backup-cube kylin_sales_cube_plus $KYLIN_HOME/cube_backups
     ```

   - Upgrade the metadata of the cube to adjust it to KyStorage: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh promote $KYLIN_HOME/cube_backups
     ```

   - Restore the upgraded metadata to overwrite the old metadata: 

     ```shell
     $KYLIN_HOME/bin/metastore.sh restore-cube learn_kylin $KYLIN_HOME/cube_backups
     ```

   - Reload metadata on KAP Plus GUI, then run commands below to create a hybrid cube: 

     ```shell
     $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.HybridCubeCLI -action create -name kylin_sales_hybrid -project learn_kylin -model kylin_sales_model -cubes kylin_sales_cube,kylin_sales_cube_plus
     ```

     If you find `HybridInstance was created at: /hybrid/kylin_sales_hybrid.json` in log printed out, you would know that you have done it successfully. 

   - Reload metadata. 

   - Notice: please use the new cube (*kylin_sales_cube_plus* in the example above) to build instead of the old cube. 
