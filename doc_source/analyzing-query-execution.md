# Analyzing Query Execution<a name="analyzing-query-execution"></a>

The **Query Execution Details** section of the **Query** view provides information about the way the query was processed\. This section combines data from [SVL\_QUERY\_REPORT](http://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_REPORT.html), [STL\_EXPLAIN](http://docs.aws.amazon.com/redshift/latest/dg/r_STL_EXPLAIN.html), and other system views and tables\.

The **Query Execution Details** section has two tabs:

+ **Plan**\. This tab shows the explain plan for the query that is displayed\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-plan.png)

+ **Actual**\. This tab shows the actual steps and statistics for the query that was executed\. This information displays in a textual hierarchy and a visual chart\. You can hover your cursor over any bar in the chart to see the **Avg** and **Max** statistics for the related step, as shown following\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-actual.png)

  The **Avg** statistic shows the average execution time for the step across data slices, and the percentage of the total query runtime that represents\. The **Max** statistic shows the longest execution time for the step on any of the data slices, and the *skew*\. The skew is the difference between the average and maximum execution times for the step\. You might want to investigate a step if the maximum execution time is consistently more than twice the average execution time over multiple runs of the query, and if the step also takes a significant amount of time \(for example, being one of the top three steps in execution time in a large query\)\.
**Note**  
When possible, you should run a query twice to see what its execution details will typically be\. Compilation adds overhead to the first run of the query that is not present in subsequent runs\. 

  To investigate high skew for a step, check the query plan for distribution steps to see what type of distribution is being performed in the query, then review your data distribution strategy to see if should be modified\. For more information about Amazon Redshift data distribution, go to [Choosing a Data Distribution Style](http://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html) in the *Amazon Redshift Database Developer Guide*\. 

  You can click any bar in the chart to compare the data estimated from the explain plan with the actual performance of the query, as shown following\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-step.png)

  If the query optimizer posted alerts for the query in the [STL\_ALERT\_EVENT\_LOG](http://docs.aws.amazon.com/redshift/latest/dg/r_STL_ALERT_EVENT_LOG.html) system table, then the plan nodes associated with the alerts are flagged with an alert icon\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-exec-details-actual-alert.png)

The information on the **Plan** tab is analogous to running the EXPLAIN command in the database\. The EXPLAIN command examines your query text, and returns the query plan\. You use this information to evaluate queries, and revise them for efficiency and performance if necessary\. The EXPLAIN command doesn’t actually run the query\.

The following example shows a query that returns the top five sellers in San Diego, based on the number of tickets sold in 2008, and the query plan for that query\.

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

For more information about understanding the explain plan, go to [Analyzing the Explain Plan](http://docs.aws.amazon.com/redshift/latest/dg/c-query-planning.html) in the *Amazon Redshift Database Developer Guide*\.

When you actually run the query \(omitting the EXPLAIN command\), the engine might find ways to optimize the query performance and change the way it processes the query\. The actual performance data for the query is stored in the system views, such as [SVL\_QUERY\_REPORT](http://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_REPORT.html) and [SVL\_QUERY\_SUMMARY](http://docs.aws.amazon.com/redshift/latest/dg/r_SVL_QUERY_SUMMARY.html)\.

The Amazon Redshift console uses a combination of [STL\_EXPLAIN](http://docs.aws.amazon.com/redshift/latest/dg/r_STL_EXPLAIN.html), SVL\_QUERY\_REPORT, and other system views and tables to present the actual query performance and compare it to the explain plan for the query\. This information appears on the **Actual** tab\. If you see that the explain plan and the actual query execution steps differ, you might need to perform some operations in the database, such as [ANALYZE](http://docs.aws.amazon.com/redshift/latest/dg/r_ANALYZE.html), to update statistics and make the explain plan more effective\.

Additionally, sometimes the query optimizer breaks complex SQL queries into parts and creates temporary tables with the naming convention volt\_tt\_*guid* to process the query more efficiently\. In this case, both the explain plan and the actual query execution summary apply to the last statement that was run\. You can review previous query IDs to see the explain plan and actual query execution summary for each of the corresponding parts of the query\.

For more information about the difference between the explain plan and system views and logs, go to [Analyzing the Query Summary](http://docs.aws.amazon.com/redshift/latest/dg/c-analyzing-the-query-summary.html) in the *Amazon Redshift Database Developer Guide*\.

## Viewing Query Execution Details Using the Console<a name="performance-metrics-viewing-query-execution-details"></a>

Use the following procedure to look at the details of query execution\.

**To view query execution details**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation pane, click **Clusters**\.

1. In the **Cluster** list, select the cluster for which you want to view query execution details\.

1. Click the **Queries** tab, and open the query for which you want to view performance data\.

1. Expand the **Query Execution Details** section and do the following:

   1. On the **Plan** tab, review the explain plan for the query\. If you find that your explain plan differs from the actual query execution on the **Actual** tab, you might need to run ANALYZE to update statistics or perform other maintenance on the database to optimize the queries you run\. For more information about query optimization, see [Tuning Query Performance](http://docs.aws.amazon.com/redshift/latest/dg/c-optimizing-query-performance.html) in the *Amazon Redshift Database Developer Guide*\.

   1. On the **Actual** tab, review the performance data associated with each of the plan nodes in the query execution\. You can click an individual plan node in the hierarchy to view performance data associated with that specific plan node\. This data will include both the estimated and actual performance data\.