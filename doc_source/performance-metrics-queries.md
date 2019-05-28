# Viewing Query Performance Data<a name="performance-metrics-queries"></a>

 The Amazon Redshift console provides information about performance of queries that run in the database\. You can use this information to identify and troubleshoot queries that take a long time to process and that create bottlenecks preventing other queries from processing efficiently\. You can use the **Queries** tab on the cluster details page to view this information\. The **Queries** tab shows a table that lists queries that are currently running or have run recently in the cluster\. This list includes **Query ID**, **Run time**, **Start time**, and other information about the query\.

**To view query performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster for which you want to view performance data\.

1. Choose the **Queries** tab\.

   By default, the **Queries** tab displays query performance over the past 24 hours\. To change the data displayed, use the **Filter** list to select the time period for which you want to view queries\. You can also type a keyword in the **Search** box to search for queries that match your search criteria\.
**Important**  
The **Queries** tab displays the longest running queries in the system; up to 100 queries\.

## Terminating a Running Query<a name="terminate-queries"></a>

You can also use the **Queries** page to terminate a query that is currently in progress\.

**Note**  
The ability to terminate queries and loads in the Amazon Redshift console requires specific permission\. If you want users to be able to terminate queries and loads, make sure to add the `redshift:CancelQuerySession` action to your AWS Identity and Access Management \(IAM\) policy\. This requirement applies whether you select the **Amazon Redshift Read Only** AWS\-managed policy or create a custom policy in IAM\. Users who have the **Amazon Redshift Full Access** policy already have the necessary permission to terminate queries and loads\. For more information about actions in IAM policies for Amazon Redshift, see [Managing Access to Resources](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol-managingaccess)\.

**To terminate a running query**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster you want to open\.

1. Choose the **Queries** tab\.

1. Do one of the following:
   + In the list, select the query or queries that you want to terminate, and choose **Terminate Query**\.
   + In the list, open a query if you want to review the query information first, and then choose **Terminate Query**\.

1. In the **Terminate Queries** dialog box, choose **Confirm**\.