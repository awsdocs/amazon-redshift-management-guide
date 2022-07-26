# Viewing queries and loads<a name="performance-metrics-queries"></a>

 The Amazon Redshift console provides information about queries and loads that run in the database\. You can use this information to identify and troubleshoot queries that take a long time to process and that create bottlenecks preventing other queries from processing efficiently\. You can use the queries information in the Amazon Redshift console to monitor query processing\. 

**To display query performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Queries and loads** to display the list of queries for your account\. 

   By default, the list displays queries for all your clusters over the past 24 hours\. You can change the scope of the displayed date in the console\. 
**Important**  
The **Queries and loads** list displays the longest running queries in the system, up to 100 queries\.

## Ending a running query<a name="terminate-queries"></a>

You can also use the **Queries** page to end a query that is currently in progress\.

**Note**  
The ability to terminate queries and loads in the Amazon Redshift console requires specific permission\. If you want users to be able to terminate queries and loads, make sure to add the `redshift:CancelQuerySession` action to your AWS Identity and Access Management \(IAM\) policy\. This requirement applies whether you select the **Amazon Redshift Read Only** AWS managed policy or create a custom policy in IAM\. Users who have the **Amazon Redshift Full Access** policy already have the necessary permission to terminate queries and loads\. For more information about actions in IAM policies for Amazon Redshift, see [Managing access to resources](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol-managingaccess)\.

**To end a running query**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Queries and loads** to display the list of queries for your account\. 

1. Choose the running query that you want to end in the list, and then choose **Terminate query**\. 