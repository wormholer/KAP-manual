## KAP跨Hadoop集群迁移

目前跨集群迁移只支持**KAP Enterprise Plus**。

**跨集群迁移包括两个子任务:**

- 从Source Hadoop Cluster (SHC)备份KAP的元数据到本地，然后上传到KAP在HDFS上的工作目录
- 利用HDFS的Distcp命令将KAP在SHC的HDFS工作目录拷贝到Destination Hadoop Cluster (DHC)的HDFS上

 整个任务是通过脚本`cluster-migration.sh` 来实现的。

**必要条件：**

迁移的集群间是连通的，并且集群间的命令: **hadoop distcp**是可用的。

**使用步骤:**

*步骤一：*

将KAP Plus分别部署到两个集群上，并且保证它们拥有相同的 `kylin.metadata.url` 和 `kylin.env.hdfs-working-dir`. 同时保证KAP在SHC没有运行。

*步骤二：*

保证 `cluster-migration.sh`在两个集群上都存在并且是可执行的。

*步骤三：*

在SHC上，运行`bin/cluster-migration.sh backup`

*步骤四：*

在DHC上，运行`bin/cluster-migration.sh restore hdfs://SHC-namenode/kylin_working_dir`