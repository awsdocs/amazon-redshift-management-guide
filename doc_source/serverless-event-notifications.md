# Amazon Redshift Serverless event notifications<a name="serverless-event-notifications"></a>

## Event notifications with Amazon EventBridge<a name="serverless-event-notifications-eventbridge"></a>

Amazon Redshift Serverless uses Amazon EventBridge to manage event notifications to keep you up\-to\-date regarding changes in your data warehouse\. Amazon EventBridge is a serverless event bus service that you can use to connect your applications with data from a variety of sources\. In this case, the event source is Amazon Redshift\. Events, which are monitored changes in an environment, are sent to EventBridge from your Amazon Redshift data warehouse automatically\. Events are delivered in near\-real time\.

Capabilities of EventBridge include providing an environment for you to write event rules, which can specify actions to take for specific events\. You can also set up targets, which are resources that EventBridge can send an event to\. A target can include an API destination, an Amazon CloudWatch log group, and others\. For more information about rules, see [Amazon EventBridge rules](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-rules.html)\. For more information about targets, see [Amazon EventBridge targets](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-targets.html)\.

Events can be classified into severities and categories\. The following filters are available:
+ *Resource filtering* – Receive messages based on the resource the events are associated with\. Resources include a workgroup, a snapshot, and so on\.
+ *Time window filtering* – Scope events in a specific time period\.
+ *Category filtering* – Receive event notifications for all events in specified categories\.

The following table includes Amazon Redshift Serverless events, with additional metadata:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-event-notifications.html)