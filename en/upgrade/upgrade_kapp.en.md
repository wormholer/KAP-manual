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

4. 更新配置文件：Update the configuration files: 

   If you have modified any configuration files under `conf/` of the old version, please merge all of them to corresponding configuration files of the new version.  

5. Upgrade and redeploy coprocessors: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI default all
   ```

6. Confirm the License:

   Confirm the license file in the new directory of KAP. 

7. Start the KAP Plus instance: 

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```



