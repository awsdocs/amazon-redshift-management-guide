# Managing Event Notifications Using the Amazon Redshift Console<a name="manage-event-notifications-console"></a>


+ [Creating an Event Notification Subscription](#event-subscribe)
+ [Listing Your Amazon Redshift Event Notification Subscriptions](#list-event-notifications)
+ [Modifying an Amazon Redshift Event Notification Subscription](#modify-event-notifications)
+ [Adding a Source Identifier to an Amazon Redshift Event Notification Subscription](#addsource-event-notifications)
+ [Removing a Source Identifier from an Amazon Redshift Event Notification Subscription](#removesource-event-notifications)
+ [Deleting an Amazon Redshift Event Notification Subscription](#delete-event-notifications)

You can create an Amazon Simple Notification Service \(Amazon SNS\) event notification subscription to send notifications when an event occurs for a given Amazon Redshift cluster, snapshot, security group, or parameter group\. These notifications are sent to an SNS topic, which in turn transmits messages to any SNS consumers subscribed to the topic\. The SNS messages to the consumers can be in any notification form supported by Amazon SNS for an AWS region, such as an email, a text message, or a call to an HTTP endpoint\. For example, all regions support email notifications, but SMS notifications can only be created in the US East \(N\. Virginia\) Region\. For more information, see [Amazon Redshift Event Notifications](working-with-event-notifications.md)\.

This section describes how to manage Amazon Redshift event notification subscriptions from the AWS Management Console\.

## Creating an Event Notification Subscription<a name="event-subscribe"></a>

**To create an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the Amazon Redshift Console navigation pane, click **Events**, and then click the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, click **Create Event Subscription**\. 

1.  In the **Create Event Subscription** dialog box, do the following:

   1. Use the **Subscription Settings** pane to specify the event filter criteria\. As you select the criteria, the **Subscribed Events** list displays the Amazon Redshift events that match the criteria\. Do the following:

      1. Select one or more event categories from the **Categories** box\. To specify all categories, select the **Category** button\. To select a subset of the categories, select the buttons for the categories to be included\.

      1. Select an event severity from the **Severity** dropdown menu\. If you select **Any**, events with severities of either INFO or ERROR are published\. If you select **Error**, only events with a severity of ERROR are published\.

      1. Select a source type from the **Source Type** dropdown menu\. Only events raised by resources of that type, such as clusters or cluster parameter groups, are published by the event subscription\.

      1. In the **Resources** dropdown menu, specify whether events will be published from all resources having the specified **Source Type**, or only a subset\. Select **Any** to publish events from all resources of the specified type\. Select **Choose Specific** if you want to select specific resources\.
**Note**  
The name of the **Resource** box changes to match the value specified in **Source Type**\. For example, if you select **Cluster** in **Source Type**, the name of the **Resources** box changes to **Clusters**\.

         If you select **Choose Specific**, you can then specify the IDs of the specific resources whose events will be published by the event subscription\. You specify the resources one at a time and add them to the event subscription\. You can only specify resources that are in the same region as the event subscription\. The events you have specified are listed below the **Specify IDs:** box\.

         1. To specify an existing resource, find the resource in the **Specify IDs:** box, and click the **\+** button in the **Add** column\.

         1. To specify the ID of a resource before you create it, type the ID in the box below the **Specify IDs:** box and click the **Add** button\. You can do this to add resources that you plan to create later\.

         1. To remove a selected resource from the event subscription, click the **X** box to the right of the resource\.

   1. At the bottom of the pane, type a name for the event notification subscription in the **Name** text box\.

   1. Select **Yes** to enable the subscription\. If you want to create the subscription but to not have notifications sent yet, select **No**\. A confirmation message will be sent when the subscription is created, regardless of this setting\.

   1. Select **Next** to proceed to specifying the Amazon SNS topic\.  
![\[Subscription Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/event-sub-create-10.png)

   1. Use one of three tabs to specify the Amazon SNS topic the subscription will use to publish events\.

      1. To select an existing Amazon SNS topic by from a list, select the **Use Existing Topic** tab and select the topic from the list\.

      1. To specify an existing Amazon SNS topic by its Amazon Resource Name \(ARN\), select the **Provide Topic ARN** tab and specify the ARN in the **ARN:** box\. You can find the ARN of an Amazon SNS topic by using the Amazon SNS console:

         1. Sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.

         1. In the **Navigation** pane, expand **Topics**\.

         1. Click the topic to be included in the Amazon Redshift event subscription\.

         1. In the **Topic Details** pane, copy the value of the **Topic ARN:** field\.

      1. To have the subscription create operation also create a new Amazon SNS topic, select the **Create New Topic** tab and do the following:

         1. Type a name for the topic in the **Name** text box\.

         1. For each notification recipient, select the notification method in the **Send** list box, specify a valid address in the **to** box, and then click **Add Recipient**\. You can only create **SMS** entries in the US East \(N\. Virginia\) Region\.

         1. To remove a recipient, click the red X in the **Remove** column\.  
![\[Subscription Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/event-sub-create-40.png)

1. To create the subscription, click **Create**\. To delete the definition without creating a subscription, click **Cancel**\. To return to the subscription settings, click **Previous**\.

## Listing Your Amazon Redshift Event Notification Subscriptions<a name="list-event-notifications"></a>

You can list your current Amazon Redshift event notification subscriptions\.

**To list your current Amazon Redshift event notification subscriptions**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the Amazon Redshift Console navigation pane, click **Events**\. The **Subscriptions** tab shows all your event notification subscriptions\.

## Modifying an Amazon Redshift Event Notification Subscription<a name="modify-event-notifications"></a>

After you have created a subscription, you can change the subscription name, source identifier, categories, or topic ARN\.

**To modify an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the Amazon Redshift Console navigation pane, click **Events**, and then click the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, select the subscription that you want to modify, and click **Modify**\. 

1.  In the **Modify Event Subscription** dialog box, do the following:

   1. Use the **Subscription Settings** pane to change the event filter criteria\. As you select the criteria, the **Subscribed Events** list displays the Amazon Redshift events that match the criteria\. Do the following:

      1. Select one or more event categories from the **Categories** box\. To specify all categories, select the **Category** button\. To select a subset of the categories, select the buttons for the categories to be included\.

      1. Select an event severity from the **Severity** dropdown menu\.

      1. Select a source type from the **Source Type** dropdown menu\.

      1. Select the IDs of the resources from the **Source Type** dropdown menu\. Only events raised by the specified resources will be published by the subscription\.

   1. For **Enabled**, select **Yes** to enable the subscription\. Select **No** to disable the subscription\.

   1. Select **Next** to proceed to changing the Amazon SNS topic\.

   1. Use one of three tabs to change the Amazon SNS topic the subscription will use to publish events\.

      1. To select an existing Amazon SNS topic by from a list, select the **Use Existing Topic** tab and select the topic from the list\.

      1. To specify an existing Amazon SNS topic by its Amazon Resource Name \(ARN\), select the **Provide ARN** tab and specify the ARN in the **ARN:** box\.

      1. To have the subscription modify operation also create a new Amazon SNS topic, select the **Create New Topic** tab and do the following:

         1. Type a name for the topic in the **Name** text box\.

         1. For each notification recipient, select the notification method in the **Send** list box, specify a valid address in the **to** box, and then click **Add Recipient**\. You can only create **SMS** entries in the US East \(N\. Virginia\) Region\.

         1. To remove a recipient, click the red X in the **Remove** column\.

1. To save your changes, click **Modify**\. To delete your changes without modifying the subscription, click **Cancel**\. To return to the subscription settings, click **Previous**\.

## Adding a Source Identifier to an Amazon Redshift Event Notification Subscription<a name="addsource-event-notifications"></a>

You can add a source identifier \(the Amazon Redshift source generating the event\) to an existing subscription\.

**To add a source identifier to an Amazon Redshift event notification subscription**

1. You can easily add or remove source identifiers using the Amazon Redshift console by selecting or deselecting them when modifying a subscription\. For more information, see [Modifying an Amazon Redshift Event Notification Subscription](#modify-event-notifications)\.

1. To save your changes, click **Modify**\. To delete you changes without modifying the subscription, click **Cancel**\. To return to the subscription settings, click **Previous**\.

## Removing a Source Identifier from an Amazon Redshift Event Notification Subscription<a name="removesource-event-notifications"></a>

You can remove a source identifier \(the Amazon Redshift source generating the event\) from a subscription if you no longer want to be notified of events for that source\. 

**To remove a source identifier from an Amazon Redshift event notification subscription**

+ You can easily add or remove source identifiers using the Amazon Redshift console by selecting or deselecting them when modifying a subscription\. For more information, see [Modifying an Amazon Redshift Event Notification Subscription](#modify-event-notifications)\.

## Deleting an Amazon Redshift Event Notification Subscription<a name="delete-event-notifications"></a>

You can delete a subscription when you no longer need it\. All subscribers to the topic will no longer receive event notifications specified by the subscription\.

**To delete an Amazon Redshift event notification subscription**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the Amazon Redshift Console navigation pane, click **Events**, and then click the **Subscriptions** tab\. 

1.  In the **Subscriptions** pane, click the subscription that you want to delete\. 

1. Click **Delete**\.