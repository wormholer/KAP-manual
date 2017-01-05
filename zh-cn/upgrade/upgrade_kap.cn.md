## KAP 升级

### 从KAP 2.X升级到更高版本

KAP 2.X 元数据兼容，只需要覆盖安装包，更新配置文件，升级HBase Coprocessor即可。

1)	备份原来KAP安装

```shell
   mv kap-{version-env} kap-{version-env}.bak
```

2)	解压缩新的KAP安装包

````shell
   tar -zxvf kap-{version-env}.tar.gz
   export KYLIN_HOME=...
````

3)	更新配置文件

   如果对`conf/`下的配置文件有任何修改，请同样更新在最新的配置文件上。

4)	确认License

   确认License文件到新的KAP安装目录下。

5)	升级HBase Coprocessor

```shell
   cd $KYLIN_HOME
   bin/kylin.sh  org.apache.kylin.storage.hbase.util.DeployCoprocessorCLI  default  all
```

6)	重新启动KAP

```shell
   bin/kylin.sh stop
   bin/kylin.sh start
```



### 从KAP升级到KAP Plus版

升级方法与普通的版本升级一样，覆盖安装包，更新配置文件，升级HBase Coprocessor即可。

KAP Plus版的主要不同在于全新的存储引擎。新建的Cube，将自动使用新的存储引擎。

老的Cube将继续使用原来的HBase存储引擎。可以查询和继续更新Cube。