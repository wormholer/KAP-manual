## Basic Operations
Every day, KAP server accepts multiple Cube building jobs submitted by different customers KAP, and often because of improper design or abnormal cluster environment, then Cube building jobs would cost unacceptable long time or goes to failure, which needs more attention from operation and maintain personnel. Additionally, after KAP service running for a period, some data on the HBase or HDFS would become useless, which needs regular cleaning for storage garbage.



## Operation Tasks

As KAP operation and maintenance personnel, the daily work needs to be clear as follow:
* Ensure KAP service runs normally

* Ensure KAP uses cluster resources properly

* Ensure Cube building job goes normally

* Disaster backup and recovery   

* So on and forth

  ​

As the saying goes "to do good work, one must first sharpen his tools", operation personnel would better to get familiar with tools mentioned in this chapter, in order to monitor KAP daily service as well as handle unforeseen situation quickly. For instance, to ensure KAP service running normally, operation personnel should keep an eye on KAP logs; to ensure KAP uses cluster resources properly,  operation personnel would better check busy level of MR job queue frequently and HBase storage utilization(such as number HTable and Region, etc); to ensure Cube building job goes normally,  operation personnel needs to monitor jobs according to the email notification or the monitor page of Web GUI.
