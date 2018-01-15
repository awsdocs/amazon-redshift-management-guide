# Viewing Cluster Performance Data<a name="performance-metrics-perf"></a>

Cluster metrics in Amazon Redshift enable the following common performance use cases:

+ Determine if cluster metrics are abnormal over a specified time range and, if so, identify the queries responsible for the performance hit\.

+ Check if historical or current queries are impacting cluster performance\. If you identify a problematic query, you can view details about it including the cluster performance during the query's execution, information which may assist you in diagnosing why the query was slow, and what can be done to improve its performance\.

The default cluster view shows all nodes graphed together, an `Average` statistic, and data for the last hour\. You can change this view as needed\. Some metrics, such as `HealthStatus`, are only applicable for the leader node while others, such as `WriteOps`, are only applicable for compute nodes\. Switching the node display mode will reset all filters\. 

**To view cluster performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation, click **Clusters**\.

1. In the **Cluster** list, click the magnifying glass icon beside the cluster for which you want to view performance data\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Select the **Performance** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-20.png)

   By default, the performance view displays cluster performance over the past hour\. If you need to fine tune the view you have *filters* that you can use as described in the following table\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-30.png)    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/performance-metrics-perf.html)

## Cluster Metrics: Examples<a name="performance-metrics-examples"></a>

The following example shows `CPUUtilization` and `NetworkReceiveThroughput` metrics for a single node cluster\. In this case the graphs for cluster metrics show one line marked as **Shared** since the leader and compute node are combined\. The example shows that multiple queries were run in the time period shown\. On the **Queries** graph the cursor is positioned over the query running at the peak values of the two metrics and the **Query ID** is displayed on the right\. You could then click the Query ID to find out more about the query running\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-40.png)

The following example shows the `NetworkReceiveThroughput` for a cluster with two nodes\. It shows a line for the leader and two compute nodes\. Note that the leader node metrics is flat and is not of interest since data is only loaded on the compute nodes\. The example shows that one long query ran in the time period shown\. On the **Queries** graph the cursor is positioned over the long running query and the **Query ID** is displayed on the right\. You could then click the** Query ID** to find out more about the query running\. The `NetworkReceiveThroughput` value is displayed during the query execution\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-60.png)