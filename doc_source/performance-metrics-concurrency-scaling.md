# Viewing Concurrency Scaling Data<a name="performance-metrics-concurrency-scaling"></a>

By using concurrency scaling metrics in Amazon Redshift, you can do the following:
+ View concurrency scaling activity in concurrency scaling clusters\. This can tell you if concurrency scaling is limited by the `max_concurrency_scaling_clusters`\. If so, you can choose to increase the `max_concurrency_scaling_clusters` in the DB parameter\.
+ View the total usage of concurrency scaling summed across all concurrency scaling clusters\.

**To view concurrency scaling data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster for which you want to view concurrency scaling data\.

1. Choose the **Database Performance** tab\.

   By default, the performance view displays concurrency scaling data over the past hour\. If you need to fine\-tune the view, you have *filters* that you can use to change the scope of the data\.

## Concurrency Scaling Graphs<a name="performance-metrics-concurrency-scaling-examples"></a>

The Amazon Redshift console displays the following graphs\.
+ **Concurrency Scaling Activity** – Shows the number of active concurrency scaling clusters for the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/concurrency-scaling-activity-graph.png)
+ **Queued vs\. Running Queries** – Compares queued and running queries of concurrency scaling clusters configured during the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/queued-running-configured-concurrency-scaling-clusters-graph.png)
+ **Concurrency Scaling Usage in Minutes \(Sum\)** – Shows the total usage of concurrency scaling clusters for the chosen time range\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/concurrency-scaling-usage-minutes-graph.png)