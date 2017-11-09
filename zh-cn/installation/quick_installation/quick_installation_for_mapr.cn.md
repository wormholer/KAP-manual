## 在 MapR 沙箱中快速安装 KAP

为使您可以尽快体验到 KAP 的强大功能，我们推荐您将 KAP 与 All-in-one sandbox VM 等沙箱软件一起配合使用。在本节中，我们将引导您在 MapR 沙箱中快速安装 KAP。

### 准备运行环境

为了使 KAP 能够获取足够的资源正常运行，首先请确保您为即将运行 KAP 的沙箱分配了充足的资源。详情请参阅 KAP 安装前置条件。

在配置沙箱时，我们推荐您使用 bridged adapter 模型替代 NAT 模型。bridged adapter 模型将为您的沙箱分配一个独立的 IP 地址，使您可以任意选择通过本地或远程访问 KAP GUI。

为了避免权限问题，我们推荐您使用 MapR 的 `root` 账户访问 MapR 沙箱。MapR 5.2.1 的默认密码是`mapr`。本节中均以`root`账户为例。

### 下载安装 KAP

1. 获取 KAP 软件包。您可以访问 [KAP release notes](../../release/README.md)，选择适合您的版本；

2. 将 KAP 软件包拷贝至您需要安装 KAP 的服务器或虚拟机，并解压至安装路径下。我们假设您的安装路径为`/usr/local/`，安装 KAP 所使用的 Linux 账户为`root`。运行下述命令：

   ```shell
   cd /usr/local
   tar -zxvf kap-{version}.tar.gz
   ```

3. 将环境变量`KYLIN_HOME`的值设为 KAP 解压后的路径：

   ```shell
   export KYLIN_HOME=/usr/local/kap-{version}
   ```

4. 在 MapR-FS 上创建 KAP 的工作目录，并授予启动 KAP 的账户读写该工作目录的权限。默认的工作目录为`/kylin`。KAP需要向`/user/{current_user}`目录下写入临时数据，需要创建对应目录。运行下述命令：

   ```shell
   hadoop fs -mkdir /kylin
   hadoop fs -chown root /kylin
   ```

   > 提示：您可以在`$KYLIN_HOME/conf/kylin.properties`配置文件中修改 KAP 工作目录的位置。

### 快速配置 KAP

由于沙箱等单节点环境能够提供的资源有限，我们推荐您在单节点上安装 KAP 时，对 KAP 进行配置以限制其使用的资源。在`$KYLIN_HOME/conf/`路径下，我们已经为您准备了两套配置方案：`profile_prod`和`profile_min`，前者是默认方案，适用于实际生产环境；后者使用较少的资源，适用于沙箱等环境。运行下述命令，切换为`profile_min`配置：

```shell
rm -f $KYLIN_HOME/conf/profile
ln -sfn $KYLIN_HOME/conf/profile_min $KYLIN_HOME/conf/profile
```

为了使用MapR文件系统，需要将KAP的默认工作目录指向MapR-FS(maprfs:///)。更新kylin.properties文件

```
kylin.env.hdfs-working-dir=maprfs:///kylin
```

### 检查运行环境

首次启动 KAP 之前，KAP 会对所依赖的环境进行检查。如果在检查过程中发现问题，您将在控制台中看到警告或错误信息。

检查中遇到的一部分问题可能是由于无法有效获取环境依赖信息导致的。如果遇到这类问题，您可以尝试通过环境变量显示指定 KAP 获取这些信息的途径。示例如下：

```shell
export HIVE_CONF=/opt/mapr/hive/hive-2.1/conf
export SPARK_HOME=/opt/mapr/spark/spark-2.1.0
```

**注意：在MapR中文件操作命令为`hadoop fs`，而非`hdfs`，这会导致KAP检查运行环境时无法通过，这时候只需将`$KYLIN_HOME/bin/check-os-command.sh`脚本中的`hdfs`命令检查注释即可。示例如下：**

```shell
#command -v hdfs                         || quit "ERROR: Command 'hdfs' is not accessible. Please check Hadoop client setup."
```

> 提示：您可以在任何时候手动检查运行环境。运行下述命令：
>
> ```shell
> $KYLIN_HOME/bin/check-env.sh
> ```

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

