## KAP Upgrade

### Upgrade from KAP 2.X to Higher KAP

The metadata of the KAP 2.X are compatible. Simply upgrade by overwrite the binary, update the config files, and redeploy the HBase coprocessor.

1)	Backup the orignal installation

```shell
   mv kap-{version-env} kap-{version-env}.bak
```

2)	Unpack the new binary

```shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
```

3)	Update configuration files

   If you have modified config files under `conf/`, please apply the changes again in the new `conf/` folder.

4)	Confirm the License file

   Confirm the license file exists in the new KAP home.

5)	Upgrade HBase Coprocessor

```shell
   cd $KYLIN_HOME
   bin/kylin.sh  org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI  default  all
```

6)	Restart KAP

````shell
   bin/kylin.sh stop
   bin/kylin.sh start
````




### Upgrade from KAP to KAP Plus

The upgrade follows the normal upgrade steps -- overwrite the binary, update the config files, and redeploy the HBase coprocessor.

KAP Plus introduces a new storage engine. New cubes will start to use the new storage engine automatically.

Old cubes will continue to use HBase as storage. They can continue to build and serve query.



