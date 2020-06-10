# Viewing database performance data<a name="performance-metrics-database-performance"></a>

You can use database performance metrics in Amazon Redshift to do the following:
+ Analyze the time spent by queries by processing stages\. You can look for unusual trends in the amount of time spent in a stage\. 
+ Analyze the number of queries, duration, and throughput of queries by duration ranges \(short, medium, long\)\. 
+ Look for trends in the about of query wait time by query priority \(Lowest, Low, Normal, High, Highest, Critical\)\. 
+ Look for trends in the query duration, throughput, or wait time by WLM queue\. 

**To display database performance data**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **to **, **Backup**, **Properties**, and **Schedule** tabs\. 

1. Choose the **Query monitoring** tab for metrics about your queries\.

1. In the **Query monitoring** section, choose **Database performance** tab\. 

   Using controls on the window, you can toggle between **Cluster metrics** and **WLM queue metrics**\. 

   When you choose **Cluster metrics**, the tab includes the following graphs: 
   + **Workload execution breakdown** – The time used in query processing stages\. 
   + **Queries by duration range** – The number of short, medium, and long queries\. 
   + **Query throughput** – The average number of queries completed per second\. 
   + **Query duration** – The average amount of time to complete a query\. 
   + **Average queue wait time by priority** – The total time queries spent waiting in the WLM queue by query priority\. 

   When you choose **WLM queue metrics**, the tab includes the following graphs: 
   + **Query duration by queue** – The average query duration by WLM queue\. 
   + **Query throughput by queue** – The average number of queries completed per second by WLM queue\. 
   + **Query wait time by queue** – The average duration of queries spent waiting by WLM queue\. 

## Database performance graphs<a name="performance-metrics-database-performance-examples"></a>

The following examples show graphs that are displayed in the new Amazon Redshift console\. 
+ **Workload execution breakdown**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-workload-execution-breakdown.png)
+ **Queries by duration range**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-queries-by-duration.png)
+ **Query throughput**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-query-throughput.png)
+ **Query duration**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-query-duration.png)
+ **Average queue wait time by priority**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-queue-wait-by-priority.png)
+ **Query duration by queue**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-query-duration-by-queue.png)
+ **Query throughput by queue**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-query-throughput-by-queue.png)
+ **Query wait time by queue**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/database-performance-queue-wait-by-queue.png)