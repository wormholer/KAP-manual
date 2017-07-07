## 卸载 KAP
在本节中，我们将为您介绍卸载 KAP 的步骤。

完整卸载 KAP 并清除所有相关数据的步骤如下：

1. 在所有 KAP 节点上运行下述命令以停止 KAP 实例： 

   ```shell
   $KYLIN_HOME/bin/kylin.sh stop
   ```

2. 在 KAP 节点上运行下述命令以备份元数据：

   ```shell
   $KYLIN_HOME/bin/metastore.sh backup
   ```

   > 提示：我们建议您在随后将元数据拷贝至更可靠的存储设备上。

3. 请参阅[垃圾清理](/operation/storage_cleanup.cn.md)，并使用其中的方法有选择地删除 Cube 数据以清理 HBase 存储空间。

  > 如果您的 HBase 集群上只有一个 KAP 实例运行，并且您确定要卸载该实例，可以运行下述命令开启 HBase shell 并快速删除所有已构建的 Cube：
  >
  > ```shell
  > hbase shell
  > disable_all "KYLIN.*"
  > drop_all "KYLIN.*"
  > ```

4. 请您查看`$KYLIN_HOME/conf/kylin.properties`配置文件以确定工作目录的名称。假设您的相应配置项为：

  ```properties
  kylin.hdfs.working.dir=/kylin
  ```

  请您运行下述命令删除工作目录：

  ```shell
  hdfs dfs -rm -r /kylin
  ```

5. 请您查看`$KYLIN_HOME/conf/kylin.properties`配置文件以确定元数据表的名称。假设您的相应配置项为：

   ```properties
   kylin.metadata.url=kylin_metadata@hbase
   ```

   请您运行下述命令删除源数据表：

   ```shell
   hbase shell
   disable_all "kylin_metadata.*"
   drop_all "kylin_metadata.*"
   ```

6. 在所有 KAP 节点上运行下述命令以删除 KAP 安装：

   ```shell
   rm -rf $KYLIN_HOME
   ```


至此，KAP 卸载完成。
