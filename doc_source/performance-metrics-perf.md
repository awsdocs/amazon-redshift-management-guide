# Viewing cluster performance data<a name="performance-metrics-perf"></a>

By using cluster metrics in Amazon Redshift, you can do the following common performance tasks:
+ Determine if cluster metrics are abnormal over a specified time range and, if so, identify the queries responsible for the performance hit\.
+ Check if historical or current queries are impacting cluster performance\. If you identify a problematic query, you can view details about it including the cluster performance during the query's execution\. You can use this information in diagnosing why the query was slow and what can be done to improve its performance\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="cluster-performance-metric"></a>

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
   + **Concurrency scaling activity**

   Many more metrics are available\. To see the available metrics and choose which are displayed, choose the **Preferences** icon\.

## Original console<a name="cluster-performance-metric-originalconsole"></a>

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

## Cluster performance graphs<a name="cluster-performance-metrics-examples"></a>

The following examples show some of the graphs that are displayed in the new Amazon Redshift console\. 
+ **CPU utilization** – Shows the percentage of CPU utilization for all nodes \(leader and compute\)\. To find a time when the cluster usage is lowest before scheduling cluster migration or other resource\-consuming operations, monitor this chart to see CPU utilization per individual or all of nodes\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-cpu-utilization.png)
+ **Maintenance mode** – Shows whether the cluster is in the maintenance mode at a chosen time by using `On` and `Off` indicators\. You can see the time when the cluster is undergoing maintenance\. You can then correlate this time to operations that are done to the cluster to estimate its future downtimes for recurring events\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-maintenance-mode.png)
+ **Percentage disk space used** – Shows the percentage of disk space usage per each compute node, and not for the cluster as a whole\. You can explore this chart to monitor the disk utilization\. Maintenance operations like VACUUM and COPY use intermediate temporary storage space for their sort operations, so a spike in disk usage is expected\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-percentage-disk-space-used.png)
+ **Read throughput** – Shows the average number of megabytes read from disk per second\. You can evaluate this chart to monitor the corresponding physical aspect of the cluster\. This throughput doesn't include network traffic between instances in the cluster and its volume\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-read-throughput.png)
+ **Read latency** – Shows the average amount of time taken for disk read I/O operations per millisecond\. You can view the response times for the data to return\. When latency is high, it means that the sender spends more time idle \(not sending any new packets\), which reduces how fast throughput grows\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-read-latency.png)
+ **Write throughput** – Shows the average number of megabytes written to disk per second\. You can evaluate this metric to monitor the corresponding physical aspect of the cluster\. This throughput doesn't include network traffic between instances in the cluster and its volume\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-write-throughput.png)
+ **Write latency** – Shows the average amount of time in milliseconds taken for disk write I/O operations\. You can evaluate the time for the write acknowledgment to return\. When latency is high, it means that the sender spends more time idle \(not sending any new packets\), which reduces how fast throughput grows\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-write-latency.png)
+ **Database connections** – Shows the number of database connections to a cluster\. You can use this chart to see how many connections are established to the database and find a time when the cluster usage is lowest\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-database-connections.png)
+ **Total table count** – Shows the number of user tables open at a particular point in time within a cluster\. You can monitor the cluster performance when open table count is high\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-total-table-count.png)
+ **Health status** – Indicates the health of the cluster as `Healthy` or `Unhealthy`\. If the cluster can connect to its database and performs a simple query successfully, the cluster is considered healthy\. Otherwise, the cluster is unhealthy\. An unhealthy status can occur when the cluster database is under extremely heavy load or if there is a configuration problem with a database on the cluster\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-health-status.png)
+ **Query duration** – Shows the average amount of time to complete a query in microseconds\. You can benchmark the data on this chart to measure I/O performance within the cluster and tune its most time\-consuming queries if necessary\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-query-duration.png)
+ **Query throughput** – Shows the average number of completed queries per second\. You can analyze data on this chart to measure database performance and characterize the ability of the system to support a multiuser workload in a balanced way\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-query-throughput.png)
+ **Query duration per WLM queue** – Shows the average amount of time to complete a query in microseconds\. You can benchmark the data on this chart to measure I/O performance per WLM queue and tune its most time\-consuming queries if necessary\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-query-duration-per-wlm-queue.png)
+ **Query throughput per WLM queue** – Shows the average number of completed queries per second\. You can analyze data on this chart to measure database performance per WLM queue\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-query-throughput-per-wlm-queue.png)
+ **Concurrency scaling activity** – Shows the number of active concurrency scaling clusters\. When concurrency scaling is enabled, Amazon Redshift automatically adds additional cluster capacity when you need it to process an increase in concurrent read queries\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-performance-concurrency-scaling-activity.png)