# Monitoring the Data API<a name="data-api-monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of the Data API and your other AWS solutions\. AWS provides the following monitoring tools to watch the Data API, report when something is wrong, and take automatic actions when appropriate: 
+ Amazon EventBridge can be used to automate your AWS services and respond automatically to system events, such as application availability issues or resource changes\. Events from AWS services are delivered to EventBridge in near\-real time\. You can write simple rules to indicate which events are of interest to you and which automated actions to take when an event matches a rule\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. 
+ AWS CloudTrail captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. For more information, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\. 

**Topics**
+ [Monitoring events for the Amazon Redshift Data API in Amazon EventBridge](data-api-monitoring-events.md)
+ [Logging Amazon Redshift Data API calls with AWS CloudTrail](logging-using-cloudtrail.md)