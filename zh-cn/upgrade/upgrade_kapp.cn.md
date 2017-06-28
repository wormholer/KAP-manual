## 从KAP Plus升级##

### 从KAP Plus 2.X升级至KAP Plus更高版本###

KAP Plus 2.X各版本之间兼容元数据。因此在从KAP Plus 2.X升级至更高版本时，无需对元数据进行升级，只需要覆盖软件包、更新配置文件并升级HBase协处理器即可。具体升级步骤如下：

1. 备份元数据：

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. 停止正在运行的KAP Plus实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. 解压缩新版本的KAP Plus安装包。更新KYLIN_HOME环境变量值：

   ```shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. 更新配置文件：


   如果是从>=2.4.0的版本升级到更新的版本，只需要简单地用老版本中的的`$KYLIN_HOME/conf`替换新版本中的`$KYLIN_HOME/conf`
  
   如果是从<2.4.0的版本升级到更新的版本，需要： 1. 手动地把在老版本`$KYLIN_HOME/conf`中的改动重新在新版本的`$KYLIN_HOME/conf`重做一遍 2. 手动地把在老版本中`$KYLIN_HOME/bin/setenv.sh`中的改动再新版本中的`$KYLIN_HOME/conf/setenv.sh`重新做一遍。 注意：1. setenv.sh的目录发生了改变 2. 不允许直接拷贝-替换配置文件


5. 升级并重新部署HBase协处理器：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI default all
   ```

6. 如果升级目标是KAP Plus 2.4.X，需要对ACL数据进行迁移，执行下述命令：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.AclTableMigrationCLI MIGRATE
   ```

7. 确认License：

   在新版本的KAP Plus安装目录下确认License。

8. 启动KAP Plus实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```



