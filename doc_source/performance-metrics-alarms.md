# Creating an alarm<a name="performance-metrics-alarms"></a>

Alarms you create in the Amazon Redshift console are CloudWatch alarms\. They are useful because they help you make proactive decisions about your cluster and its databases\. You can set one or more alarms on any of the metrics listed in [Amazon Redshift performance data](metrics-listing.md)\. For example, setting an alarm for high `CPUUtilization` on a cluster node helps indicate when the node is overutilized\. Likewise, setting an alarm for low `CPUUtilization` on a cluster node helps indicate when the node is underutilized\. 

In this section, you can find how to create an alarm using the Amazon Redshift console\. You can create an alarm using the CloudWatch console or any other way you work with metrics, such as with the AWS CLI or an AWS SDK\. To delete an alarm, you must use the CloudWatch console\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="alarm-create"></a>

**To create a CloudWatch alarm with the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **ALARMS**, then choose **Create alarm**\. 

1. On the **Create alarm** page, enter the properties to create a CloudWatch alarm\. 

1. Choose **Create alarm**\. 

## Original console<a name="alarm-create-originalconsole"></a>

**To create an alarm on a cluster metric in the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster for which you want to view cluster performance during query execution\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Choose the **Events\+Alarms** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-130.png)

1. Choose **Create Alarm**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-140.png)

1. In the **Create Alarm** dialog box, configure an alarm, and choose **Create**\.
**Note**  
The notifications that are displayed the **Send a notification to** box are your Amazon Simple Notification Service \(Amazon SNS\) topics\. To learn more about Amazon SNS and creating topics, see [Create a topic](https://docs.aws.amazon.com/sns/latest/gsg/CreateTopic.html) in the *Amazon Simple Notification Service Getting Started Guide*\. If you don't have any topics in Amazon SNS, you can create a topic in the Create Alarm dialog by choosing the **create topic** link\.

   The details of your alarm vary with your circumstance\. In the following example, the average CPU utilization of a node \(Compute\-0\) has an alarm set\. If the CPU goes above 80 percent for four consecutive five\-minute periods, this alarm sends a notification to the topic **redshift\-example\-cluster\-alarms**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-20.png)

1. In the list of alarms, find your new alarm\. 

   You might need to wait a few moments as sufficient data is collected to determine the state of the alarm as shown in the following example\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-30.png)

   After a few moments, the state should turn to **OK**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-alarm-40.png)

1. \(Optional\) Choose **Name** for the alarm to change the configuration of the alarm or choose the view link under **More Options** to go to this alarm in the CloudWatch console\.