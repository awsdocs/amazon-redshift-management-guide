# Analyzing query execution<a name="analyzing-query-execution"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="metric-queries-execution-details"></a>

**To analyze a query**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **QUERIES**, and then choose **Queries and loads** to display the list of queries for your account\. You might need to change settings on this page to find your query\. 

1. Choose the **Query** identifier in the list to display **Query details**\. 

   The **Query details** page includes **Query details** and **Query plan** tabs with metrics about the query\. 
**Note**  
You can also navigate to the **Query details** page from a **Cluster details** page, **Query history** tab when you drill down into a query in a **Query runtime** graph\. 

The **Query details** page contains the following sections:
+ A list of **Rewritten queries**, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-rewritten-queries.png)
+ A **Query details** section, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-query.png)
+ A **Query details** tab that contains the **SQL** that was run and **Execution details** about the run\. 
+ A **Query plan** tab that contains the **Query plan** steps and other information about the query plan\. This table also contains graphs about the cluster when the query ran\. 
  + **Cluster health status**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-cluster-health-status.png)
  + **CPU utilization**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-cpu-utilization.png)
  + **Storage capacity used**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-storage-capacity-used.png)
  + **Active database connections**   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-details-active-database-connections.png)

## Original console<a name="metric-queries-execution-details-originalconsole"></a>

**To view query execution details**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. For **Cluster**, choose the cluster for which you want to view query execution details\.

1. Choose the **Queries** tab, and open the query for which you want to view performance data\.

