## 在 FusionInsight 上快速安装 KAP

KAP 及 KAP Plus 均可以在 FusionInsight 环境中运行。在本节中，我们将引导您在 FusionInsight 环境中快速安装 KAP。

### 准备运行环境

KAP 支持在版本为 V100R002C60U20 的 FusionInsight 环境中运行。该版本中各组件的版本如下：

+ Hadoop: 2.7.2
+ HBase: 1.0.2
+ Hive: 1.3.0
+ Zookeeper: 3.5.1
+ Spark: 1.5.1

如果您需要在 FusionInsight 的环境下运行 KAP，请选择 HBase 1.x 对应的发行版。

执行下述命令以创建运行 KAP 的 Kerberos 用户并初始化环境：

```shell
kinit <user_name>
source /opt/hadoopclient/bigdata_env
```

如果您所使用的环境中 HBase 客户端和 Hive 客户端的`lib/`目录下的 thrift 包版本不一致，请保留较高版本并将较低版本的 thrift 包移出`lib/`目录并备份。

### 下载安装 KAP

1. 获取 KAP 软件包。您可以访问 [KAP release notes](../../release/README.md)，选择适合您的版本；

2. 将 KAP 软件包拷贝至您需要安装 KAP 的服务器或虚拟机，并解压至安装路径下。我们假设您的安装路径为`/usr/local/`。运行下述命令：

   ```shell
   cd /usr/local
   tar -zxvf kap-{version}.tar.gz
   ```

3. 将环境变量`KYLIN_HOME`的值设为 KAP 解压后的路径：

   ```shell
   export KYLIN_HOME=/usr/local/kap-{version}
   ```

4. 在 HDFS 上创建 KAP 的工作目录，并授予启动 KAP 的账户读写该工作目录的权限。默认的工作目录为`/kylin`。KAP需要向`/user/{current_user}`目录下写入临时数据，需要创建对应目录。运行下述命令：

   ```shell
   hdfs dfs -mkdir /kylin
   hdfs dfs -chown root /kylin
   hdfs dfs -mkdir /user/root
   hdfs dfs -chown root /user/root
   ```

   > 提示：您可以在`$KYLIN_HOME/conf/kylin.properties`配置文件中修改 KAP 工作目录的位置。

   **注意：如果您所使用的账户在 HDFS 上没有读写权限，请先转至`hdfs`账户，然后再创建工作目录并授予权限。**执行下述命令：

   ```shell
   su hdfs
   hdfs dfs -mkdir /kylin
   hdfs dfs -chown root /kylin
   hdfs dfs -mkdir /user/root
   hdfs dfs -chown root /user/root
   ```

5. 请您将 Hive 客户端的`hivemetastore-site.xml`文件中的所有配置项拷贝至`hive-site.xml`文件中。

   **注意：对于 KAP Plus 2.4 及以上版本，还需要将`hive-site.xml`文件拷贝至`$KYLIN_HOME/spark/conf/`路径下。**

   如果您需要运行的是 KAP 而非 KAP Plus，请您将 HBase 客户端的`hbase-site.xml`文件中的所有配置项拷贝至`$KYLIN_HOME/conf/kylin_job_conf.xml`文件中。

6. 在 FI Manager 页面中，依次点击 **Hive** - **配置（全部配置）**- **安全** - **白名单**，将`$KYLIN_HOME/conf/kylin_hive_conf.xml`文件中的所有 Hive 配置项的 key（如`dfs.replication`）添加至 FI Hive 配置的白名单中。此外，对于 KAP Plus 2.2 及以上版本，还需要额外将`mapreduce.job.reduces`配置项添加至白名单中。

7. 请您在 FI 客户端中输入 **beeline**，并复制 **Connect to** 后面的内容：**jdbc:hive2://…HADOOP.COM**，并且在`$KYLIN_HOME/conf/kylin.properties`中进行如下配置：

   ```properties
   kylin.source.hive.client=beeline
   kylin.source.hive.beeline-params=-n root -u 'jdbc:hive2://…HADOOP.COM'
   ```

### 检查运行环境

首次启动 KAP 之前，KAP 会对所依赖的环境进行检查。如果在检查过程中发现问题，您将在控制台中看到警告或错误信息。

检查中遇到的一部分问题可能是由于无法有效获取环境依赖信息导致的。如果遇到这类问题，请您运行如下命令，显示指定 KAP 获取环境依赖信息的途径：

```properties
export HIVE_CONF=HIVE_CLIENT_CONF //hive客户端的配置文件路径,不是hive路径
export HIVE_LIB=HIVE_CLIENT_LIB //hive客户端的lib路径
export HCAT_HOME=HCATALOG_DIR
export SPARK_HOME=$KYLIN_HOME/spark //对于 KAP Plus 2.2 以上版本
```

> 提示：您可以在任何时候手动检查运行环境。运行下述命令：
>
> ```shell
> $KYLIN_HOME/bin/check-env.sh
> ```

如果检查运行环境时提示缺少 HBase 权限，请您在 FI Manager 页面上创建一个新用户，并将该用户添加至`supergroup`用户组下，分配权限`System_administrator`。然后，请您运行下述命令，将 KAP 工作目录的所有者更改为该用户：

```shell
hdfs dfs -chown -R <user_name> <working_directory>
```

如果检查运行环境时提示未安装 Snappy，您可以自行安装 Snappy，也可以在`$KYLIN_HOME/conf/kylin.properties`配置文件中修改下述与 Snappy 相关的配置项：

```properties
kylin.storage.hbase.compression-codec=none
# kap.storage.columnar.page-compression=SNAPPY //注释掉该项
```

### 启动 KAP

运行下述命令以启动 KAP：

```shell
$KYLIN_HOME/bin/kylin.sh start
```

> 您可以执行下述命令以观察启动的详细进度：
>
> ```shell
> tail $KYLIN_HOME/logs/kylin.log
> ```

启动成功后，您将在控制台中看到提示信息。此时可以运行下述命令以查看 KAP 进程是否正在运行：

```shell
ps -ef | grep kylin
```

### 访问 KAP GUI

当 KAP 顺利启动后，您可以打开 web 浏览器，访问`http://<host_name>:7070/kylin/`。请将其中`<host_name>`替换为具体的 host 名、IP 地址或域名。默认端口值为`7070`。默认用户名和密码分别为`ADMIN`和`KYLIN`。

当您成功从 KAP GUI 登录后，可以通过构建 Sample Cube 以验证 KAP 的功能。请参阅[安装验证](install_validate.cn.md)。

### 停止 KAP

运行下述命令以停止运行 KAP：

```shell
$KYLIN_HOME/bin/kylin.sh stop
```

您可以运行下述下述命令以查看 KAP 进程是否已停止：

```shell
ps -ef | grep kylin
```


