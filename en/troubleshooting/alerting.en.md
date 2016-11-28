# Job Alert

In KAP, building a Cube often takes at least a few minutes. Therefore, when a Cube build job is completed or failed, the operation and maintenance personals often want to be notified at the first time for the following incremental construction and troubleshooting. KAP provides a message notification function, which can send emails to Cube administrators when Cube status changed. 



## Setting Steps

Users want to set the job alert by e-mail, then they need to set in the configuration file kylin.properties: `mail.enabled`  set it as true to start mail notification function; `mail.host` Set it as the SMTP server address for the mail; `mail.username` set it as the SMTP login user name for the mail; `mail.password` set it as the SMTP login password; `mail.sender` set it as the send e-mail address. After all these setting completed, restart KAP service and these setting would work. 

The next, users need to configure for Cube. First set Cube's email contactors as email notification recipients, which shown as follow:![](images/alerting/Picture1.png)Besides, select some status as the trigger condition for the message notification. Send a message to the users if the Cube build job is switched to these states.![](images/alerting/Picture2.png)