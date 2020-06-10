# Viewing cluster performance during query execution<a name="performance-metrics-query-cluster"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="cluster-query-metrics"></a>

**To display cluster performance during query execution**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Cluster performance**, **Query monitoring**, **Maintenance and monitoring**, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Query monitoring** tab for more details\. 

   For more information, see [Viewing query history data](performance-metrics-query-history.md)\. 

## Original console<a name="cluster-query-metrics-originalconsole"></a>

You can use the **Cluster Performance During Query Execution** section of the **Query** view to see cluster metrics during query execution\. Doing so helps identify poorly performing queries, look for bottleneck queries, and determine if you need to resize your cluster for your workload\.

**To view cluster metrics during query execution**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster for which you want to view cluster performance during query execution\.

1. Choose the **Queries** tab\.

1. In the query list, find the query you want to work with, and choose the query ID in the **Query** column\.

   In the following example, the queries are sorted by **Run time** to find the queries with the longest run times\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-queries-list.png)

1. In the **Query** page that opens, scroll to the **Cluster Performance During Query Execution** section to view cluster metrics\.

   In the following example, the `CPUUtilization` and `NetworkReceiveThroughput` metrics are displayed for the time that this query was running\.
**Tip**  
You can close the details of the **Query Execution Details** or **SQL** sections to manage how much information is displayed in the pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-100.png)