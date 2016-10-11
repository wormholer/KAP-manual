## 安装KAP

拷贝KAP二进制包至安装机器并解压。

```
cd /usr/local
tar -zxvf kylin-kap-{version}-bin.tar.gz
```

设置环境变量`KYLIN_HOME`为KAP的解压缩目录。

```
export KYLIN_HOME=/usr/local/kylin-kap-{version}-bin
```

准备一个在HDFS上的KAP工作目录。该目录默认为`/kylin`，也可以在`conf/kylin.properties`中修改目录的位置。

```
hdfs dfs -mkdir /kylin
# 并授权给KAP用户，能读写该目录
```

确保运行KAP的系统用户已经获得访问Hadoop各项服务的权限。如果不确定，可以执行`check-env.sh`快速检测，如果有问题，警告或错误信息将会输出在控制台。
```
${KYLIN_HOME}/bin/check-env.sh
```

> **可选项**：如果要在一个Hadoop集群上安装多个KAP实例，请为每个实例配置不同的元数据URL。即在`conf/kylin.properties`中设置`kylin.metadata.url`为不同的值，例如`kylin_metadata@hbase`（缺省值），或`kylin_prod@hbase`，或`kylin_qa@hbase`等。

## 启动KAP

执行`bin/kylin.sh start`，KAP将在后台开始启动，您可以用`tail`等命令观察`logs/kylin.log`文件，了解启动详细进度。

```
${KYLIN_HOME}/bin/kylin.sh start
```

要确认KAP进程正在运行，可以执行`ps -ef | grep kylin`查看进程。

> **对Plus版本**: 上述命令还会启动一个额外的Spark客户进程。它的日志在 `logs/spark_client.out`，可以用 `ps -ef | grep spark`确认该进程的存在。

> **故障排除**: 如果遇到问题，请确认所有KAP都已经停止，再重试启动。请参阅"停止KAP"。

## 打开KAP GUI

在KAP启动完成后，可以打开Web浏览器，访问此KAP实例的GUI界面`http://<host_name>:7070/kylin`。

请替换*host_name*为具体的机器名、IP地址或域名。默认KAP启动在*7070*端口，默认用户名ADMIN，默认密码KYLIN。

## 停止KAP
执行`bin/kylin.sh stop`命令，停止KAP进程。

要确认KAP进程已经停止，请执行`ps -ef | grep kylin`确认没有活跃进程。

> **对Plus版本**: 要确认Spark客户进程已经停止，请执行 `ps -ef | grep spark`确认没有活跃进程。