## 修改JVM参数

在`$KYLIN_HOME/bin/setenv.sh`中，为KYLIN_JVM_SETTINGS给出了两种示例配置。默认配置使用的内存较少，用户可以根据自己的实际情况，注释掉默认配置并取消另一配置前的注释符号以启用另一配置，从而为KAP示例分配更多的内存资源。该项配置的默认值如下：

```
export KYLIN_JVM_SETTINGS="-Xms1024M -Xmx4096M -Xss1024K -XX:MaxPermSize=128M -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:$KYLIN_HOME/logs/kylin.gc.$$ -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=64M"
# export KYLIN_JVM_SETTINGS="-Xms16g -Xmx16g -XX:MaxPermSize=512m -XX:NewSize=3g -XX:MaxNewSize=3g -XX:SurvivorRatio=4 -XX:+CMSClassUnloadingEnabled -XX:+CMSParallelRemarkEnabled -XX:+UseConcMarkSweepGC -XX:+CMSIncrementalMode -XX:CMSInitiatingOccupancyFraction=70 -XX:+DisableExplicitGC -XX:+HeapDumpOnOutOfMemoryError"
```

