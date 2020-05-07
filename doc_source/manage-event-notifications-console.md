# Managing event notifications using the Amazon Redshift console<a name="manage-event-notifications-console"></a>

**Topics**
+ [Creating an event notification subscription](#event-subscribe)

You can create an Amazon Simple Notification Service \(Amazon SNS\) event notification subscription to send notifications when an event occurs for a given Amazon Redshift cluster, snapshot, security group, or parameter group\. These notifications are sent to an SNS topic, which in turn transmits messages to any SNS consumers subscribed to the topic\. The SNS messages to the consumers can be in any notification form supported by Amazon SNS for an AWS Region, such as an email, a text message, or a call to an HTTP endpoint\. For example, all regions support email notifications, but SMS notifications can only be created in the US East \(N\. Virginia\) Region\. For more information, see [Amazon Redshift event notifications](working-with-event-notifications.md)\.

This section describes how to manage Amazon Redshift event notification subscriptions from the AWS Management Console\.

## Creating an event notification subscription<a name="event-subscribe"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="event-subscription-create"></a>

**To create an event subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **EVENTS**\. 

1. Choose the **Event subscription** tab, then choose **Create event subscriptions**\. 

1. Enter the properties of your event subscription, such as name, source type, category, and severity\. You can also enable Amazon SNS topics to get notified of events\. 

1. Choose **Create event subscriptions** to create your subscription\. 

### Original console<a name="event-subscription-create-originalconsole"></a>

**To create an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Events**, and then choose the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, choose **Create Event Subscription**\. 

1.  In the **Create Event Subscription** dialog box, do the following:

   1. Use the **Subscription Settings** pane to specify the event filter criteria\. As you choose the criteria, the **Subscribed Events** list displays the Amazon Redshift events that match the criteria\. Do the following:

      1. For **Categories**, choose one or more event categories\. To specify all categories, choose **Category**\. To choose a subset of the categories, choose the buttons for the categories to be included\.

      1. For **Severity**, choose an event severity\. If you choose **Any**, events with severities of either INFO or ERROR are published\. If you choose **Error**, only events with a severity of ERROR are published\.

      1. For **Source Type**, choose a source type\. Only events raised by resources of that source type, such as clusters or cluster parameter groups, are published by the event subscription\.

      1. For **Resources**, specify whether events should be published from all resources having the specified source type, or only a subset\. Choose **Any** to publish events from all resources of the specified type\. Choose **Choose Specific** if you want to choose specific resources\.
**Note**  
The name of the **Resource** box changes to match the value specified in **Source Type**\. For example, if you choose **Cluster** in **Source Type**, the name of the **Resources** box changes to **Clusters**\.

         If you choose **Choose Specific**, you can then specify the IDs of the specific resources whose events are published by the event subscription\. You specify the resources one at a time and add them to the event subscription\. You can only specify resources that are in the same AWS Region as the event subscription\. The events you specify are listed below the **Specify IDs** box\.

         1. To specify an existing resource, find the resource in the **Specify IDs** box, and choose the **\+** button in the **Add** column\.

         1. To specify the ID of a resource before you create it, enter the ID in the box below the **Specify IDs** box and choose **Add**\. You can do this to add resources that you plan to create later\.

         1. To remove a resource from the event subscription, choose the **X** box to the right of the resource\.

   1. At the bottom of the pane, enter a name for the event notification subscription for **Name**\.

   1. Choose **Yes** to enable the subscription\. If you want to create the subscription but to not send notifications yet, choose **No**\. A confirmation message is sent when the subscription is created, regardless of this setting\.

   1. Choose **Next** to proceed to specifying the Amazon SNS topic\.  
![\[Subscription Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/event-sub-create-10.png)

   1. Use one of three tabs to specify the Amazon SNS topic that the subscription uses to publish events\.

      1. To choose an existing Amazon SNS topic by from a list, choose the **Use Existing Topic** tab and choose the topic from the list\.

      1. To specify an existing Amazon SNS topic by its Amazon Resource Name \(ARN\), choose the **Provide Topic ARN** tab and specify the ARN in the **ARN:** box\. You can find the ARN of an Amazon SNS topic by using the Amazon SNS console:

         1. Sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

         1. In the navigation pane, expand **Topics**\.

         1. Choose the topic to include in the Amazon Redshift event subscription\.

         1. In the **Topic Details** pane, copy the value of the **Topic ARN:** field\.

      1. To have the subscription create operation also create a new Amazon SNS topic, choose the **Create New Topic** tab and do the following:

         1. Type a name for the topic for **Name**\.

         1. For each notification recipient, choose the notification method for **Send**, specify a valid address for **to**, and then choose **Add Recipient**\. You can only create **SMS** entries in the US East \(N\. Virginia\) Region\.

         1. To remove a recipient, choose the red X in the **Remove** column\.  
![\[Subscription Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/event-sub-create-40.png)

1. To create the subscription, choose **Create**\. To delete the definition without creating a subscription, choose **Cancel**\. To return to the subscription settings, choose **Previous**\.

## \(Original console\) Managing event subscriptions<a name="event-subscription-manage-originalconsole"></a>

### Listing your Amazon Redshift event notification subscriptions<a name="list-event-notifications"></a>

You can list your current Amazon Redshift event notification subscriptions\.

**To list your current Amazon Redshift event notification subscriptions**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Events**\. The **Subscriptions** tab shows all your event notification subscriptions\.

### Modifying an Amazon Redshift event notification subscription<a name="modify-event-notifications"></a>

After you have created a subscription, you can change the subscription name, source identifier, categories, or topic ARN\.

**To modify an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Events**, and then choose the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, choose the subscription that you want to modify, and choose **Modify**\. 

1.  In the **Modify Event Subscription** dialog box, do the following:

   1. Use the **Subscription Settings** pane to change the event filter criteria\. As you choose the criteria, the **Subscribed Events** list displays the Amazon Redshift events that match the criteria\. Do the following:

      1. For **Categories**, choose one or more event categories\. To specify all categories, choose **Category**\. To choose a subset of the categories, choose the buttons for the categories to be included\.

      1. For **Severity**, choose an event severity\.

      1. For **Source Type**, choose a source type\.

      1. For **Resources**, choose the IDs of the resources from the given source type\. Only events raised by the specified resources are published by the subscription\.

   1. For **Enabled**, choose **Yes** to enable the subscription\. Choose **No** to disable the subscription\.

   1. Choose **Next** to proceed to changing the Amazon SNS topic\.

   1. Use one of three tabs to change the Amazon SNS topic for the subscription to use to publish events\.

      1. To choose an existing Amazon SNS topic from a list, choose the **Use Existing Topic** tab and choose the topic from the list\.

      1. To specify an existing Amazon SNS topic by its Amazon Resource Name \(ARN\), choose the **Provide ARN** tab and specify the ARN in the **ARN** box\.

      1. To have the subscription modify operation also create a new Amazon SNS topic, choose the **Create New Topic** tab and do the following:

         1. Enter a name for the topic for **Name**\.

         1. For each notification recipient, choose the notification method for **Send**, specify a valid address for **to**, and then choose **Add Recipient**\. You can only create **SMS** entries in the US East \(N\. Virginia\) Region\.

         1. To remove a recipient, choose the red X in the **Remove** column\.

1. To save your changes, choose **Modify**\. To delete your changes without modifying the subscription, choose **Cancel**\. To return to the subscription settings, choose **Previous**\.

### Adding a source identifier to an Amazon Redshift event notification subscription<a name="addsource-event-notifications"></a>

You can add a source identifier \(the Amazon Redshift source generating the event\) to an existing subscription\.

**To add a source identifier to an Amazon Redshift event notification subscription**

1. You can easily add or remove source identifiers using the Amazon Redshift console by selecting or deselecting them when modifying a subscription\. For more information, see [Modifying an Amazon Redshift event notification subscription](#modify-event-notifications)\.

1. To save your changes, choose **Modify**\. To delete your changes without modifying the subscription, choose **Cancel**\. To return to the subscription settings, choose **Previous**\.

### Removing a source identifier from an Amazon Redshift event notification subscription<a name="removesource-event-notifications"></a>

You can remove a source identifier \(the Amazon Redshift source generating the event\) from a subscription if you no longer want to be notified of events for that source\. 

**To remove a source identifier from an Amazon Redshift event notification subscription**
+ You can easily add or remove source identifiers using the Amazon Redshift console by selecting or deselecting them when modifying a subscription\. For more information, see [Modifying an Amazon Redshift event notification subscription](#modify-event-notifications)\.

### Deleting an Amazon Redshift event notification subscription<a name="delete-event-notifications"></a>

You can delete a subscription when you no longer need it\. All subscribers to the topic will no longer receive event notifications specified by the subscription\.

**To delete an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Events**, and then choose the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, choose the subscription that you want to delete\. 

1. Choose **Delete**\.