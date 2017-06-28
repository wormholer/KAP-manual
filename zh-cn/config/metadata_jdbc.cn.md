## 使用jdbc连接其他数据库作为metastore
由于KAP Plus使用KyStorage作为cube存储介质，因而hbase仅作为metadata的存储数据库。KAP 2.4.x版本开始支持使用标准jdbc的数据库存储metadata。

### 配置jdbc形式的metadata
以下以mysql为例说明配置步骤
1. 安装部署KAP Plus
2. 在数据库中新建名为`kylin`的数据库
3. 在kap安装目录下的`$KYLIN_HOME/conf/kylin.properties`配置文件中修改`kylin.metadata.url`为`{metadata_name}@jdbc`，`{metadata_name}`为用户的metadata名，
如`{metadata_name}@jdbc`，以及jdbc的配置项，例如：`kylin_default_instance@jdbc,url=jdbc:mysql://localhost:3306/kylin,username=root,password=,maxActive=10,maxIdle=10`。
配置项的含义如下：

    *url*：jdbc的url，默认值为`jdbc:mysql://localhost:3306/kylin`；
    
    *username*：jdbc的用户名，默认值`root`；
    
    *password*：jdbc的密码，默认值为空字符串；
    
    *driverClassName*：jdbc的driver类名，默认值为`com.mysql.jdbc.Driver`；
    
    *maxActive*：最大数据库连接数，默认值为`5`；
    
    *maxIdle*：最大等待中的连接数量，默认值为`5`；
    
    *maxWait*：最大等待连接毫秒数，默认值为`1000`；
    
    *removeAbandoned*：是否自动回收超时连接，默认值为`true`；
    
    *removeAbandonedTimeout*：超时时间秒数，默认为`300`；
    
4. 在配置文件`$KYLIN_HOME/conf/kylin.properties`中配置jdbc connector jar包的路径，例如：`kylin.metadata.jdbc-connector-path=/usr/share/java/mysql-connector-java.jar`

5. 在配置文件`$KYLIN_HOME/conf/kylin.properties`中添加zookeeper的连接项`kylin.env.zookeeper-connect-string`，若部署kap的server同时部署有
zookeeper，可配置为`kylin.env.zookeeper-connect-string=localhost:2181`

6. 启动KAP

###  如何将hbase的metadata迁移至jdbc
1. 将`$KYLIN_HOME/conf/kylin.properties`的metadata配置项`kylin.metadata.url`修改为待迁移的hbase metadata配置，如：`kylin_default_instance@hbase`
2. 运行`$KYLIN_HOME/bin/metastore.sh backup`命令备份metadata，获取备份地址
3. 将metadata配置改为jdbc配置
4. 运行`$KYLIN_HOME/bin/metastore.sh restore /path/to/backup`的restore命令实现metadata的迁移，如`metastore.sh restore meta_backups/meta_2016_06_10_20_24_50`
