# Viewing workload concurrency and concurrency scaling data<a name="performance-metrics-concurrency-scaling"></a>

By using concurrency scaling metrics in Amazon Redshift, you can do the following:
+ Analyze whether you can reduce the number of queued queries by enabling concurrency scaling\. You can compare by WLM queue or for all WLM queues\. 
+ View concurrency scaling activity in concurrency scaling clusters\. This can tell you if concurrency scaling is limited by the `max_concurrency_scaling_clusters`\. If so, you can choose to increase the `max_concurrency_scaling_clusters` in the DB parameter\.
+ View the total usage of concurrency scaling summed across all concurrency scaling clusters\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="metrics-concurrency-scaling"></a>

**To display concurrency scaling data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **to **, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose the **Query monitoring** tab for metrics about your queries\.

1. In the **Query monitoring** section, choose **Workload concurrency** tab\. 

   The tab includes the following graphs: 
   + **Queued vs\. Running queries on the cluster** – The number of queries running \(from the main cluster and concurrency scaling cluster\) compared to the number of queries waiting in all WLM queues in the cluster\. 
   + **Queued vs\. Running queries per queue** – The number of queries running \(from the main cluster and concurrency scaling cluster\) compared to the number or queries waiting in each WLM queue\. 
   + **Concurrency scaling activity** – The number of concurrency scaling clusters that are actively processing queries\. 
   + **Concurrency scaling usage** – The usage of concurrency scaling clusters that have active query processing activity\. 

## Original console<a name="metrics-concurrency-scaling-originalconsole"></a>

**To view concurrency scaling data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster for which you want to view concurrency scaling data\.

1. Choose the **Database Performance** tab\.

   By default, the performance view displays concurrency scaling data over the past hour\. If you need to fine\-tune the view, you can use filters to change the scope of the data\.

## Workload concurrency graphs<a name="performance-metrics-concurrency-scaling-examples"></a>

The following examples show graphs that are displayed in the new Amazon Redshift console\. 
+ **Queued vs\. Running queries on the cluster**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/workload-concurrency-queued-vs-running-cluster.png)
+ **Queued vs\. Running queries per queue**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/workload-concurrency-queued-vs-running-per-queue.png)
+ **Concurrency scaling activity**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/workload-concurrency-concurrency-scaling-activity.png)
+ **Concurrency scaling usage**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/workload-concurrency-concurrency-scaling-usage.png)