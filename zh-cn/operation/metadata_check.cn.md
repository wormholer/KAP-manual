## 元数据检查

我们建议用户经常备份元数据，以便损坏时可以快速恢复。但有时因为各种原因造成元数据出现不一致问题，导致KAP不能正常运行，此时可以使用元数据检查及恢复的工具。



检查元数据是否存在某些不一致问题：

```shell
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker check
```
如果检查发现了元数据存在不一致的情况，执行恢复：

```shell
bin/kylin.sh io.kyligence.kap.tool.metadata.MetadataChecker recovery
```

