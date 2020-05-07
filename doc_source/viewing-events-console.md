# Viewing events using the console<a name="viewing-events-console"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="event-view"></a>

**To view events**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **EVENTS**\. 

## Original console<a name="event-view-originalconsole"></a>

To view events in the Amazon Redshift console, choose **Events** on the navigation pane\. In the list of events, you can filter the results using **Source Type** or a custom **Filter** value that filters for text in all fields of the list\. For example, if you search for **12 Dec 2012**, you match **Date** fields that contain this value\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-10.png)

An event source type indicates what the event was about\. The following source types are possible: **Cluster**, **Cluster Parameter Group**, **Cluster Security Group**, and **Snapshot**\.

### Filtering events<a name="filtering-events"></a>

Sometimes, you might want to find a specific category of events or events for a specific cluster\. In these cases, you can filter the events displayed\.<a name="filtering-event-task"></a>

**To filter events**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Events**\.

1. To filter events, do one of the following:

   1. To filter by event type, choose **Filter Cluster** and choose the source type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-20.png)

   1. To filter by text in the event description, enter a value in the search box\. The list narrows based on what you type\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/events-list-30.png)