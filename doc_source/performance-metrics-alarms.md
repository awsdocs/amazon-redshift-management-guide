# Creating an Alarm<a name="performance-metrics-alarms"></a>

Alarms you create in the Amazon Redshift console are Amazon CloudWatch alarms\. They are useful because they help you make proactive decisions about your cluster and its databases\. You can set one or more alarms on any of the metrics listed in [Amazon Redshift CloudWatch Metrics](metrics-listing.md#cloudwatch-metrics-listing)\. For example, setting an alarm for high `CPUUtilization` on a cluster node will help indicate when the node is over\-utilized\. Likewise, setting an alarm for low `CPUUtilization` on a cluster node, will help indicate when the node is underutilized\. 

This section explains how to create an alarm using the Amazon Redshift console\. You can create an alarm using the Amazon CloudWatch console or any other way you typically work with metrics such as with the Amazon CloudWatch Command Line Interface \(CLI\) or one of the Amazon Software Development Kits \(SDKs\)\. To delete an alarm, you must use the Amazon CloudWatch console\.

**To create an alarm on a cluster metric in the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation, click **Clusters**\.

1. In the **Cluster** list, select the cluster for which you want to view cluster performance during query execution\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Select the **Events\+Alarms** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-130.png)

1. Click **Create Alarm**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-140.png)

1. In the **Create Alarm** dialog box, configure an alarm, and click **Create**\.
**Note**  
The notifications that are displayed the **Send a notification to** box are your Amazon Simple Notification Service \(Amazon SNS\) topics\. To learn more about Amazon SNS and creating topics, go to [Create a Topic](http://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html) in the *Amazon Simple Notification Service Getting Started Guide*\. If you don't have any topics in Amazon SNS, you can create a topic in the Create Alarm dialog by clicking the **create topic** link\.

   The details of your alarm will vary with your circumstance\. In the following example, the average CPU utilization of a node \(Compute\-0\) has an alarm set so that if the CPU goes above 80 percent for four consecutive five minute periods, a notification is sent to the topic **redshift\-example\-cluster\-alarms**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-20.png)

1. In the list of alarms, find your new alarm\. 

   You may need to wait a few moments as sufficient data is collected to determine the state of the alarm as shown in the following example\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-30.png)

   After a few moments the state will turn to **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-40.png)

1. \(Optional\) Click the **Name** of the alarm to change the configuration of the alarm or click the view link under **More Options** to go to this alarm in the Amazon CloudWatch console\.