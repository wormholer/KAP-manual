## 任务引擎HA配置

KAP的任务引擎负责执行用户递交的构建任务，当任务完成时，通知用户和各个KAP节点。默认地，在KAP集群里只能有一个节点运行任务引擎，如果该节点失效，将导致任务引擎不能自动恢复。

要启用任务引擎高可用(HA)及自动恢复，需要额外配置。

 - KAP的任务引擎HA依赖Zookeeper实现服务状态的监测和动态分配，故需要获取并在kylin.properties里配置Zookeeper信息；Zookeeper是Hadoop和HBase集群必需的服务，您可以选择使用现有的Zookeeper，或启动新的Zookeeper：
 
```
kap.job.helix.zookeeper-address=host1:2181,host2:2181,host3:2181
```

 - 在kylin.properties里设置启用基于zookeeper的任务执行器：

```
kylin.job.scheduler.default=1
```
 
 - 重启所有KAP节点，任务引擎将在所有节点中动态分配。
