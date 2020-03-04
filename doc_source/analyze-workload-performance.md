# Analyzing Workload Performance<a name="analyze-workload-performance"></a>

You can get a detailed view of your workload's performance by looking at the Workload Execution Breakdown chart in the console\. We build the chart with data provided by the CloudWatch QueryRuntimeBreakdown metric\. With this chart, you can see how much time your queries spend in the various processing stages, such as waiting and planning\. 

**Note**  
The Workload Execution Breakdown chart isn't shown for single\-node clusters\.

The following list of metrics describes the various processing stages:
+ `QueryPlanning`: Time spent parsing and optimizing SQL statements\.
+ `QueryWaiting`: Time spent waiting in the workload management \(WLM\) queue\.
+ `QueryExecutingRead`: Time spent running read queries\. 
+ `QueryExecutingInsert`: Time spent running insert queries\.
+ `QueryExecutingDelete`: Time spent running delete queries\.
+ `QueryExecutingUpdate`: Time spent running update queries\.
+ `QueryExecutingCtas`: Time spent running CREATE TABLE AS queries\.
+ `QueryExecutingUnload`: Time spent running unload queries\.
+ `QueryExecutingCopy`: Time spent running copy queries\.

For example, the following graph in the Amazon Redshift console shows the amount of time that queries have spent in the plan, wait, read, and write stages\. You can combine the findings from this graph with other metrics for further analysis\. In some cases, your graph might show that queries with a short duration \(as measured by the `QueryDuration` metric\) are spending a long time in the wait stage\. In these cases, you can increase the WLM concurrency rate for a particular queue to increase throughput\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

## New Console<a name="cluster-workload-breakdown-chart"></a>

Following, you can see a screenshot of the workload breakdown chart\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/mgmt-workloadbreakdown-v1png.png)

The y\-axis in the diagram is cumulative for all sessions running during the selected time period\. The following diagram illustrates how Amazon Redshift aggregates query processing for concurrent sessions\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/querybreakdownschematic.png)

## Original Console<a name="cluster-workload-breakdown-chart-originalconsole"></a>

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/querybreakdownsummary.png)

The y\-axis in the diagram is cumulative for all sessions running during the selected time period\. The following diagram illustrates how Amazon Redshift aggregates query processing for concurrent sessions\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/querybreakdownschematic.png)

### Example Analysis with the Workload Execution Breakdown Chart<a name="workload-execution-breakdown-example"></a>

The following diagrams illustrate how you can use the Workload Execution Breakdown chart to optimize your cluster's performance\. In the first example chart, you can see that a majority of the query time was during the `QueryWaiting` stage\. This effect was due to a low WLM concurrency value\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/querybreakdownexample.png)

The following chart illustrates the query runtime breakdown after adjusting the concurrency to a higher value\. In the updated chart, you can see that the majority of time usage has switched from the `QueryWaiting` stage to the `QueryExecutingRead` and `QueryPlanning` stages\. In this case, more overall time is spent in the planning phase because more queries are now running during the time window after adjusting concurrency\. You can check the number of queries running during a specific time period with the `WLMQueriesCompletedPerSecond` metric\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/querybreakdownexample2.png)

These charts demonstrate how changing your cluster's settings affect the amount of time that queries spend in the various stages\. In the case preceding, queries initially spent a relatively long time waiting because the concurrency setting was low\. After increasing concurrency, more queries are processed in parallel, thus decreasing the wait time and increasing query throughput\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

## New Console<a name="cluster-workload-breakdown"></a>

**To display the cluster workload breakdown chart**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **Maintenance and monitoring**, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Query monitoring** tab for metrics about your queries\.

1. In the **Query monitoring** section, choose **WORKLOAD EXECUTION BREAKDOWN**\. 

   The following metrics are graphed for the chosen time range as a stacked bar chart: 
   + **Plan** time 
   + **Wait** time 
   + **Read** time 
   + **Write** time 

## Original Console<a name="cluster-workload-breakdown-originalconsole"></a>

### Viewing the Workload Breakdown Chart<a name="access-workload-breakdown-chart"></a>

You can view the workload breakdown chart in the console\.<a name="view-workload-breakdown-chart"></a>

**To view the Workload Execution Breakdown Chart**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose **Clusters** in the navigation pane\.

1. Choose the cluster that you want to analyze\.

1. Choose the **Database Performance** tab\.