## 升级失败后回滚##

如果升级任务失败，需要进行回滚，将系统恢复到升级前的状态，请按照以下步骤执行：

1. 停止新的Kylin、KAP或KAP Plus实例。

2. 将KYLIN_HOME环境变量值更改为原先的Kylin、KAP或KAP Plus所在路径。

3. 将升级前备份的元数据恢复：

   ```shell
   $KYLIN_HOME/bin/metastore.sh reset 
   $KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
   ```

4. 重新部署HBase协处理器。

5. 启动原先的Kylin、KAP或KAP Plus实例。