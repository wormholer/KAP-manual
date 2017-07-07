## 服务发现及任务引擎高可用

### 服务发现
多个KAP实例可以组成一个集群一起协同工作。当一个KAP实例启动、停止或者意外丢失的时候，集群中的其它实例能够自动发现并更新状态，称为服务发现。

KAP从2.0开始就支持服务发现。KAP 2.4新增了基于Apache Curator框架的服务发现功能，它比之前的版本更为稳定易用。基于Curator的服务发现默认会被启用。

当用户环境中没有使用HBase作为metadata store的时候（可以在KYLIN_HOME中运行`bin/get-properties.sh kylin.metadata.url`观察返回值是否以`@hbase`结尾），需要额外为KAP配置ZOOKEEPER地址：

- 在`kylin.properties`中配置Zookeeper地址

```
kylin.env.zookeeper-connect-string=host1:2181,host2:2181,host3:2181
```

- 重启KAP

重启后，每个KAP会在Zookeeper上注册自己，这样集群的每个实例都会从Zookeeper上发现其它实例。 

KAP默认会使用`hostname -f`的结果值做节点名，使用`tomcat/conf/server.xml`中配置的HTTP端口作为服务端口，然后注册到Zookeeper，例如: `salve1:7070`。这需要集群的其它节点能够解析此节点名（通过DNS等服务）。如果您的集群无法自动解析主机名到合适的IP地址，可能会导致KAP实例之间的通信错误。对于这种情况，您可以在启动KAP时显式指定KAP的REST地址，例如：

```
export KYLIN_REST_ADDRESS=10.0.0.100:7070
$KYLIN_HOME/bin/kylin.sh start
```

### 任务引擎高可用

KAP任务引擎执行Cube构建任务，当构建完成时通知用户和更新Cube状态。 通常集群中只能有一个任务引擎在运行，并由用户指定某个节点担当此角色。

当您启用服务发现后，任务引擎会在可以运行任务引擎(`kylin.server.mode=all` or `kylin.server.mode=job`)的节点中动态分配。当运行任务引擎的实例发生故障退出时，其它实例会接管并集训运行，从而保证了服务的高可用。

如果您不希望某些KAP实例运行任务引擎，那么只要配置它们只运行查询引擎即可：

```
kylin.server.mode=query
```
