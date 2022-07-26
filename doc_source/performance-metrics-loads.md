# Viewing cluster metrics during load operations<a name="performance-metrics-loads"></a>

When you view cluster performance during load operations, you can identify queries that are consuming resources and act to mitigate their effect\. You can terminate a load if you don't want it to run to completion\. 

**Note**  
The ability to terminate queries and loads in the Amazon Redshift console requires specific permission\. If you want users to be able to terminate queries and loads, make sure to add the `redshift:CancelQuerySession` action to your AWS Identity and Access Management \(IAM\) policy\. This requirement applies whether you select the **Amazon Redshift Read Only** AWS\-managed policy or create a custom policy in IAM\. Users who have the **Amazon Redshift Full Access** policy already have the necessary permission to terminate queries and loads\. For more information about actions in IAM policies for Amazon Redshift, see [Managing access to resources](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol-managingaccess)\.

**To display cluster performance during load operations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, which can include **Cluster performance**, **Query monitoring**, **Databases**, **Datashares**, **Schedules**, **Maintenance**, and **Properties** tabs\. 

1. Choose the **Query monitoring** tab for more details\. 

1. In the **Queries and loads** section, choose **Loads** to view the load operations of a cluster\. If a load is running, you can end it by choosing **Terminate query**\.