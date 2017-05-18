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
   如果对原先版本的KAP Plus中`conf/`路径下的配置文件有任何修改，请将这些修改合并至新版本的KAP Plus相应的配置文件中。

5. 升级并重新部署HBase协处理器：

   ```shell
   $KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI default all
   ```

6. 确认License：

   在新版本的KAP Plus安装目录下确认License。

7. 启动KAP Plus实例：

   ```shell
   $KYLIN_HOME/bin/kylin.sh start
   ```



