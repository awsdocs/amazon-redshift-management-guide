# Managing cluster event notifications<a name="manage-event-notifications"></a>

You can create an Amazon Simple Notification Service \(Amazon SNS\) event notification subscription to send notifications when an event occurs for a given Amazon Redshift cluster, snapshot, security group, or parameter group\. These notifications are sent to an SNS topic, which in turn transmits messages to any SNS consumers subscribed to the topic\. The SNS messages to the consumers can be in any notification form supported by Amazon SNS for an AWS Region, such as an email, a text message, or a call to an HTTP endpoint\. For example, all regions support email notifications, but SMS notifications can only be created in the US East \(N\. Virginia\) Region\. For more information, see [Amazon Redshift event notifications](working-with-event-notifications.md)\.

## Managing cluster event notifications using the Amazon Redshift console<a name="manage-event-notifications-console"></a>

### Creating an event notification subscription<a name="event-subscribe"></a>

**To create an event subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Events**\. 

1. Choose the **Event subscription** tab, then choose **Create event subscriptions**\. 

1. Enter the properties of your event subscription, such as name, source type, category, and severity\. You can also enable Amazon SNS topics to get notified of events\. 

1. Choose **Create event subscriptions** to create your subscription\. 

## Managing cluster event notifications using the AWS CLI and Amazon Redshift API<a name="manage-event-notifications-api-cli"></a>

You can use the following Amazon Redshift CLI operations to manage cluster event notifications\.
+ [create\-event\-subscription](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-event-subscription.html)
+ [delete\-event\-subscription](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-event-subscription.html)
+ [describe\-event\-categories](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-event-categories.html)
+ [describe\-event\-subscriptions](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-event-subscriptions.html)
+ [describe\-events](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-events.html)
+ [modify\-event\-subscription](https://docs.aws.amazon.com/cli/latest/reference/redshift/modify-event-subscription.html)

 You can use the following Amazon Redshift API actions to manage event notifications\.
+ [CreateEventSubscription](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateEventSubscription.html)
+ [DeleteEventSubscription](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteEventSubscription.html)
+ [DescribeEventCategories](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEventCategories.html)
+ [DescribeEventSubscriptions](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEventSubscriptions.html)
+ [DescribeEvents](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEvents.html)
+ [ModifyEventSubscription](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyEventSubscription.html)

For more information about Amazon Redshift event notifications, see [Amazon Redshift event notifications](working-with-event-notifications.md)\.