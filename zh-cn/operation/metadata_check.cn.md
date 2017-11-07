## 元数据检查

我们建议用户经常备份元数据，以便损坏时可以快速恢复。仍然有一些非人为原因会造成元数据出现不一致，导致KAP不能正常运行，此时可以使用元数据检查及恢复的工具。

### 检查范围

目前我们总结了系统中可能存在不一致的元数据的环节，具体检查范围如下：

1. cube与模型（cube与model的一致性检测）
2. cube与table index
3. cube与scheduler job
4. job的元数据与输出信息（即job本身的一致性检测）



### 使用方法

检查元数据是否存在某些不一致问题：

```shell
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker check
```
如果检查发现了元数据存在不一致的情况，执行恢复（删除）：

> 注意：这一步骤将删除系统中检测出来的孤立元数据。

```shell
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker recovery
```

