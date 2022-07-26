# Amazon Redshift event notifications<a name="working-with-event-notifications"></a>

## Overview<a name="working-with-event-notifications-overview"></a>

 Amazon Redshift uses the Amazon Simple Notification Service \(Amazon SNS\) to communicate notifications of Amazon Redshift events\. You enable notifications by creating an Amazon Redshift event subscription\. In the Amazon Redshift subscription, you specify a set of filters for Amazon Redshift events and an Amazon SNS topic\. Whenever an event occurs that matches the filter criteria, Amazon Redshift publishes a notification message to the Amazon SNS topic\. Amazon SNS then transmits the message to any Amazon SNS consumers that have an Amazon SNS subscription to the topic\. The messages sent to the Amazon SNS consumers can be in any form supported by Amazon SNS for an AWS Region, such as an email, a text message, or a call to an HTTP endpoint\. For example, all regions support email notifications, but SMS notifications can only be created in the US East \(N\. Virginia\) Region\.

When you create an event notification subscription, you specify one or more event filters\. Amazon Redshift sends notifications through the subscription any time an event occurs that matches all of the filter criteria\. The filter criteria include source type \(such as cluster or snapshot\), source ID \(such as the name of a cluster or snapshot\), event category \(such as Monitoring or Security\), and event severity \(such as INFO or ERROR\)\.

You can easily turn off notification without deleting a subscription by setting the **Enabled** radio button to `No` in the AWS Management Console or by setting the `Enabled` parameter to `false` using the Amazon Redshift CLI or API\.

Billing for Amazon Redshift event notification is through the Amazon Simple Notification Service \(Amazon SNS\)\. Amazon SNS fees apply when using event notification; for more information on Amazon SNS billing, go to [ Amazon Simple Notification Service pricing](https://aws.amazon.com/sns/#pricing)\.

You can also view Amazon Redshift events that have occurred by using the management console\. For more information, see [Amazon Redshift events](working-with-events.md)\.

**Topics**
+ [Subscribing to Amazon Redshift event notifications](#working-with-event-notifications-subscribe)

### Subscribing to Amazon Redshift event notifications<a name="working-with-event-notifications-subscribe"></a>

You can create an Amazon Redshift event notification subscription so you can be notified when an event occurs for a given cluster, snapshot, security group, or parameter group\. The simplest way to create a subscription is with the Amazon SNS console\. For information on creating an Amazon SNS topic and subscribing to it, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\.

You can create an Amazon Redshift event notification subscription so you can be notified when an event occurs for a given cluster, snapshot, security group, or parameter group\. The simplest way to create a subscription is with the AWS Management Console\. If you choose to create event notification subscriptions using the CLI or API, you must create an Amazon Simple Notification Service topic and subscribe to that topic with the Amazon SNS console or Amazon SNS API\. You will also need to retain the Amazon Resource Name \(ARN\) of the topic because it is used when submitting CLI commands or API actions\. For information on creating an Amazon SNS topic and subscribing to it, see [Getting started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\.

An Amazon Redshift event subscription can specify these event criteria:
+ Source type, the values are cluster, snapshot, parameter\-groups, and security\-groups\.
+ Source ID of a resource, such as `my-cluster-1` or `my-snapshot-20130823`\. The ID must be for a resource in the same AWS Region as the event subscription\.
+ Event category, the values are Configuration, Management, Monitoring, and Security\.
+ Event severity, the values are INFO or ERROR\.

The event criteria can be specified independently, except that you must specify a source type before you can specify source IDs in the console\. For example, you can specify an event category without having to specify a source type, source ID, or severity\. While you can specify source IDs for resources that are not of the type specified in source type, no notifications will be sent for events from those resources\. For example, if you specify a source type of cluster and the ID of a security group, none of the events raised by that security group would match the source type filter criteria, so no notifications would be sent for those events\.

Amazon Redshift sends a notification for any event that matches all criteria specified in a subscription\. Some examples of the sets of events returned: 
+ Subscription specifies a source type of cluster, a source ID of my\-cluster\-1, a category of Monitoring, and a severity of ERROR\. The subscription will send notifications for only monitoring events with a severity of ERROR from my\-cluster\-1\.
+ Subscription specifies a source type of cluster, a category of Configuration, and a severity of INFO\. The subscription will send notifications for configuration events with a severity of INFO from any Amazon Redshift cluster in the AWS account\.
+ Subscription specifies a category of Configuration, and a severity of INFO\. The subscription will send notifications for configuration events with a severity of INFO from any Amazon Redshift resource in the AWS account\.
+ Subscription specifies a severity of ERROR\. The subscription will send notifications for all events with a severity of ERROR from any Amazon Redshift resource in the AWS account\.

If you delete or rename an object whose name is referenced as a source ID in an existing subscription, the subscription will remain active, but will have no events to forward from that object\. If you later create a new object with the same name as is referenced in the subscription source ID, the subscription will start sending notifications for events from the new object\.

Amazon Redshift publishes event notifications to an Amazon SNS topic, which is identified by its Amazon Resource Name \(ARN\)\. When you create an event subscription using the Amazon Redshift console, you can either specify an existing Amazon SNS topic, or request that the console create the topic when it creates the subscription\. All Amazon Redshift event notifications sent to the Amazon SNS topic are in turn transmitted to all Amazon SNS consumers that are subscribed to that topic\. Use the Amazon SNS console to make changes to the Amazon SNS topic, such as adding or removing consumer subscriptions to the topic\. For more information about creating and subscribing to Amazon SNS topics, go to [Getting started with Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\.

The following section lists all categories and events that you can be notified of\. It also provides information about subscribing to and working with Amazon Redshift event subscriptions\.

## Amazon Redshift event categories and event messages<a name="redshift-event-messages"></a>

This section shows the event IDs and categories for each Amazon Redshift source type\.

The following table shows the event category and a list of events when a cluster is the source type\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html)

The following table shows the event category and a list of events when a parameter group is the source type\.

**Categories and events for the parameter group source type**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html)

The following tables shows the event category and a list of events when a security group is the source type\.

**Categories and events for the security group source type**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html)

The following tables shows the event category and a list of events when a snapshot is the source type\.

**Categories and events for the snapshot source type**

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html)