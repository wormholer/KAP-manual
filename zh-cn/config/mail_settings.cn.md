## 启用邮件通知

当任务完成或失败时，KAP可以发送邮件通知用户。如用户需要启用该功能，请在`$KYLIN_HOME/conf/kylin.properties`中配置如下内容：

```
mail.enabled=true
mail.host=your-smtp-server
mail.username=your-smtp-account
mail.password=your-smtp-pwd
mail.sender=your-sender-address
kylin.job.admin.dls=adminstrator-address
```

之后请重启KAP以使得上述配置项生效。

如需要停用该功能，请将上述配置项中的`mail.enabled`值设置为`false`。

管理员将会收到所有任务的通知。建模人员和分析师需要在Cube设计器页面中的第一页中，将自己的电子邮件地址输入至“通知邮件列表”框内，便能够收到该Cube相关任务的通知。更详细的通知事件过滤，可以在“需通知的事件”中设置。