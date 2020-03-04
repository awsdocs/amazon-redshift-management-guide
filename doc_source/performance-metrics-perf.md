# Viewing Cluster Performance Data<a name="performance-metrics-perf"></a>

By using cluster metrics in Amazon Redshift, you can do the following common performance tasks:
+ Determine if cluster metrics are abnormal over a specified time range and, if so, identify the queries responsible for the performance hit\.
+ Check if historical or current queries are impacting cluster performance\. If you identify a problematic query, you can view details about it including the cluster performance during the query's execution\. You can use this information in diagnosing why the query was slow and what can be done to improve its performance\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

## New Console<a name="cluster-performance-metric"></a>

**To view performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the name of a cluster from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **Maintenance and monitoring**, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Cluster performance** tab for performance information including the following:
   + **CPU utilization**
   + **Percentage disk space used**
   + **Database connections**
   + **Health status**
   + **Query duration**
   + **Query throughput**
   + **Concurrency scaling usage**

   Many more metrics are available\. To see the available metrics and choose which are displayed, choose the **Preferences** icon\.

## Original Console<a name="cluster-performance-metric-originalconsole"></a>

The default cluster view shows all nodes graphed together, an `Average` statistic, and data for the last hour\. You can change this view as needed\. Some metrics, such as `HealthStatus`, are only applicable for the leader node while others, such as `WriteOps`, are only applicable for compute nodes\. Switching the node display mode resets all filters\. 

**To view cluster performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the magnifying glass icon beside the cluster for which you want to view performance data\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Choose the **Performance** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-20.png)

   By default, the performance view displays cluster performance over the past hour\. If you need to fine\-tune the view, you have *filters* that you can use as described in the following table\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-30.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/performance-metrics-perf.html)

## Cluster Metrics: Examples<a name="performance-metrics-examples"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

### New Console<a name="cluster-metrics-examples"></a>

Choose the **DASHBOARD** page and find the **Cluster overview** section to see graphs of metrics about your clusters, such as the following: 
+ Number of queries
+ Database connections
+ Disk space used
+ CPU utilization

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/mgmt-cluster-overview-v1.png)

To see graphs of metrics about your cluster, choose the **CLUSTER** page, choose a cluster name, and then choose the **Cluster performance** tab\. For example, you might view a graph for **Network receive throughput**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/mgmt-cluster-networkreceivethroughput-v1.png)

### Original Console<a name="cluster-metrics-examples-originalconsole"></a>

The following example shows `CPUUtilization` and `NetworkReceiveThroughput` metrics for a single node cluster\. In this case, the graphs for cluster metrics show one line marked as **Shared** because the leader and compute node are combined\. The example shows that multiple queries were run in the time period shown\. On the **Queries** graph, the cursor is positioned over the query running at the peak values of the two metrics and the **Query ID** is displayed on the right\. You can then choose the Query ID to find out more about the query running\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-40.png)

The following example shows the `NetworkReceiveThroughput` for a cluster with two nodes\. It shows a line for the leader and two compute nodes\. The leader node metrics appear as flat and aren't of interest because data is only loaded on the compute nodes\. The example shows that one long query ran in the time period shown\. On the **Queries** graph, the cursor is positioned over the long running query and the **Query ID** is displayed on the right\. You can then choose the** Query ID** to find out more about the query running\. The `NetworkReceiveThroughput` value is displayed during the query execution\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-60.png)