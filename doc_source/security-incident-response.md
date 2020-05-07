# Logging and monitoring in Amazon Redshift<a name="security-incident-response"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Amazon Redshift and your AWS solutions\. You can collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multi\-point failure if one occurs\. AWS provides several tools for monitoring your Amazon Redshift resources and responding to potential incidents:

**Amazon CloudWatch Alarms**  
Using Amazon CloudWatch alarms, you watch a single metric over a time period that you specify\. If the metric exceeds a given threshold, a notification is sent to an Amazon SNS topic or AWS Auto Scaling policy\. CloudWatch alarms do not invoke actions because they are in a particular state\. Rather the state must have changed and been maintained for a specified number of periods\. For more information, see [Creating an alarm](performance-metrics-alarms.md)\. For a list of metrics, see [Amazon Redshift performance data](metrics-listing.md)\. 

**AWS CloudTrail Logs**  
CloudTrail provides a record of API operations taken by a IAM user, role, or an AWS service in Amazon Redshift\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Redshift, the IP address from which the request was made, who made the request, when it was made, and additional details\. For more information, see [Logging Amazon Redshift API calls with AWS CloudTrail](db-auditing.md#rs-db-auditing-cloud-trail)\.