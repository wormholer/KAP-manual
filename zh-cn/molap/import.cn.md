## 导入Hive数据源
目前，KAP支持Hive作为默认的输入数据源。为了使用KAP中自带的样例数据，需要把数据表导入Hive中。在KAP安装目录的bin文件夹中，有一个可执行脚本，可以把样例数据导入Hive：

```shell
$KYLIN_HOME/bin/sample.sh
```

脚本执行成功之后，在服务器执行Hive命令行，确认这些数据已经导入成功，命令如下：

```shell
hive
    hive> show tables;
    OK
    kylin_cal_dt
    kylin_category_groupings
    kylin_sales
    Time taken: 0.127 seconds, Fetched: 3 row(s)
    hive> select count(*) from kylin_sales;
    Query ID = root_20160707221515_b040318d-1f08-44ab-b337-d1f858c46d7d
    Total jobs = 1
    Launching Job 1 out of 1
    Number of reduce tasks determined at compile time: 1
    In order to change the average load for a reducer (in bytes):
      set hive.exec.reducers.bytes.per.reducer=<number>
    In order to limit the maximum number of reducers:
      set hive.exec.reducers.max=<number>
    In order to set a constant number of reducers:
      set mapreduce.job.reduces=<number>
    Starting Job = job_1467288198207_0129, Tracking URL = http://sandbox.hortonworks.com:8088/proxy/application_1467288198207_0129/
    Kill Command = /usr/hdp/2.2.4.2-2/hadoop/bin/hadoop job  -kill job_1467288198207_0129
    Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
    2016-07-07 22:15:11,897 Stage-1 map = 0%,  reduce = 0%
    2016-07-07 22:15:17,502 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.64 sec
    2016-07-07 22:15:25,039 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.37 sec
    MapReduce Total cumulative CPU time: 3 seconds 370 msec
    Ended Job = job_1467288198207_0129
    MapReduce Jobs Launched:
    Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.37 sec   HDFS Read: 505033 HDFS Write: 6 SUCCESS
    Total MapReduce CPU Time Spent: 3 seconds 370 msec
    OK
    10000
    Time taken: 24.966 seconds, Fetched: 1 row(s)
```

### 创建项目
打开KAP的Web UI，如下图所示的操作创建一个新的项目（Project），并命名为KAP_Sample_1。
![](images/dataimport_1.png)
在Web UI的左上角选择刚刚创建的项目，表示我们接下来的全部操作都在这个项目中，在当前项目的操作不会对其他项目产生影响。
![](images/dataimport_2.png)
### 同步Hive表
需要把Hive数据表同步到KAP当中才能使用。为了方便操作，我们通过“使用图形化树状结构方式加载Hive表”进行同步，如下图所示：
![](images/dataimport_3.png)
在弹出的对话框中展开default数据库，并选择需要的三张表，如图所示
![](images/dataimport_4.png)
导入后系统会自动计算各表各列的维数，以掌握数据的基本情况。稍等几分钟后，我们可以通过数据源表的详情页查看这些信息。
![](images/dataimport_5.png)