# Managing alarms<a name="performance-metrics-alarms"></a>

Alarms you create in the Amazon Redshift console are CloudWatch alarms\. They are useful because they help you make proactive decisions about your cluster and its databases\. You can set one or more alarms on any of the metrics listed in [Monitoring Amazon Redshift using CloudWatch metrics](metrics-listing.md)\. For example, setting an alarm for high `CPUUtilization` on a cluster node helps indicate when the node is overutilized\. Likewise, setting an alarm for low `CPUUtilization` on a cluster node helps indicate when the node is underutilized\. 

From **Actions**, you can modify or delete alarms\. You can also create a chime or slack alert to send an alert from CloudWatch to Slack or Amazon Chime by specifying a Slack or Amazon Chime webhook URL\.

In this section, you can find how to create an alarm using the Amazon Redshift console\. You can create an alarm using the CloudWatch console or any other way you work with metrics, such as with the AWS CLI or an AWS SDK\. 

**To create a CloudWatch alarm with the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Alarms**, then choose **Create alarm**\. 

1. On the **Create alarm** page, enter the properties to create a CloudWatch alarm\. 

1. Choose **Create alarm**\. 