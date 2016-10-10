KAP用户在使用过程中经常会遇到一些棘手的问题，例如Cube创建失败、SQL查询失败、SQL查询时间过长等；运维人员需要抓取相关信息并分析问题原因。此外，也可通过Kyligence提供的在线服务平台——KyBot对KAP实例进行诊断、优化、寻求技术支持。

# 诊断
KAP的Web UI上提供了一个“诊断”功能，可将相关的信息打包成zip格式压缩包，供运维人员进行问题排查。该功能的入口总共有两处：系统诊断和任务诊断

## 系统诊断
单击System页面下的Diagnosis按钮，所生成的诊断包包含整个KAP实例的诊断信息。
![](/images/monitor/diagnosis/Picture1.png)

## 任务诊断
单击Monitor页面中某个Job下的Diagnosis按钮，所生成的诊断包仅包含该任务的诊断信息。
![](/images/monitor/diagnosis/Picture2.png)

# 求助KyBot
KyBot是Kyligence提供的一个为Apache Kylin或KAP进行诊断、优化的在线服务平台。用户可以使用KyBot Agent产生KyBot诊断包，并上传到KyBot网站。

#### 登录注册
KyBot登录地址：http://kybot.io

目前KyBot正处于内测阶段，首次试用请先联系[support@kyligence.io](mailto:support@kyligence.io)申请内测帐号，取得帐号密码后即可登录。

![](/images/monitor/diagnosis/kybot_login.png)

#### KyBot诊断包生成
（仅适用于KAP用户）KAP中已默认包含了KyBot Agent，用户可以直接单击System页面的Diagnosis按钮，生成的系统诊断包可直接上传到KyBot进行分析处理。

（适用于KAP和Apache Kylin用户）在KyBot网站下载KyBot Agent二进制包，并解压到Kylin安装目录，如$KYLIN_HOME/kybot，然后执行如下命令：

```
$KYLIN_HOME/kybot/kybot.sh
```

待命令执行结束会有如下提示，路径所示即为生成的KyBot诊断包。
```
2016-09-29 12:56:59,102 INFO  [main AbstractInfoExtractor:129]:
========================================
Dump kybot package locates at:
/root/kylin-kap-2.1-beta-bin/kybot_dump/kybot_2016_09_29_12_56_26/kybot_2016_09_29_12_56_26_0D9858.zip
========================================
```

#### KyBot诊断包上传
1. 登录KyBot网站，单击页面顶部的”上传“按钮，即打开上传页面
2. 单击”浏览“按钮，选择一个生成好的KyBot诊断包，并等待其上传成功
3. 上传成功后即加入分析队列，用户可以在上传页面查看分析进度

![](/images/monitor/diagnosis/kybot_upload.png)

#### 仪表盘
待上传的KyBot诊断包分析完成，所分析的KAP实例会出现在页面顶部的列表当中：

用户可以通过左侧菜单切换不同模块的仪表盘，查看不同功能模块的统计信息。

![](/images/monitor/diagnosis/kybot_cube.png)
![](/images/monitor/diagnosis/kybot_query.png)
![](/images/monitor/diagnosis/kybot_job.png)
![](/images/monitor/diagnosis/kybot_user.png)
![](/images/monitor/diagnosis/kybot_storage.png)
![](/images/monitor/diagnosis/kybot_env.png)

#### 故障监测
KyBot可以自动检测日志中出现的异常情况，并进行统计和列表。用户可以单击左侧菜单的“故障-异常”进行查看。

![](/images/monitor/diagnosis/kybot_exception.png)

#### 技术支持
用户可以通过KyBot直接向Kyligence技术人员寻求技术支持。单击左侧菜单中的“支持”可以提交技术工单。

![](/images/monitor/diagnosis/kybot_create_ticket.png)