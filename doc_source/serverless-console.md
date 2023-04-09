# Amazon Redshift Serverless console<a name="serverless-console"></a>

To learn how to get started with the Amazon Redshift Serverless console, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/eq4o26Hpuac/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/eq4o26Hpuac)

## Serverless dashboard<a name="serverless-console-dashboard"></a>

On the **Serverless dashboard** page, you can view a summary of your resources and graphs of your usage\.
+ **Namespace overview** – This section shows the amount of snapshots and datashares within your namespace\.
+ **Workgroups** – This section shows all of the workgroups within Amazon Redshift Serverless\.
+ **Queries metrics** – This section shows query activity for the last one hour\. 
+ **RPU capacity used** – This section shows capacity used for the last one hour\. 
+ **Free trial** – This section shows the free trial credits remaining in your AWS account\. This covers all usage of Amazon Redshift Serverless resources and operations, including snapshots, storage, workgroup, and so on, under the same account\.
+ **Alarms** – This section shows the alarms you configured in Amazon Redshift Serverless\.

## Data backup<a name="serverless-console-data-backup"></a>

On the **Data backup** tab you can work with the following:
+ **Snapshots** – You can create, delete, and manage snapshots of your Amazon Redshift Serverless data\. The default retention period is `indefinitely`, but you can configure the retention period to be any value between 1 and 3653 days\. You can authorize AWS accounts to restore namespaces from a snapshot\.
+ **Recovery points** – Displays the recovery points that are automatically created so you can recover from an accidental write or delete within the last 24 hours\. To recover data, you can restore a recovery point to any available namespace\. You can create a snapshot from a recovery point if you want to keep a point of recovery for a longer time period\. The default retention period is `indefinitely`, but you can configure the retention period to be any value between 1 and 3653 days\.

## Data access<a name="serverless-console-data-access"></a>

On the **Data access** tab you can work with the following:
+ **Network and security** settings – You can view VPC\-related values, AWS KMS encryption values, and audit logging values\. You can update only audit logging\. For more information on setting network and security settings using the console, see [Managing usage limits, query limits, and other administrative tasks](serverless-console-configuration.md)\.
+ **AWS KMS key** – The AWS KMS key used to encrypt resources in Amazon Redshift Serverless\. 
+ **Permissions** – You can manage the IAM roles that Amazon Redshift Serverless can assume to use resources on your behalf\. For more information, see [Identity and access management in Amazon Redshift Serverless](serverless-security.md#serverless-iam)\.
+ **Redshift\-managed VPC endpoints** – You can access your Amazon Redshift Serverless instance from another VPC or subnet\. For more information, see [Connecting to Amazon Redshift Serverless from a Redshift managed VPC endpoint](serverless-connecting.md#database-connect-from-managed-vpc-endpoint)\.

## Limits<a name="serverless-console-limits"></a>

On the **Limits** tab, you can work with the following:
+ **Base capacity in Redshift processing units \(RPUs\)** settings – You can set the base capacity used to process your workload\. To improve query performance, increase your RPU value\. 
+ **Usage limits** – The maximum compute resources that your Amazon Redshift Serverless instance can use in a time period before an action is initiated\. You limit the amount of resource Amazon Redshift Serverless uses to run your workload\. Usage is measured in Redshift Processing Unit \(RPU\) hours\. An RPU hour is the number of RPUs used in an hour\. You determine an action when a threshold that you set is reached, as follows: 
  + Send an alert\.
  + Log an entry to a system table\.
  + Turn off user queries\.
+  **Query limits** – You can add a limit to monitor performance and limits\. For more information about query monitoring limits, see [WLM query monitoring rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\. 

For more information, see [Understanding Amazon Redshift Serverless capacity](serverless-capacity.md#serverless-rpu-capacity)\.

## Datashares<a name="serverless-console-datashares"></a>

On the **Datashares** tab you can work with the following:
+ **Datashares created in my namespace** settings – You can create a datashare and share it with other namespaces and AWS accounts\. 
+ **Datashares from other namespaces and AWS accounts** – You can create a database from a datashare from other namespace and AWS accounts\. 

For more information about data sharing, see [Data sharing in Amazon Redshift Serverless](serverless-datasharing.md)\.

## Query and database monitoring<a name="serverless-console-database-monitoring"></a>

On the **Query and database monitoring** page, you can view graphs of your **Query history** and **Database performance**\.

On the **Query history** tab, you see the following graphs \(you can choose between **Query list** and **Resource metrics**\):
+ **Query runtime** – This graph shows which queries are running in the same timeframe\. Choose a bar in the graph to view more query execution details\. 
+ **Queries and loads** – This section lists queries and loads by **Query ID**\. 
+ **RPU capacity used** – This graph shows overall capacity in Redshift Processing Units \(RPUs\)\. 
+ **Database connections** – This graph shows the number of active database connections\. 

## Database performance<a name="serverless-console-database-performance"></a>

On the **Database performance** tab, you see the following graphs:
+ **Queries completed per second** – This graph shows the average number of queries completed per second\. 
+ **Queries duration** – This graph shows the average amount of time to complete a query\. 
+ **Database connections** – This graph shows the number of active database connections\. 
+ **Running queries** – This graph shows the total number of running queries at a given time\. 
+ **Queued queries** – This graph shows the total number of queries queued at a given time\. 
+ **Query run time breakdown** – This graph shows the total time queries spent running by query type\. 

## Resource monitoring<a name="serverless-console-resource-monitoring"></a>

On the **Resource monitoring** page, you can view graphs of your consumed resources\. You can filter the data based on several facets\.
+ **Metric filter** – You can use metric filters to select filters for a specific workgroup, as well as choose the time range and time interval\.
+ **RPU capacity used** – This graph shows the overall capacity in Redshift processing units \(RPUs\)\. 
+ **Compute usage** – This graph shows the accumulative usage of Amazon Redshift Serverless by period for the selected time range\. 

On the **Datashares** page, you can manage datashares **In my account** and **From other accounts**\. For more information about data sharing, see [Data sharing in Amazon Redshift Serverless](serverless-datasharing.md)\.