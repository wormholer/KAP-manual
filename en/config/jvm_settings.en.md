## JVM Configuration Setting

In `$KYLIN_HOME/conf/setenv.sh` (for version lower than 2.4, `$KYLIN_HOME/bin/setenv.sh`), two sample settings for `KYLIN_JVM_SETTINGS` environment variable are given. The default setting use relatively less memory. You can comment it and then uncomment the next line to allocate more memory for KAP. The default configuration is: 

```
export KYLIN_JVM_SETTINGS="-Xms1024M -Xmx4096M -Xss1024K -XX:MaxPermSize=128M -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:$KYLIN_HOME/logs/kylin.gc.$$ -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=64M"
# export KYLIN_JVM_SETTINGS="-Xms16g -Xmx16g -XX:MaxPermSize=512m -XX:NewSize=3g -XX:MaxNewSize=3g -XX:SurvivorRatio=4 -XX:+CMSClassUnloadingEnabled -XX:+CMSParallelRemarkEnabled -XX:+UseConcMarkSweepGC -XX:+CMSIncrementalMode -XX:CMSInitiatingOccupancyFraction=70 -XX:+DisableExplicitGC -XX:+HeapDumpOnOutOfMemoryError"
```

