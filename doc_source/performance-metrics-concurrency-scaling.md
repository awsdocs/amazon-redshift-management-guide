# Viewing concurrency scaling data<a name="performance-metrics-concurrency-scaling"></a>

By using concurrency scaling metrics in Amazon Redshift, you can do the following:
+ View concurrency scaling activity in concurrency scaling clusters\. This can tell you if concurrency scaling is limited by the `max_concurrency_scaling_clusters`\. If so, you can choose to increase the `max_concurrency_scaling_clusters` in the DB parameter\.
+ View the total usage of concurrency scaling summed across all concurrency scaling clusters\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="metrics-concurrency-scaling"></a>

**To display concurrency scaling data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **to **, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Query monitoring** tab for metrics about your queries\.

1. In the **Query monitoring** section, choose **WORKLOAD EXECUTION BREAKDOWN**\. 

   If concurrency scaling is active, the graph includes the following: 
   + **Concurrency scaling minutes**
   + **Concurrency scaled** number of clusters 

## Original console<a name="metrics-concurrency-scaling-originalconsole"></a>

**To view concurrency scaling data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster for which you want to view concurrency scaling data\.

1. Choose the **Database Performance** tab\.

   By default, the performance view displays concurrency scaling data over the past hour\. If you need to fine\-tune the view, you have *filters* that you can use to change the scope of the data\.

## Concurrency scaling graphs<a name="performance-metrics-concurrency-scaling-examples"></a>

The following example graphs are displayed in the Amazon Redshift console\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="performance-metrics-concurrency-scaling-graphs"></a>
+ **Concurrency scaling minutes** – Shows the total usage of concurrency scaling clusters for the chosen time range\. 
+ **Concurrency scaled** – Shows the number of concurrency scaling clusters for the chosen time range\.

### Original console<a name="performance-metrics-concurrency-scaling-graphs-originalconsole"></a>
+ **Concurrency Scaling Activity** – Shows the number of active concurrency scaling clusters for the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/concurrency-scaling-activity-graph.png)
+ **Queued vs\. Running Queries** – Compares queued and running queries of concurrency scaling clusters configured during the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/queued-running-configured-concurrency-scaling-clusters-graph.png)
+ **Concurrency Scaling Usage in Minutes \(Sum\)** – Shows the total usage of concurrency scaling clusters for the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/concurrency-scaling-usage-minutes-graph.png)