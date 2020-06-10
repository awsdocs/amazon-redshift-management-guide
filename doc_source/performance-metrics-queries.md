# Viewing queries and loads<a name="performance-metrics-queries"></a>

 The Amazon Redshift console provides information about queries and loads that run in the database\. You can use this information to identify and troubleshoot queries that take a long time to process and that create bottlenecks preventing other queries from processing efficiently\. You can use the queries information in the Amazon Redshift console to monitor query processing\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="metric-queries-overview"></a>

**To display query performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **QUERIES**, and then choose **Queries and loads** to display the list of queries for your account\. 

   By default, the list displays queries for all your clusters over the past 24 hours\. You can change the scope of the displayed date in the console\. 
**Important**  
The **Queries and loads** list displays the longest running queries in the system, up to 100 queries\.

## Original console<a name="metric-queries-overview-originalconsole"></a>

**To view query performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster for which you want to view performance data\.

1. By default, the **Queries** list displays query performance over the past 24 hours\. To change the data displayed, use the **Filter** list to select the time period for which you want to view queries\. You can also enter a keyword in the **Search** box to search for queries that match your search criteria\.

## Ending a running query<a name="terminate-queries"></a>

You can also use the **Queries** page to end a query that is currently in progress\.

**Note**  
The ability to terminate queries and loads in the Amazon Redshift console requires specific permission\. If you want users to be able to terminate queries and loads, make sure to add the `redshift:CancelQuerySession` action to your AWS Identity and Access Management \(IAM\) policy\. This requirement applies whether you select the **Amazon Redshift Read Only** AWS managed policy or create a custom policy in IAM\. Users who have the **Amazon Redshift Full Access** policy already have the necessary permission to terminate queries and loads\. For more information about actions in IAM policies for Amazon Redshift, see [Managing access to resources](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol-managingaccess)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="metric-queries-terminate"></a>

**To end a running query**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **QUERIES**, and then choose **Queries and loads** to display the list of queries for your account\. 

1. Choose the running query that you want to end in the list, and then choose **Terminate query**\. 

### Original console<a name="metric-queries-terminate-originalconsole"></a>

**To end a running query**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster you want to open\.

1. Choose the **Queries** tab\.

1. Do one of the following:
   + In the list, select the query or queries that you want to terminate, and choose **Terminate Query**\.
   + In the list, open a query if you want to review the query information first, and then choose **Terminate Query**\.

1. In the **Terminate Queries** dialog box, choose **Confirm**\.