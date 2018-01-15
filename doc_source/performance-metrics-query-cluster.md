# Viewing Cluster Performance During Query Execution<a name="performance-metrics-query-cluster"></a>

You can use the **Cluster Performance During Query Execution** section of the **Query** view to see cluster metrics during query execution to help identify poorly performing queries, look for bottleneck queries, and determine if you need to resize your cluster for your workload\.

**To view cluster metrics during query execution**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the left navigation pane, click **Clusters**\.

1. In the **Cluster** list, select the cluster for which you want to view cluster performance during query execution\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-10.png)

1. Click the **Queries** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-70.png)

1. In the query list, find the query you want to work with, and click the query ID in the **Query** column\.

   In the following example, the queries are sorted by **Run time** to find the query with the maximum run time\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-80.png)

1. In the **Query** page that opens, scroll to the **Cluster Performance During Query Execution** section to view cluster metrics\.

   In the following example, the `CPUUtilization` and `NetworkReceiveThroughput` metrics are displayed for the time that this query was running\.
**Tip**  
You can close the details of the **Query Execution Details** or **SQL** sections to manage how much information is displayed in the pane\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-100.png)