1. Expand the **Query Execution Details** section and do the following:

   1. On the **Plan** tab, review the explain plan for the query\. In some cases, you might find that your explain plan differs from the actual query execution on the **Actual** tab\. In these cases, you might need to run ANALYZE to update statistics or perform other maintenance on the database to optimize the queries that you run\. For more information about query optimization, see [Tuning query performance](https://docs.aws.amazon.com/redshift/latest/dg/c-optimizing-query-performance.html) in the *Amazon Redshift Database Developer Guide*\.

   1. On the **Actual** tab, review the performance data associated with each of the plan nodes in the query execution\. You can choose an individual plan node in the hierarchy to view performance data associated with that specific plan node\. This data includes both the estimated and actual performance data\.

   1. On the **Metrics** tab, review the metrics for each of the cluster nodes\.

The **Query Execution Details** section of the **Query** view provides information about the way the query was processed\. This section combines data from [SVL\_QUERY\_REPORT](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_REPORT.html), [STL\_EXPLAIN](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_EXPLAIN.html), and other system views and tables\.

The **Query Execution Details** section has three tabs:
+ **Plan**\. This tab shows the explain plan for the query that is displayed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-plan.png)

  The information on the **Plan** tab is analogous to running the EXPLAIN command in the database\. The EXPLAIN command examines your query text, and returns the query plan\. You use this information to evaluate queries, and revise them for efficiency and performance if necessary\. The EXPLAIN command doesn't actually run the query\.

  The following example shows a query that returns the top five sellers in San Diego\. The result is based on the number of tickets sold in 2008 and the query plan for that query\.

  ```
  explain 
  select sellerid, username, (firstname ||' '|| lastname) as name,
  city, sum(qtysold)
  from sales, date, users
  where sales.sellerid = users.userid
  and sales.dateid = date.dateid
  and year = 2008
  and city = 'San Diego'
  group by sellerid, username, name, city
  order by 5 desc
  limit 5;
  ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-query-plan.png)

  For more information about understanding the explain plan, see [Analyzing the explain plan](https://docs.aws.amazon.com/redshift/latest/dg/c-query-planning.html) in the *Amazon Redshift Database Developer Guide*\.

  When you actually run the query \(omitting the EXPLAIN command\), the engine might find ways to optimize the query performance and change the way it processes the query\. The actual performance data for the query is stored in the system views, such as [SVL\_QUERY\_REPORT](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_REPORT.html) and [SVL\_QUERY\_SUMMARY](https://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_SUMMARY.html)\.

  The Amazon Redshift console uses a combination of [STL\_EXPLAIN](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_EXPLAIN.html), SVL\_QUERY\_REPORT, and other system views and tables to present the actual query performance and compare it to the explain plan for the query\. This information appears on the **Actual** tab\. In some cases, you might see that the explain plan and the actual query execution steps differ\. In these cases, you might need to perform some operations in the database, such as [ANALYZE](https://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE.html), to update statistics and make the explain plan more effective\.

  Additionally, sometimes the query optimizer breaks complex SQL queries into parts and creates temporary tables with the naming convention volt\_tt\_*guid* to process the query more efficiently\. In this case, both the explain plan and the actual query execution summary apply to the last statement that was run\. You can review previous query IDs to see the explain plan and actual query execution summary for each of the corresponding parts of the query\.

  For more information about the difference between the explain plan and system views and logs, see [Analyzing the query summary](https://docs.aws.amazon.com/redshift/latest/dg/c-analyzing-the-query-summary.html) in the *Amazon Redshift Database Developer Guide*\.
+ **Actual**\. This tab shows the actual steps and statistics for the query that was executed\. This information displays in a textual hierarchy and visual charts for **Timeline** and **Execution time**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-actual.png)

  The **Timeline** view shows the sequence in which the actual steps of the query are executed\. 

  The **Execution time** view shows the time taken for every step of the query\. It can be used to understand what steps are taking longer to complete\. 

  Each view shows **Avg time** and **Max time**\.

  The **Avg** statistic shows the average execution time for the step across data slices, and the percentage of the total query runtime that represents\. The **Max** statistic shows the longest execution time for the step on any of the data slices, and the skew\. The *skew *is the difference between the average and maximum execution times for the step\. 

  You might want to investigate a step if two conditions are both true\. One condition is that the maximum execution time is consistently more than twice the average execution time over multiple runs of the query\. The other condition is that the step also takes a significant amount of time\. An example is its being one of the top three steps in execution time in a large query\.
**Note**  
When possible, you should run a query twice to see what its execution details typically are\. Compilation adds overhead to the first run of the query that is not present in subsequent runs\. 

  You can choose any bar in the chart to compare the data estimated from the explain plan with the actual performance of the query, as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-step.png)

  If the query optimizer posted alerts for the query in the [STL\_ALERT\_EVENT\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_ALERT_EVENT_LOG.html) system table, then the plan nodes associated with the alerts are flagged with an alert icon\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-actual-alert.png)
+ **Metrics**\. This tab shows the metrics for the query that was executed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-metrics.png)

  The **Rows returned** metric is the sum of the number of rows produced during each step of the query\.

  The **Bytes returned** metric shows the number of bytes returned for each cluster node\.

  The **Execution time** metric shows the query execution time for each cluster node\.

  The **Row throughput** metric shows the number of rows returned divided by query execution time for each cluster node\.

  If a query runs slower than expected, you can use the **Metrics** tab to troubleshoot the cause\. Look at the **Row throughput** metric\. If one of the cluster nodes appears to have a much higher row throughput than the other nodes, the workload is unevenly distributed among the cluster nodes\. One possible cause is that your data is unevenly distributed, or skewed, across node slices\. For more information, see [Identifying tables with data skew or unsorted rows](https://docs.aws.amazon.com/redshift/latest/dg/diagnostic-queries-for-query-tuning.html#identify-tables-with-data-skew-or-unsorted-rows.html)\. 

  If your data is evenly distributed, your query might be filtering for rows that are located mainly on that node\. To fix this issue, look at the distribution styles for the tables in the query and see if any improvements can be made\. Remember to weigh the performance of this query against the performance of other important queries and the system overall before making any changes\. For more information, see [Choosing a data distribution style](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html)\. 

**Note**  
The metrics tab is not available for a single\-node cluster\.