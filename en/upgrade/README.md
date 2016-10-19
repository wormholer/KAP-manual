## KAP Upgrade

See different upgrade scenarios below.



### Upgrade from KAP 2.0 to KAP 2.1

The metadata of the two versions are compatible. Simply upgrade by overwrite the binary, update the config files, and redeploy the HBase coprocessor.

1. Backup the orignal installation

   `mv kap-{version-env} kap-{version-env}.bak`

2. Unpack the new binary

   `tar -zxvf kap-{version-env}.tar.gz`

   `export KYLIN_HOME=...`

3. Update configuration files

   If you have modified config files under `conf/`, please apply the changes again in the new `conf/` folder.

4. Confirm the license file

   Confirm the license file exists in the new KAP home.

5. Upgrade HBase Coprocessor

   `cd $KYLIN_HOME`

   `bin/kylin.sh  org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI  default  all`

6. Restart KAP

   `bin/kylin.sh stop`

   `bin/kylin.sh start`



### Upgrade from KAP Enterpise to KAP Plus

The upgrade follows the normal upgrade steps -- overwrite the binary, update the config files, and redeploy the HBase coprocessor.

KAP Plus introduces a new storage engine. New cubes will start to use the new storage engine automatically.

Old cubes will continue to use HBase as storage. They can continue to build and serve query.



