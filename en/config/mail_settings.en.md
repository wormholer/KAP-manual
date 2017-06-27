## Enable email notification

KAP can send email notification on job complete/fail. To enable this, edit `conf/kylin.properties`, set the following parameters: 

```
mail.enabled=true
mail.host=your-smtp-server
mail.username=your-smtp-account
mail.password=your-smtp-pwd
mail.sender=your-sender-address
kylin.job.admin.dls=adminstrator-address
```

Restart KAP server to take effective. 

To disable, set `mail.enabled` back to `false`.

Administrator will get notifications for all jobs. Modeler and Analyst need entering email address into the “Notification List” at the first page of Cube wizard, and then will get notifications for that Cube.