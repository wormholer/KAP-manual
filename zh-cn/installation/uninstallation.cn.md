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

   > 我们建议您在随后将元数据拷贝至更可靠的存储设备上。

3. 删除HBase存储；如果您的HBase集群只运行了一个KAP环境，且确定要卸载该KAP环境，可以使用hbase命令（如下）快速删除所有已构建的Cube; 否则，请谨慎使用，建议使用[垃圾清理](/operation/storage_cleanup.cn.md)中介绍的方法有选择地删除Cube。

  ```
  hbase shell
  disable_all "KYLIN.*"
  drop_all "KYLIN.*"
  ```

4. 删除KAP在HDFS上的工作目录：首先检查`conf/kylin.properties`文件，确定工作目录名，如`kylin.hdfs.working.dir=/kylin`， 使用`hdfs`命令行删除此目录：

  ```
  hdfs dfs -rm -r /kylin
  ```

5. 删除KAP元数据表，首先检查`conf/kylin.properties`文件，确定元数据表名，如`kylin.metadata.url=kylin_metadata@hbase`, 则元数据表名为`kylin_metadata`, 同时还有名为`kylin_metadata_user`和`kylin_metadata_acl`的辅助表。使用`hbase shell`删除这些表：

  ```
  hbase shell
  disable_all "kylin_metadata.*"
  drop_all "kylin_metadata.*"
  ```

6. 在所有KAP节点上删除KAP安装：rm -rf $KYLIN_HOME


至此，KAP 卸载完成。
