## 更换存储引擎为KyStorage

KAP Plus采用了全新的KyStorage存储引擎，同时也兼容Apache Kylin及KAP的HBase存储方式。在KAP Plus中新建的Cube，将自动使用新的存储引擎。

### 混合升级

如果需要升级原有Cube的存储引擎为KyStorage并且保留以前的构建数据，需要使用*Hybrid（混合）*模式。

假设原有项目为 *learn_kylin* ，Model为 *kylin_sales_model*，Cube为 *kylin_sales_cube*，操作如下：

1. 在KAP Plus中，克隆*kylin_sales_cube* 为*kylin_sales_cube_plus*，被克隆的cube自动使用KyStorage存储引擎。

2. 构建新的Cube，*kylin_sales_cube_plus*。注意新老Cube的时间区间不可以有重叠，否则会造成查询数据错误。

3. 创建Hybrid Cube，合并以上两个Cube，控制台执行命令

          bin/kylin.sh org.apache.kylin.tool.HybridCubeCLI -action create -name kylin_sales_hybrid -project learn_kylin -model    kylin_sales_model -cubes kylin_sales_cube,kylin_sales_cube_plus

   创建成功后，日志会提示```HybridInstance was created at: /hybrid/kylin_sales_hybrid.json```

4. 重载KAP元数据。

5. 后续新的构建，都基于*kylin_sales_cube_plus*，原有的*kylin_sales_cube*不再构建新的segment。



### 重写升级

如果不需要保留原有构建好的Cube数据，或者原有Cube没有采用递进式构建，则可以利用新的KyStorage存储引擎，重新构建Cube。

按照以下步骤进行升级：

1. 运行指令`<KYLIN_HOME>/bin/metastore.sh backup --noSeg`。该指令将所有的元数据保存到`meta_backups`目录下。
2. 运行指令`<KYLIN_HOME>/bin/metastore.sh promote meta_backups`。该指令升级目录中的元数据以适应KyStorage。
3. 运行指令`<KYLIN_HOME>/bin/metastore.sh restore meta_backups`。 该指令将目录中的元数据恢复，覆盖原始的元数据。
4. 禁用并清理所有的Cube。重新构建所有Cube。
