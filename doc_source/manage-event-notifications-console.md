# Managing event notifications using the Amazon Redshift console<a name="manage-event-notifications-console"></a>

**Topics**
+ [Creating an event notification subscription](#event-subscribe)

You can create an Amazon Simple Notification Service \(Amazon SNS\) event notification subscription to send notifications when an event occurs for a given Amazon Redshift cluster, snapshot, security group, or parameter group\. These notifications are sent to an SNS topic, which in turn transmits messages to any SNS consumers subscribed to the topic\. The SNS messages to the consumers can be in any notification form supported by Amazon SNS for an AWS Region, such as an email, a text message, or a call to an HTTP endpoint\. For example, all regions support email notifications, but SMS notifications can only be created in the US East \(N\. Virginia\) Region\. For more information, see [Amazon Redshift event notifications](working-with-event-notifications.md)\.

This section describes how to manage Amazon Redshift event notification subscriptions from the AWS Management Console\.

## Creating an event notification subscription<a name="event-subscribe"></a>

**To create an event subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Events**\. 

1. Choose the **Event subscription** tab, then choose **Create event subscriptions**\. 

1. Enter the properties of your event subscription, such as name, source type, category, and severity\. You can also enable Amazon SNS topics to get notified of events\. 

1. Choose **Create event subscriptions** to create your subscription\. 