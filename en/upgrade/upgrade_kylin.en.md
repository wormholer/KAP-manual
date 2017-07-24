## Upgrading from Apache Kylin##

KAP is developed based on Apache Kylin. Users could upgrade the system from Apache Kylin to KAP conveniently. 

### Upgrading from Apache Kylin 1.5.1+ to KAP 2.X###

KAP 2.X shares compatible metadata with Kylin 1.5.1+. Thus you could upgrade the system from Kylin to KAP by overwriting the software package, updating configuration files, and upgrading HBase coprocessors without upgrading the metadata unnecessarily. 

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

3. Unzip the KAP package. Update the value of the environment variable KYLIN_HOME: 

   ```shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. Update the configuration files: 

   If you're upgrading from Apache Kylin >=2.1, simply replace new versions' `$KYLIN_HOME/conf` with old version's `$KYLIN_HOME/conf`.

   Otherwise if you're upgrading from Apache Kylin <2.1, you need to: 1. manually re-apply all changes in old version's `$KYLIN_HOME/conf` to new version's `$KYLIN_HOME/conf`. 2. manually re-apply all changes in old version's `$KYLIN_HOME/bin/setenv.sh` to new version's `$KYLIN_HOME/conf/setenv.sh`. Watch out: 1. the folder for setenv.sh has changed. 2. Direct file copy-and-replace is not allowed.

5. Modify configuration parameters: 

   KAP provides two sets of parameters: `conf/profile_prod/` and `conf/profile_min/`, in which the former is the default setting. If you are going to run KAP in environments with shortage of resources, such as sandboxes, run commands below to switch to `conf/profile_min/`: 

   ```shell
   rm $KYLIN_HOME/conf/profile
   ln -s $KYLIN_HOME/conf/profile_min $KYLIN_HOME/conf/profile
   ```

   KAP and Kylin have different naming rules for metadata tables. Change the value `kylin_default_instance@hbase` of parameter `kylin.metadata.url` in `conf/kylin.properties` to the same as in Kylin, which is `kylin_metadata@hbase` by default, if you would like to inherent metadata existing in Kylin. 

   KAP and Kylin have subtle differences in the cube building algorithm. Change the value `true` of parameter `kylin.cube.aggrgroup.is-mandatory-only-valid` to `false` in `conf/kylin.properties`, if you would like to use your already built cubes in KAP. 

6. Upgrade and redeploy coprocessors: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
   ```

7. Update the information of cubes: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh refresh-cube-signature
   ```

8. If you are upgrading from Apache Kylin <2.1, you are required to migrate ACL data. Run commands below: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.AclTableMigrationCLI MIGRATE
   ```

9. Start the KAP instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```


### Upgrading from Kylin 1.5.1+ to KAP Plus

The most mentionable difference of KAP Plus and KAP is the adoption of KyStorage, a new storage engine into the former. Thus you could upgrade the system from Kylin to KAP Plus in the same way as upgrading to KAP followed by operations related to storage engine. Please follow the steps below: 

1. Follow the step 1 to 7 in last section "Upgrading from Apache Kylin 1.5.1+ to KAP 2.X", but replace KAP package with that of KAP Plus. 

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


### Upgrading from Apache Kylin 1.5.0 or lower versions to KAP 2.X or KAP Plus ###

Apache Kylin 1.5.1 or higher versions do not share compatible metadata with Kylin 1.5.0 or lower versions. Thus you have to upgrade the system to at least Kylin 1.5.1 before you follow the steps mentioned above. 

1. Backup the metadata: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. Stop the running Kylin instance:

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. Unzip the Kylin package of new version. Update the value of the environment variable KYLIN_HOME: 

   ```shell
   tar -zxvf apache-kylin-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. Update the configuration files: 

   If you have modified any configuration files under `conf/` of the old version, please merge all of them to corresponding configuration files of the new version.  

5. Copy the metadata to the directory of new version, then run commands below to upgrade it: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh  org.apache.kylin.cube.upgrade.entry.CubeMetadataUpgradeEntry_v_1_5_1 <path_of_BACKUP_FOLDER>
   ```

6. Reset metadata and restore the metadata upgraded: 

   ```shell
   $KYLIN_HOME/bin/metastore.sh reset
   $KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
   ```

7. Upgrade and redeploy coprocessors: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
   ```

8. Start the KAP instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```

### 
