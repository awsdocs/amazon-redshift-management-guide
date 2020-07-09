# Viewing cluster metrics during load operations<a name="performance-metrics-loads"></a>

When you view cluster performance during load operations, you can identify queries that are consuming resources and act to mitigate their effect\. You can terminate a load if you don't want it to run to completion\. 

**Note**  
The ability to terminate queries and loads in the Amazon Redshift console requires specific permission\. If you want users to be able to terminate queries and loads, make sure to add the `redshift:CancelQuerySession` action to your AWS Identity and Access Management \(IAM\) policy\. This requirement applies whether you select the **Amazon Redshift Read Only** AWS\-managed policy or create a custom policy in IAM\. Users who have the **Amazon Redshift Full Access** policy already have the necessary permission to terminate queries and loads\. For more information about actions in IAM policies for Amazon Redshift, see [Managing access to resources](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol-managingaccess)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="cluster-load-metrics"></a>

**To display cluster performance during load operations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **Maintenance and monitoring**, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Query monitoring** tab for more details\. 

1. In the **Queries and loads** section, choose **Loads** to view the load operations of a cluster\. If a load is running, you can end it by choosing **Terminate query**\.

## Original console<a name="cluster-load-metrics-originalconsole"></a>

**To view cluster metrics during load operations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster for which you want to view cluster performance during query execution\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Choose the **Loads** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-110.png)

1. In the load list, find the load operation you want to work with, and choose the load ID in the **Load** column\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-120.png)

1. In the new **Query** tab that is opened, you can view the details of the load operation\.

   At this point, you can work with the **Query** tab as shown in [Viewing queries and loads](performance-metrics-queries.md)\. You can review the details of the query and see the values of cluster metrics during the load operation\.

**To terminate a running load**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster that you want to open\.

1. Choose the **Loads** tab\.

1. Do one of the following:
   + In the list, select the load or loads that you want to terminate, and choose **Terminate Load**\.
   + In the list, open a load if you want to review the load information first, and then choose **Terminate Load**\.

1. In the **Terminate Loads** dialog box, choose **Confirm**\.