## 服务发现及任务引擎高可用

### 服务发现
KAP 从2.0版本开始支持服务发现，即在多个 KAP 实例组成的集群中，当一个 KAP 实例启动、停止或意外中断通讯时，集群中的其他实例能够自动发现并更新状态。在KAP 2.4版本中新增了基于 Apache Curator 框架的服务发现功能，相较于之前的版本更为稳定易用。

基于 Curator 的服务发现默认会被启用。如果您运行 KAP 时没有使用 HBase 作为 metastore，那么您应当在`$KYLIN_HOME/conf/kylin.properties`文件中配置 Zookeeper 的地址，例如：

```properties
kylin.env.zookeeper-connect-string=host1:2181,host2:2181,host3:2181
```

> 提示：您可以运行下述命令观察结果是否以`@hbase`结尾，以判断使用的 metastore 是否为 HBase：
>
> ```shell
> $KYLIN_HOME/bin/get-properties.sh kylin.metadata.url
> ```

配置完成后，请重新启动 KAP。在重新启动时，每个 KAP 实例会在 Zookeeper 上进行注册，从而使得集群中的任何一个 KAP 实例都能够通过 Zookeeper 发现其他的实例。

KAP 在 Zookeeper 上进行注册时，默认使用 hostname 作为节点名，使用`$KYLIN_HOME/tomcat/conf/server.xml`文件中配置的 HTTP 端口作为服务端口，如`slave1:7070`。如果您运行 KAP 的集群无法通过 DNS 等服务自动将主机名解析为对应的 IP 地址，您应当在启动 KAP 之前显示指定 KAP 的 REST 地址，以避免 KAP 实例之间通信错误，例如：

```shell
export KYLIN_REST_ADDRESS=10.0.0.100:7070
$KYLIN_HOME/bin/kylin.sh start
```

### 任务引擎高可用

KAP 任务引擎负责调度和执行 Cube 构建任务，并在构建任务完成时通知用户并更新 Cube 状态。通常由用户所指定的集群中的某个节点担当此角色。

当您启用服务发现后，任务引擎将在所有能够充当任务引擎的节点中动态选择。如果当前充当任务引擎的实例发生故障退出，其他实例中的某一个实例将会接管其职能并使得任务继续进行，从而保证 KAP 服务的高可用。

> 提示：您可以查看`$KYLIN_HOME/conf/kylin.properties`配置文件以确认该实例是否可以充当任务引擎。如果`kylin.server.mode`配置项的值被设为`all`或者`job`，则该节点能够充当任务引擎。

如果您不希望某些 KAP 实例充当任务引擎，那么只需要在`$KYLIN_HOME/conf/kylin.properties`配置文件中进行下述配置，使该实例仅充当查询引擎即可：

```properties
kylin.server.mode=query
```

每一个KAP节点都可以接收用户的Cube构建请求，请求会被放入系统统一的构建排队队列中，当前角色为`job`的节点会从队列中处理Cube构建请求，`job`节点会将有效的构建请求提交到Hadoop／Spark集群。

如果原有`job`节点失效，其他表识为`all`或者`job`的节点中的一个会自动切换为当前有效的`job`节点，并继续跟踪正在构建的任务，以及处理后续的构建请求。如果没有有效的`job`节点，则构建任务将失效。