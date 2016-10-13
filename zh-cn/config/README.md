## KAP 配置

把KAP安装到集群节点之后，往往还需要对KAP进行配置，一方面将KAP接入现有的Apache Hadoop、Apache HBase、Apache Hive环境，另一方面可以根据实际环境条件对KAP进行性能优化。

在KAP安装目录的conf目录里，保存了对KAP进行配置的所有配置文件。读者可以修改配置文件中的参数，以达到环境适配、性能调优等目的。在conf目录下默认存在以下配置文件：
###	kylin.properties
该文件是KAP服务所用的全局配置文件，和KAP有关的配置项都在此文件中。具体配置项在下文会有详细讲解。
###	kylin\_hive\_conf.xml
该文件包含了Apache Hive任务的配置项。在构建Cube的第一步通过Hive生成中间表时，会根据该文件的设置调整Hive的配置参数。
###	kylin\_job\_conf\_inmem.xml
该文件包含了Map Reduce任务的配置项。当Cube构建算法是Fast Cubing时，会根据该文件的设置来调整构建任务中的Map Reduce参数。
###	kylin\_job\_conf.xml
该文件包含了Map Reduce任务的配置项。当kylin\_job\_conf\_inmem.xml不存在，或Cube构建算法是Layer Cubing时，用来调整构建任务中的Map Reduce参数。