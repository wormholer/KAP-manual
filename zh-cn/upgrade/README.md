## KAP 升级

KAP的元数据配置信息保存在HBase中，*KAP 2.1*与*KAP 2.0*在数据模型上完全兼容。升级时，仅需要替换安装包即可。

KAP采用了HBase coprocessor来优化查询性能，更新版本后，由于RPC通信协议可能改变，因此需要为HTable重新部署coprocessor。需要执行以下命令：

```shell
$KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
```

在*KAP*与*KAP Plus*之间升级时，由于存储引擎不同，Cube数据无法重用，因此需要重新构建Cube。



