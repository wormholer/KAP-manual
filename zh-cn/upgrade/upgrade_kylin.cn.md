## 从Apache Kylin升级

KAP是基于Apache Kylin进行二次开发的，因此支持从Apache Kylin升级到KAP。

#### 从Apache Kylin 1.5.1+升级到KAP 2.X

KAP 2.X与Kylin 1.5.1+版本的元数据、Cube数据都是兼容的，不需要对数据进行升级。但是因为HBase协处理的通讯协议发生了改动，而已有的HTable仍然绑定了老版本的HBase协处理器Jar包，因此必须为现有的HTable重新部署HBase协处理器Jar包。在命令行中执行如下命令即可为所有的HTable重新部署HBase协处理器jar包：

```shell
$KYLIN_HOME/bin/kylin.sh org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI $KYLIN_HOME/lib/kylin-coprocessor-*.jar all
```

#### 从Apache Kylin 1.5.0及之前版本升级到KAP 2.X

如果用户希望从Apache Kylin 1.5.0升级到KAP 2.X，需要先升级到Apache Kylin 1.5.1，然后再按照上文的介绍从Apache Kylin 1.5.1+升级到KAP 2.X。
Apache Kylin 1.5.1和Apache  Kylin 1.5.0及之前版本的元数据格式并不兼容，用户需要对元数据进行升级。在升级后，原有的已构建好的Cube仍然可以继续进行查询。以下是升级过程：

1)	备份元数据根据上文介绍的元数据备份方法，在执行升级操作前对元数据进行备份，以便于在升级失败后进行回滚。

2)	停止正在运行的Kylin实例

3)	备份配置文件

   将目前Kylin安装目录中的conf目录备份至其他目录

4)	安装Kylin 1.5.1

   前往Kylin官网下载1.5.1版本的二进制包，并解压到一个新的目录中，并把上一步备份好的conf目录复制到该安装目录。特别注意，新版本可能引入了一些新的配置参数，用户需要对新老版本的配置文件进行手动比较和合并。

5)	执行升级

   把新的安装目录设置为KYLIN_HOME，然后将第一步的元数据备份拷贝到该目录。接下来在命令行中执行如下命令，会对拷贝进来的元数据文件进行升级：

   ```shell
     export KYLIN_HOME="<path_of_1_5_0_installation>"
     $KYLIN_HOME/bin/kylin.sh  org.apache.kylin.cube.upgrade.entry.CubeMetadataUpgradeEntry_v_1_5_1 <path_of_BACKUP_FOLDER>
   ```

6)	上传元数据

   利用上文介绍的元数据恢复工具，将升级后的元数据恢复到Kylin实例的元数据仓库中。如果这新老版本实例的元数据仓库使用了同一个HTable，则需要先对元数据仓库进行重置。可以参考如下命令：
   ```shell
   $KYLIN_HOME/bin/metastore.sh reset
   $KYLIN_HOME/bin/metastore.sh restore <path_of_workspace>
   ```

7)	重新部署HBase协处理器Jar包

   和所有版本升级一样，此处也需要对HBase协处理器进行重新部署，新版本的HBase协处理器Jar包放置在安装目录的lib目录中。具体部署方法可以参考前文，在此不再赘述。

8)	启动新Kylin实例

### 升级失败后回滚

如果升级任务失败，导致生产系统长时间无法运行，往往会对业务系统产生严重的影响。因此，为了保证生产系统的正常运行，必须立即在升级失败后进行回滚，将系统恢复到升级前的状态。一下是回滚的操作步骤：

1)	停止新Kylin或KAP实例

2)	恢复元数据

   正如上文所述，升级前必须对元数据进行备份。这一步就需要从升级前备份的元数据目录中对元数据仓库进行恢复。如果新老Kylin实例的元数据仓库使用同一个HTable，需要先对元数据进行重置。请参考以下命令：

   ```shell
   export KYLIN_HOME="<path_of_old_installation>"
   $KYLIN_HOME/bin/metastore.sh reset 
   $KYLIN_HOME/bin/metastore.sh restore <path_of_BACKUP_FOLDER>
   ```


3)	重新部署HBase协处理器

   因为升级过程中给HTable部署了新版本的HBase协处理器Jar包，回滚时需要把老版本的HBase协处理Jar包部署到HTable中。老版本的HBase协处理器Jar包放置于Kylin安装目录的lib目录中。具体部署方法可以参考前文，在此不再赘述。

4)	启动老版本Kylin实例