## 从Apache Kylin升级##

KAP是基于Apache Kylin进行二次开发的产品，支持从Apache Kylin升级至KAP或KAP Plus。

### 从Apache Kylin 1.5.1+升级至KAP 2.X###

KAP 2.X与Kylin 1.5.1+版本兼容元数据。因此在Kylin升级至KAP时，无需对元数据进行升级，只需要覆盖软件包、更新配置文件并升级HBase协处理器即可。具体升级步骤如下：

1. 备份元数据：

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. 停止正在运行的Kylin实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. 解压缩KAP安装包。更新KYLIN_HOME环境变量值：

   ```shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. 更新配置文件：


   如果是从>=2.4.0的版本升级到更新的版本，只需要简单地用老版本中的的`$KYLIN_HOME/conf`替换新版本中的`$KYLIN_HOME/conf`
  
   如果是从<2.4.0的版本升级到更新的版本，需要： 1. 手动地把在老版本`$KYLIN_HOME/conf`中的改动重新在新版本的`$KYLIN_HOME/conf`重做一遍 2. 手动地把在老版本中`$KYLIN_HOME/bin/setenv.sh`中的改动再新版本中的`$KYLIN_HOME/conf/setenv.sh`重新做一遍。 注意：1. setenv.sh的目录发生了改变 2. 不允许直接拷贝-替换配置文件


5. 修改配置参数：

   KAP中提供了两套配置参数：`conf/profile_prod/`和`conf/profile_min/`，默认情况下选择前者。如果是在沙箱等资源较为紧缺的环境下运行KAP，请执行如下操作将配置参数切换至`conf/profile_min/`：

      ```shell
   rm $KYLIN_HOME/conf/profile
   ln -s $KYLIN_HOME/conf/profile_min $KYLIN_HOME/conf/profile
      ```

   由于KAP与Kylin默认的元数据表命名方式不同，如果想要继承Kylin中已有的元数据，需要将`conf/kylin.properties`文件中的参数`kylin.metadata.url`值`kylin_default_instance@hbase`修改为与Kylin一致，默认情况下为`kylin_metadata@hbase` 。

   由于KAP与Kylin的cube逐层构建算法存在差异，如果想要使用Kylin中已构建的Cube数据，需要将`conf/kylin.properties`文件中的参数`kylin.cube.aggrgroup.is-mandatory-only-valid`的值`true`修改为`false`。


6. 升级并重新部署HBase协处理器：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
   ```

7. 刷新Cube信息：

   ```shell
   $KYLIN_HOME/bin/metastore.sh refresh-cube-signature
   ```

8. 如果升级目标是KAP 2.4.X，需要对ACL数据进行迁移，执行下述命令：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.AclTableMigrationCLI MIGRATE
   ```

9. 启动KAP实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```


### 从Apache Kylin 1.5.1+升级至KAP Plus

KAP Plus与KAP的主要区别在于引入了全新的存储引擎KyStorage。因此在从Kylin升级至KAP Plus时，只需要按照从Kylin升级至KAP的方法执行，最后进行存储引擎相关的升级操作即可。具体升级步骤如下：

1. 执行上一节“从Apache Kylin 1.5.1+升级至KAP 2.X”中的步骤1至8。其中KAP安装包用KAP Plus安装包替换。

2. 由于KAP Plus中新引入的KAP Query Driver会占用一定系统资源，如果是在沙箱等资源较为紧张的环境下运行KAP Plus，请调整相关配置以确保KAP Plus能够获取足够的资源。

3. 启动KAP Plus实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```

4. 升级之后新建的Cube默认使用KyStorage作为存储引擎，原先的Cube则使用HBase作为存储引擎。如果不需要保留原先构建好的Cube数据，可以按照如下步骤重新构建原先的Cube：

   - 保存当前元数据至`meta_backups/`路径下：

     ```shell
     $KYLIN_HOME/bin/metastore.sh backup --noSeg
     ```

   - 对`meta_backups/`路径下的元数据进行升级以使其适应KyStorage：

     ```shell
     $KYLIN_HOME/bin/metastore.sh promote $KYLIN_HOME/meta_backups
     ```

   - 将`meta_backups/`路径下的元数据恢复以覆盖原先的元数据：

     ```shell
     $KYLIN_HOME/bin/metastore.sh restore $KYLIN_HOME/meta_backups
     ```

   - 在KAP Plus GUI上重载元数据后，手动清理所有Cube中的数据并重新构建。

5. 如果需要升级原有的Cube的存储引擎为KyStorage并保留原先构建好的Cube数据，需要使用混合(Hybrid)升级。假设需要混合升级的Cube为*kylin_sales_cube*，所在项目为*learn_kylin*，Model为*kylin_sales_model*，需要执行的操作如下：

   - 在KAP Plus GUI中，克隆*kylin_sales_cube*，假设克隆得到的Cube为*kylin_sales_cube_plus*。

   - 保存*kylin_sales_cube_plus*的Cube元数据至指定路径，假设将元数据保存至`cube_backups/`:

     ```shell
     $KYLIN_HOME/bin/metastore.sh backup-cube kylin_sales_cube_plus $KYLIN_HOME/cube_backups
     ```

   - 对该Cube的元数据进行升级以使其适应KyStorage：

     ```shell
     $KYLIN_HOME/bin/metastore.sh promote $KYLIN_HOME/cube_backups
     ```

   - 将升级后的元数据恢复以覆盖原先的元数据：

     ```shell
     $KYLIN_HOME/bin/metastore.sh restore-cube learn_kylin $KYLIN_HOME/cube_backups
     ```

   - 在KAP Plus GUI上重载元数据后，执行如下命令创建Hybrid Cube：

     ```shell
     $KYLIN_HOME/bin/kylin.sh org.apache.kylin.tool.HybridCubeCLI -action create -name kylin_sales_hybrid -project learn_kylin -model kylin_sales_model -cubes kylin_sales_cube,kylin_sales_cube_plus
     ```

     如果日志中提示`HybridInstance was created at: /hybrid/kylin_sales_hybrid.json`，则表明创建成功。

   - 重载元数据。

   - 注意：后续构建请使用新建的Cube，即*kylin_sales_cube_plus*，请勿使用原先的Cube。


### 从Apache Kylin 1.5.0及之前版本升级至KAP 2.X或KAP Plus ###

Apache Kylin 1.5.1及之后版本与Apache Kylin 1.5.0及之前版本不兼容元数据。因此需要先升级至Kylin 1.5.1，再按照前述步骤进行升级。

1. 备份元数据：

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

2. 停止正在运行的Kylin实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

3. 解压缩新版本的Kylin安装包。更新KYLIN_HOME环境变量值：

   ```shell
   tar -zxvf apache-kylin-{version-env}.tar.gz
   export KYLIN_HOME=...
   ```

4. 更新配置文件：

   如果对原先版本的Kylin中`conf/`路径下的配置文件有任何修改，请将这些修改合并至新版本的Kylin相应的配置文件中。

5. 将备份的元数据拷贝至新版本Kylin的路径下，执行如下命令对其进行升级：

   ```shell
   $KYLIN_HOME/bin/kylin.sh  org.apache.kylin.cube.upgrade.entry.CubeMetadataUpgradeEntry_v_1_5_1 <path_of_BACKUP_FOLDER>
   ```

6. 重置元数据并将升级后的元数据恢复：

   ```shell
   $KYLIN_HOME/bin/metastore.sh reset
   $KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
   ```

7. 升级并重新部署HBase协处理器：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
   ```

8. 启动kylin实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```

### 
