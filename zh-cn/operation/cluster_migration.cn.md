## KAP跨Hadoop集群迁移

目前跨集群迁移只支持**KAP Enterprise Plus**，包括：KAP整体的迁移及针对单个Cube的迁移。

**KAP整体跨集群迁移包括两个子任务:**

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



**单个Cube跨集群迁移也包括两个子任务:****

- 从SHC备份Cube的元数据及其构建数据并上传到HDFS上的/tmp目录下
- 利用HDFS的Distcp命令将KAP在SHC的HDFS工作目录拷贝到DHC的HDFS上

 整个任务是通过脚本`bin/kylin io.kyligence.kap.tool.release.KapCubeMigrationCLI`来实现的。

**必要条件：**

迁移的集群间是连通的，并且集群间的命令: **hadoop distcp**是可用的。

**使用步骤:**

*步骤一：*

在SHC上的KAP目录下执行`bin/kylin io.kyligence.kap.tool.release.KapCubeMigrationCLI backup cubeName`。

其中cubeName是要迁移的Cube的名字。

*步骤二：*

在DHC上的KAP目录下执行`bin/kylin io.kyligence.kap.tool.release.KapCubeMigrationCLI restore cubeName toProject hdfs://SHC_IP overwrite `

其中cubeName是要迁移的Cube的名字，toProject是Cube要加入的project，hdfs://SHC_IP是源集群的HDFS地址，overwrite为true表示如果目的project已经存在同名Cube则覆盖，为false将抛异常，提示已有同名Cube存在。