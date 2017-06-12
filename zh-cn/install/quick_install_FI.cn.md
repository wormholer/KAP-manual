## 在FusionInsight快速安装KAP

### FI发行版环境介绍

FI集群版本：FusionInsight V100R002C60U20



| HADOOP    | 2.7.2 |
| --------- | ----- |
| ZOOKEEPER | 3.5.1 |
| HBASE     | 1.0.2 |
| HIVE      | 1.3.0 |
| SPARK     | 1.5.1 |

### 环境兼容性检测及安装

关于KAP详细安装文档请参考：

[https://kyligence.gitbooks.io/kap-manual/content/zh-cn/](https://kyligence.gitbooks.io/kap-manual/content/zh-cn/)

1．FI支持KAP Hbase1.x系列产品，即：KAP企业版及KAP Plus版，两个版本都适用本文档。 

2．Kerberos用户登录即环境初始化。

```shell
kinit YOUR_DEFINED_USER   //通过FI Manager创建运行KAP的kerberos用户
source /opt/hadoopclient/bigdata_env 
```

3．HBASE和HIVE的客户端的lib目录下可能包含了不同版本的thrift包(分别是:libthrift-0.9.0.jar和libthrift-0.9.3)，建议保留0.9.3版本，即：可将libthrift-0.9.0.jar移出lib目录并重命名为libthrift-0.9.0.jar.bak。

4．将Hive客户端的hivemetastore-site.xml合并到hive-site.xml。即:将hivemetastore-site.xml里所有的property拷贝到hive-site.xml里。

5．将Hbase客户端的hbase-site.xml合并到$KYLIN_HOME/conf/kylin_job_conf.xml。即:将hbase-site.xml里所有的property拷贝到kylin_job_conf.xml。

6．在FI管理页面里，将$KYLIN_HOME/conf/kylin_hive_conf.xml涉及到的所有hive配置项的key，如: *dfs.replication*添加到FIHive配置的白名单里（FI管理界面:Hive－配置（全部配置）－安全－白名单），另外如果使用KAP  Plus，并且版本是2.2及以上，还需要额外添加mapreduce.job.reduces到白名单。

7．编辑$KYLIN_HOME/bin/find-hive-dependency.sh，将hive_lib变量中的$hive_exec_path替换成客户端的HIVE_CLIENT_LIB。

8．导出环境变量：
```shell
export KYLIN_HOME=KAP_DIR
export HIVE_CONF=HIVE_CLIENT_CONF //hive客户端的配置文件路径,不是hive路径
export HCAT_HOME=HCATALOG_DIR
export SPARK_HOME=$KYLIN_HOME/spark  //注：针对KAP Plus 版本>=2.2
```

9．配置kap/conf/kylin.properties：

	kylin.source.hive.client=beeline

在FI客户端输入beeline，复制Connect to后面的内容： **jdbc:hive2://…HADOOP.COM**，并按如下格式赋值：
kylin.source.hive.beeline-params="-n root --hiveconf hive.security.authorization.sqlstd.confwhitelist.append='mapreduce.job.*|dfs.*' -u ‘**COPY_TO_HERE**’"。

对于KAP Plus版本需要配置kerberos认证：

在安装节点上，从Spark客户端conf的spark-defaults.conf里找到配置项：

spark.yarn.am.extraJavaOptions，将-Djava.security.auth.login.config开始的内容拼接到	kylin.properties对应的配置项kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions后面，如：

```bash
kap.storage.columnar.spark-conf.spark.yarn.am.extraJavaOptions=-Dhdp.version=current -Djava.security.auth.login.config=/opt/huawei/Bigdata/FusionInsight/spark/cfg/jaas-zk.conf-Dzookeeper.server.principal=zookeeper/hadoop.hadoop.com -Djava.security.krb5.conf=/opt/huawei/Bigdata/FusionInsight/spark/cfg/kdc.conf
```

kap.storage.columnar.spark-conf.spark.driver.extraJavaOptions及kap.storage.columnar.spark-conf.spark.executor.extraJavaOptions作类似修改

如果KAP安装的客户端没有 spark.driver.extraJavaOptions里提到的相关配置文件，还需要将集群上 spark.driver.extraJavaOptions里提到的相关配置文件拷贝的客户机一样的目录里，否则会报找不到kerberos默认配置找不到的错误。

10．环境检测

运行$KYLIN_HOME/bin/check-env.sh，如果没有任何错误就可以准备启动KAP了。

**在环境检测中遇到的问题及解决方法：**

1.    提示HBase没有权限。

      解决方法：在FusionInsight Web_UI管理平台上创建一个新的用户，如：kap。将该用户添加到supergroup用户组下，角色分配的权限为System_administrator。 另外，需要将KAP工作目录，如：/kylin的owner更改为kap用户，可使用如下命令“hdfsdfs –chown －R kap /kylin”

2. 提示没有安装SNAPPY。

      解决方法：

      ​方案一，安装SNAPPY。

      ​方案二，注释掉配置文件conf/kylin.properties中的snappy相关设置。 具体如下，设置kylin.storage.hbase.compression-codec=none，注释掉kap.storage.columnar.page-compression=SNAPPY。

### 启动KAP服务

1. 启动命令$KYLIN_HOME/bin/kylin.shstart，可以通过命令tail–f $KYLIN_HOME/logs/kylin.log观察KAP运行日志。

2. 打开浏览器，访问此KAP服务器的GUI界面`http://<host_name>:7070/kylin`。如果打不开页面，请确认客户机防火墙是否允许访问7070端口。

3. 从Web前端登录，默认用户名ADMIN，默认密码KYLIN。

4. 成功登录KAP后，可以通过构建Sample Cube验证安装的正确性。请继续阅读[安装验证](install/install_validate.cn.md)。

