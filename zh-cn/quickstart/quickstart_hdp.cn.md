## KAP 快速上手(基于HDP)

KAP内置了样例数据集及模型，可以通过脚本，快速导入数据集和模型。更详细的安装和使用请参考手册相关章节。

## 环境准备

运行KAP需要Hadoop环境支持，KAP安装在Hadoop集群的客户端节点上。作为快速上手，我们推荐使用*All in one*的沙箱虚拟机用于本地测试，包括*Hortonworks Sandbox 2.2* 和*Cloudera QuickStart VM 5.7*。虚拟机需要至少*10G*内存。

为了避免权限问题，我们建议使用*root*账号通过SSH的方式登录虚拟机，*Hortonworks Sandbox 2.2*的默认密码是*hadoop*， *Cloudera QuickStart VM 5.7*的默认密码是cloudera。

我们建议采用网桥（bridged）模式配置虚拟机网络，网桥模式将为虚拟机分配独立IP地址，方便本机访问KAP Web页面。

### 安装KAP

拷贝KAP二进制包至安装机器并解压。

```shell
cd /usr/local
tar -zxvf kylin-kap-{version}-bin.tar.gz
```

设置环境变量`KYLIN_HOME`为KAP的解压缩目录。

```shell
export KYLIN_HOME=/usr/local/kylin-kap-{version}-bin
```

## 导入样例数据和模型

`bin/sample.sh`会创建3个Hive Table，并导入样例数据。数据导入成功后，会自动创建样例项目、模型和Cube定义。

```shell
cd kylin-kap-{version}-bin
bin/sample.sh
```

## 启动KAP

进入KAP安装目录，并执行启动脚本`bin/kylin.sh start`。

```shell
cd kylin-kap-{version}-bin
bin/kylin.sh start
```

KAP启动之后，可以通过浏览器访问，默认地址http://{hostname}:7070/kylin，默认用户名ADMIN，密码KYLIN

## 构建Cube

进入KAP后，通过页面左上角的下拉菜单选择样例项目*learn_kylin*。

进入**模型**页面，选择样例Cube *kylin_sales_cube*，选择*操作*->*构建*，选择一个晚于*2014-01-01*的日期，这样会包含全部10000条原始数据，并提交。

进入**监控**页面，可以看到正在构建的Cube任务，可以点击*刷新*，获得最新进度，直到100%。

## 执行SQL查询

当Cube构建成功后，进入**查询**页面，可以在页面左侧看到之前导入的三张表，这时可以输入SQL语句，对样例数据进行查询分析。样例语句包括：

```sql
select part_dt, sum(price) as total_selled, count(distinct seller_id) as sellers from kylin_sales group by part_dt order by part_dt
```

查询的结果会呈现在页面中，可以对比Hive验证查询的结果和响应的速度。
