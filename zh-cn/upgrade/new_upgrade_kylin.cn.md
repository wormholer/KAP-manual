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

   如果对Kylin中`conf/`路径下的配置文件有任何修改，请将这些修改合并至KAP相应的配置文件中。

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

8. 启动KAP实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```


### 从Apache Kylin 1.5.1+升级至KAP Plus

KAP Plus与KAP的主要区别在于引入了全新的存储引擎KyStorage。因此在从Kylin升级至KAP Plus时，只需要按照从Kylin升级至KAP的方法执行，最后进行存储引擎相关的升级操作即可。具体升级步骤如下：

1. 执行上一节“从Apache Kylin 1.5.1+升级至KAP 2.X”中的步骤1至7。其中KAP安装包用KAP Plus安装包替换。

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
     $KYLIN_HOME/bin/metastore.sh promote meta_backups
     ```

   - 将`meta_backups/`路径下的元数据恢复以覆盖原先的元数据：

     ```shell
     $KYLIN_HOME/bin/metastore.sh restore meta_backups
     ```

   - 在KAP GUI上重载元数据后，手动清理所有Cube中的数据并重新构建。

