# Viewing Events Using the Console<a name="viewing-events-console"></a>

You can view events in the Amazon Redshift console by click on **Events** on the left navigation\. In the list of events you can filter the results using the **Source Type** filter or a custom **Filter** that filters for text in all fields of the list\. For example, if you search for "12 Dec 2012" you will match **Date** fields that contain this value\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-10.png)

An event source type indicates what the event was about\. The following source types are possible: **Cluster**, **Cluster Parameter Group**, **Cluster Security Group**, and **Snapshot**\.

## Filtering Events<a name="filtering-events"></a>

Sometimes you want to find a specific category of events or events for a specific cluster\. In these cases, you can filter the events displayed\.<a name="filtering-event-task"></a>

**To filter events**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Events**\.

1. To filter events do one of the following:

   1. To filter by event type, click **Filter Cluster** and select the source type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-20.png)

   1. To filter by text that appears in the event description, type in the in the search box and the list narrows based on what you type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-30.png)