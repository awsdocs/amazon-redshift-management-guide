# Viewing Query Details<a name="performance-metrics-query-execution-details"></a>

You can view details for a particular query by clicking an individual query in the table on the **Queries** page to open the **Query *ID*** view\. The following list describes the information available for individual queries:
+ **Query Properties**\. Displays a summary of information about the query such as the query ID, the database user who ran the query, and the duration\.
+ **Details**\. Displays the status of the query\.
+ **SQL**\. Displays the query text in a friendly, human\-readable format\.
+ **Query Execution Details**\. Displays information about how the query was processed\. This section includes both planned and actual execution data for the query\. For information on using the **Query Execution Details** section, see [Analyzing Query Execution](analyzing-query-execution.md)\.
+ **Cluster Performance During Query Execution**\. Displays performance metrics from Amazon CloudWatch\. For information on using the **Cluster Performance During Query Execution** section, see [Viewing Cluster Performance During Query Execution](performance-metrics-query-cluster.md)\.

The **Query** view looks similar to the following when you open it\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cm-metrics-query-id-page.png)