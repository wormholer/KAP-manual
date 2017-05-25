## KAP配置

KAP在集群结点上安装完毕后，还需要对KAP的参数进行配置，以便将KAP接入现有的Apache Hadoop、Apache HBase、Apache Hive环境中，并根据实际环境条件对KAP进行性能优化。

在KAP安装目录的`conf/`路径下，保存了对KAP进行配置的所有配置文件。读者可以修改配置文件中的参数，以达到环境适配、性能调优等目的。在`conf/`目录下默认存在以下配置文件：
###	kylin.properties
该文件是KAP使用的全局配置文件，和KAP有关的配置项都在此文件中。具体配置项在下文中详述。
###	kylin\_hive\_conf.xml
该文件包含了Hive任务的配置项。在构建Cube的第一步通过Hive生成中间表时，会根据该文件的设置调整Hive的配置参数。
###	kylin\_job\_conf\_inmem.xml
该文件包含了Map Reduce任务的配置项。当Cube构建算法是Fast Cubing时，会根据该文件的设置调整构建任务中的Map Reduce参数。
###	kylin\_job\_conf.xml
该文件包含了Map Reduce任务的配置项。当kylin\_job\_conf\_inmem.xml不存在，或Cube构建算法是Layer Cubing时，会根据该文件的设置调整构建任务中的Map Reduce参数。