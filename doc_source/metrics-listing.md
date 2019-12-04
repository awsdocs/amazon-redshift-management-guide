# Amazon Redshift Performance Data<a name="metrics-listing"></a>

Using CloudWatch metrics for Amazon Redshift, you can get information about your cluster's health and performance and see information at the node level\. When working with these metrics, keep in mind that each metric has one or more dimensions associated with it\. These dimensions tell you what the metric is applicable to, that is the scope of the metric\. Amazon Redshift has the following two dimensions:
+ Metrics that have a `NodeID` dimension are metrics that provide performance data for nodes of a cluster\. This set of metrics includes leader and compute nodes\. Examples of these metrics include `CPUUtilization`, `ReadIOPS`, `WriteIOPS`\. 
+ Metrics that have only a `ClusterIdentifier` dimension are metrics that provide performance data for clusters\. Examples of these metrics include `HealthStatus` and `MaintenanceMode`\. 
**Note**  
In some metric cases, a cluster\-specific metric represents an aggregation of node behavior\. In these cases, take care in the interpretation of the metric value because the leader node's behavior is aggregated with the compute node\.

For general information about CloudWatch metrics and dimensions, see [CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_concepts.html) in the *Amazon CloudWatch User Guide*\. 

For a further description of CloudWatch metrics for Amazon Redshift, see the following sections\.

