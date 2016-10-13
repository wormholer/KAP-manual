---
layout: docs15
title:  元数据恢复
categories: howto
permalink: /docs15/howto/howto_backup_metadata.html
---

## 元数据恢复

有了元数据备份，往往用户还需要对元数据从备份进行恢复。和备份类似，KAP中提供了一个元数据恢复的工具，具体用法是：

```
$KYLIN_HOME/bin/metastore.sh restore /path/to/backup
```
如果从前一节元数据备份的例子中恢复，需要在命令行中执行：
```
cd $KYLIN_HOME
bin/metastore.sh restore meta_backups/meta_2016_06_10_20_24_50
```
等待恢复操作成功，用户可以在Web UI上单击“重新加载元数据”按钮对元数据缓存进行刷新，即可看到最新的元数据。

请注意，做恢复操作时，会用本地元数据覆盖远端元数据，所以请确认从备份到恢复期间，KAP处于关闭／无活动（包括构建任务）状态，否则从备份到恢复之间的其它元数据改变会因此而丢失。如果希望KAP服务不受影响，请使用下面的方式操作。

## 有选择的元数据恢复（推荐）

如管理员只是改动某几个元数据文件，可以只恢复这几个文件，而无需覆盖所有元数据。相比于全部恢复，这种做法效率高、对用户影响小，故推荐使用；步骤如下：

* 创建一个新的空目录，按要恢复的元数据文件所在的位置创建子目录；例如恢复一个Cube实例信息，应创建cube子目录：

```
mkdir /path/to/restore_new
mkdir /path/to/restore_new/cube

```

* 将要恢复的元数据文件拷贝到此新目录下

```
cp meta_backups/meta_2016_06_10_20_24_50/cube/kylin_sales_cube.json /path/to/restore_new/cube/

```
此时可以用编辑器对元数据做修改。

* 从此目录下恢复
```
cd $KYLIN_HOME
bin/metastore.sh restore /path/to/restore_new
```

同样，等待恢复操作成功，用户可以在Web UI上单击“重新加载元数据”按钮对元数据缓存进行刷新，即可看到最新的元数据。
