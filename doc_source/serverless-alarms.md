# Managing Amazon Redshift Serverless alarms<a name="serverless-alarms"></a>

Alarms you create in the Amazon Redshift Serverless console are CloudWatch alarms\. They can help you make proactive decisions about your serverless namespaces, workgroups, and database by tracking any of the metrics listed in [Audit logging for Amazon Redshift Serverless](serverless-audit-logging.md)\. 

 For example, you can set an alarm for high `DatabaseConnections` to see when your workgroup is experiencing heavy load, or one for high `DataStorage` to keep track of the storage space that your namespace is using for your data\.

You can edit or delete alarms that you’ve created using the **Actions** dropdown list in the **Alarms** page\. You can also create a Slack or Amazon Chime alert to send an alert from CloudWatch to Slack or Amazon Chime by specifying a Slack or Amazon Chime webhook URL\.

You can create an alarm using the CloudWatch console, the Amazon Redshift Serverless console, or any other way you work with metrics, such as the AWS CLI or an AWS SDK\.

**To create a CloudWatch alarm with the Amazon Redshift Serverless console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose **Go to Serverless** on the upper right of the dashboard\.

1. On the navigation menu, choose **Alarms**, then choose **Create alarm**\. 

1. On the **Create alarm** page, enter the properties to create a CloudWatch alarm\. 

1. Choose **Create alarm**\. 