**Topics**
+ [Amazon Redshift Metrics](#redshift-metrics)
+ [Dimensions for Amazon Redshift Metrics](#metrics-filterable-dimensions)
+ [Amazon Redshift Query and Load Performance Data](#custom-metrics-listing)

## Amazon Redshift Metrics<a name="redshift-metrics"></a>

The `AWS/Redshift` namespace includes the following metrics\.


**Title**  

| Metric | Description | 
| --- | --- | 
| ConcurrencyScalingActiveClusters |  The number of concurrency scaling clusters that are actively processing queries at any given time\. Units: Count Dimensions: `ClusterIdentifier`  | 
| ConcurrencyScalingSeconds |  The number of seconds used by concurrency scaling clusters that have active query processing activity\. Units: Sum Dimensions: `ClusterIdentifier`  | 
| CPUUtilization |  The percentage of CPU utilization\. For clusters, this metric represents an aggregation of all nodes \(leader and compute\) CPU utilization values\. Units: Percent Dimensions: `NodeID`, `ClusterIdentifier`  | 
| DatabaseConnections |  The number of database connections to a cluster\. Units: Count Dimensions: `ClusterIdentifier`  | 
| HealthStatus |  Indicates the health of the cluster\. Every minute the cluster connects to its database and performs a simple query\. If it is able to perform this operation successfully, the cluster is considered healthy\. Otherwise, the cluster is unhealthy\. An unhealthy status can occur when the cluster database is under extremely heavy load or if there is a configuration problem with a database on the cluster\.   In Amazon CloudWatch, this metric is reported as 1 or 0 whereas in the Amazon Redshift console, this metric is displayed with the words `HEALTHY` or `UNHEALTHY` for convenience\. When this metric is displayed in the Amazon Redshift console, sampling averages are ignored and only `HEALTHY` or `UNHEALTHY` are displayed\. In Amazon CloudWatch, values different than 1 and 0 might occur because of sampling issue\. Any value below 1 for `HealthStatus` is reported as 0 \(`UNHEALTHY`\)\.  Units: Count \(1/0\) \(`HEALTHY`/`UNHEALTHY` in the Amazon Redshift console\) Dimensions: `ClusterIdentifier`  | 
| MaintenanceMode |  Indicates whether the cluster is in maintenance mode\.  In Amazon CloudWatch, this metric is reported as 1 or 0 whereas in the Amazon Redshift console, this metric is displayed with the words `ON` or `OFF` for convenience\. When this metric is displayed in the Amazon Redshift console, sampling averages are ignored and only `ON` or `OFF` are displayed\. In Amazon CloudWatch, values different than 1 and 0 might occur because of sampling issues\. Any value greater than 0 for `MaintenanceMode` is reported as 1 \(`ON`\)\.  Units: Count \(1/0\) \(`ON`/`OFF` in the Amazon Redshift console\)\. Dimensions: `ClusterIdentifier`  | 
| MaxConfiguredConcurrencyScalingClusters |  Maximum number of concurrency scaling clusters configured from the parameter group\. For more information, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\.  Units: Count Dimensions: `ClusterIdentifier`  | 
| NetworkReceiveThroughput |  The rate at which the node or cluster receives data\. Units: Bytes/Second \(MB/s in the Amazon Redshift console\) Dimensions: `NodeID`, `ClusterIdentifier`  | 
| NetworkTransmitThroughput |  The rate at which the node or cluster writes data\. Units: Bytes/Second \(MB/s in the Amazon Redshift console\) Dimensions: `NodeID`, `ClusterIdentifier`  | 
| PercentageDiskSpaceUsed |  The percent of disk space used\. Units: Percent Dimensions: `NodeID`, `ClusterIdentifier`  | 
| QueriesCompletedPerSecond | The average number of queries completed per second\. Reported in five\-minute intervals\. Units: Count/Second Dimensions: latency | 
| QueryDuration | The average amount of time to complete a query\. Reported in five\-minute intervals\. Units: Microseconds Dimensions: latency | 
| ReadIOPS |  The average number of disk read operations per second\. Units: Count/Second Dimensions: `NodeID`, `ClusterIdentifier`  | 
| ReadLatency |  The average amount of time taken for disk read I/O operations\. Units: Seconds Dimensions: `NodeID`  | 
| ReadThroughput |  The average number of bytes read from disk per second\. Units: Bytes \(GB/s in the Amazon Redshift console\) Dimensions: `NodeID`  | 
| TotalTableCount |  The number of user tables open at a particular point in time\. This total does not include Spectrum tables\. Units: Count Dimensions: `ClusterIdentifier`  | 
| WLMQueueLength |  The number of queries waiting to enter a workload management \(WLM\) queue\. Units: Count Dimensions: `service class`  | 
| WLMQueueWaitTime |  The total time queries spent waiting in the workload management \(WLM\) queue\. Units: Milliseconds\. Dimensions: `ClusterIdentifier`, `wlmid`, `QueryPriority`  | 
| WLMQueriesCompletedPerSecond |  The average number of queries completed per second for a workload management \(WLM\) queue\. Reported in five\-minute intervals\. Units: Count/Second Dimensions: `wlmid`  | 
| WLMQueryDuration |  The average length of time to complete a query for a workload management \(WLM\) queue\. Reported in five\-minute intervals\. Units: Microseconds Dimensions: `wlmid`  | 
| WLMRunningQueries |  The number of queries running from both the main cluster and Concurrency Scaling cluster per WLM queue\. Units: Count Dimensions: `ClusterIdentifier`, `wlmid`  | 
| WriteIOPS |  The average number of write operations per second\. Units: Count/Second Dimensions: `NodeID`, `ClusterIdentifier`  | 
| WriteLatency |  The average amount of time taken for disk write I/O operations\. Units: Seconds Dimensions: `NodeID`  | 
| WriteThroughput |  The average number of bytes written to disk per second\. Units: Bytes \(GB/s in the Amazon Redshift console\) Dimensions: `NodeID`  | 

## Dimensions for Amazon Redshift Metrics<a name="metrics-filterable-dimensions"></a>

Amazon Redshift data can be filtered along any of the dimensions in the table following\.


|  Dimension  |  Description  | 
| --- | --- | 
|  latency  |  Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)  | 
|  NodeID  |  Filters requested data that is specific to the nodes of a cluster\. `NodeID` is either "Leader", "Shared", or "Compute\-N" where N is 0, 1, \.\.\. for the number of nodes in the cluster\. "Shared" means that the cluster has only one node, that is the leader node and compute node are combined\. Metrics are reported for the leader node and compute nodes only for `CPUUtilization`, `NetworkTransmitThroughput`, and `ReadIOPS`\. Other metrics that use the `NodeId` dimension are reported only for compute nodes\.  | 
|  ClusterIdentifier  |  Filters requested data that is specific to the cluster\. Metrics that are specific to clusters include `HealthStatus`, `MaintenanceMode`, and `DatabaseConnections`\. General metrics for this dimension \(for example, `ReadIOPS`\) that are also metrics of nodes represent an aggregate of the node metric data\. Take care in interpreting these metrics because they aggregate behavior of leader and compute nodes\.  | 
|  service class  |  The identifier for a `WLM` service class\.  | 
|  stage  |  The execution stages for a query\. The possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)  | 
|  wlmid  |  The identifier for a workload management queue\.  | 
|  QueryPriority  |  The priority of the query\. Possible values are `CRITICAL`, `HIGHEST`, `HIGH`, `NORMAL`, `LOW`, and `LOWEST`\.  | 

## Amazon Redshift Query and Load Performance Data<a name="custom-metrics-listing"></a>

In addition to the CloudWatch metrics, Amazon Redshift provides query and load performance data\. Query and load performance data can be used to help you understand the relation between database performance and cluster metrics\. For example, if you notice that a cluster's CPU spiked, you can find the spike on the cluster CPU graph and see the queries that were running at that time\. Conversely, if you are reviewing a specific query, metric data \(like CPU\) is displayed in context so that you can understand the query's impact on cluster metrics\.

Query and load performance data are not published as CloudWatch metrics and can only be viewed in the Amazon Redshift console\. Query and load performance data are generated from querying with your database's system tables \(for more information, see [System Tables Reference](https://docs.aws.amazon.com/redshift/latest/dg/cm_chap_system-tables.html) in the *Amazon Redshift Developer Guide*\)\. You can also generate your own custom database performance queries, but we recommend starting with the query and load performance data presented in the console\. For more information about measuring and monitoring your database performance yourself, see [Managing Performance](https://docs.aws.amazon.com/redshift/latest/dg/c-optimizing-query-performance.html) in the *Amazon Redshift Developer Guide\.*

The following table describes different aspects of query and load data you can access in the Amazon Redshift console\. 


| Query/Load Data | Description | 
| --- | --- | 
| Query summary |  A list of queries in a specified time period\. The list can be sorted on values such as query ID, query run time, and status\. Access this data in the **Queries** tab of the cluster detail page\.  | 
| Query Detail |  Provides details on a particular query including: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)  | 
| Load Summary |  Lists all the loads in a specified time period\. The list can be sorted on values such as query ID, query run time, and status\. Access this data in the **Loads** tab of the cluster detail page\. Access this data in the **Queries** tab of the cluster detail page\.  | 
| Load Detail |  Provides details on a particular load operation including:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/metrics-listing.html)  | 