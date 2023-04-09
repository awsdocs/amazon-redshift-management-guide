# Audit logging for Amazon Redshift Serverless<a name="serverless-audit-logging"></a>

## Exporting logs<a name="serverless_audit-logging"></a>

You can configure Amazon Redshift Serverless to export connection, user, and user\-activity log data to a log group in Amazon CloudWatch Logs\. With Amazon CloudWatch Logs, you can perform real\-time analysis of the log data and use CloudWatch to create alarms and view metrics\. You can use CloudWatch Logs to store your log records in durable storage\.

You can create CloudWatch alarms to track your metrics using the Amazon Redshift console\. For more information on creating alarms, see [Managing alarms](https://docs.aws.amazon.com/redshift/latest/mgmt/performance-metrics-alarms.html)\.

 To export generated log data to Amazon CloudWatch Logs, the respective logs must be selected for export in your Amazon Redshift Serverless configuration settings, on the console\. 

### Monitoring log events in CloudWatch<a name="db-auditing-manage-logs-cloudwatch-monitoring"></a>

After selecting which Redshift logs to export, you can monitor events in Amazon CloudWatch Logs\. A new log group is automatically created for Amazon Redshift Serverless, in which `log_type` represents the log type\.

```
/aws/redshift/<namespace>/<log_type>
```

When you create your first workgroup and namespace, *default* is the namespace name\. The log group name varies according to what you call the namespace\.

For example, if you export the connection log, log data is stored in the following log group\.

```
/aws/redshift/default/connectionlog
```

Log events are exported to a log group using the serverless log stream\. The behavior depends on which of the following conditions are true:
+ **A log group with the specified name exists\.** Redshift exports log data using the existing log group\. To create log groups with predefined log\-retention periods, metric filters, and customer access, you can use automated configuration, such as that provided by **AWS CloudFormation**\.
+ **A log group with the specified name doesn't exist\.** When a matching log entry is detected in the log for the instance, Amazon Redshift Serverless creates a new log group in Amazon CloudWatch Logs automatically\. The log group uses the default log\-retention period of *Never Expire*\. To change the log\-retention period, use the Amazon CloudWatch Logs console, the AWS CLI, or the Amazon CloudWatch Logs API\. For more information about changing log\-retention periods in CloudWatch Logs, see *Change log data retention* in [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html)\.

To search for information within log events, use the Amazon CloudWatch Logs console, the AWS CLI, or the Amazon CloudWatch Logs API\. For more information about searching and filtering log data, see [Searching and filtering log data](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html)\. 

### Amazon Redshift Serverless metrics<a name="db-auditing-manage-logs-cloudwatch-monitoring-metrics"></a>

Amazon Redshift Serverless metrics are divided into compute metrics and data and storage metrics, falling under the workgroup and namespace dimension sets, respectively\. For more information about workgroups and namespaces, see [ Overview of Amazon Redshift Serverless workgroups and namespaces](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-workgroups-and-namespaces.html)\.

CloudWatch compute metrics are the following:


**CloudWatch compute metrics**  

| Metric name | Units | Description | Dimension sets | 
| --- | --- | --- | --- | 
| QueriesCompletedPerSecond | Number of queries | The number of queries completed each second\. | \{Database, LatencyRange, Workgroup\}, \{LatencyRange, Workgroup\} | 
| QueryDuration | Microseconds | The average amount of time to complete a query\. | \{Database, LatencyRange, Workgroup\}, \{LatencyRange, Workgroup\} | 
| QueriesRunning | Number of queries | The number of running queries at a point in time\. | \{Database, QueryType, Workgroup\}, \{QueryType, Workgroup\} | 
| QueriesQueued | Number of queries | The number of queries in the queue at a point in time\. | \{Database, QueryType, Workgroup\}, \{QueryType, Workgroup\} | 
| DatabaseConnections | Number of connections | The number of connections to a database at a point in time\. | \{Database, Workgroup\}, \{Workgroup\} | 
| QueryRuntimeBreakdown | Milliseconds | The total time queries ran, by query stage\. | \{Database, Stage, Workgroup\}, \{Stage, Workgroup\} | 
| ComputeCapacity | RPU | Average number of compute units allocated during the past 30 minutes, rounded up to the nearest integer\. | \{Workgroup\} | 
| ComputeSeconds | RPU\-seconds | Accumulated compute\-unit seconds used in the last 30 minutes\. | \{Workgroup\} | 
| QueriesSucceeded | Number of queries | The number of queries that succeeded in the last 5 minutes\. | \{Database, QueryType, Workgroup\}, \{QueryType, Workgroup\} | 
| QueriesFailed | Number of queries | The number of queries that failed in the last 5 minutes\. | \{Database, QueryType, Workgroup\}, \{QueryType, Workgroup\} | 
| UsageLimitAvailable | RPU\-hours or TBs | Depending on the UsageType, UsageLimitAvailable returns the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html)  | \{UsageLimitId, UsageType, Workgroup\} | 
| UsageLimitConsumed | RPU\-hours or TBs | Depending on the UsageType, UsageLimitConsumed returns the following: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html)  | \{UsageLimitId, UsageType, Workgroup\} | 

CloudWatch data and storage metrics are the following:


**CloudWatch data and storage metrics**  

| Metric name | Units | Description | Dimension sets | 
| --- | --- | --- | --- | 
| TotalTableCount | Number of tables | The number of user tables existing at a point in time\. This total doesn't include Amazon Redshift Spectrum tables\. | \{Database, Namespace\} | 
| DataStorage | Megabytes | The number of megabytes used, in disk or storage space, for Redshift data\. | \{Namespace\} | 

The `SnapshotStorage` metric is namespace\- and workgroup\-agnostic\. CloudWatch's `SnapshotStorage` metric is as follows:


**CloudWatch SnapshotStorage metric**  

| Metric name | Units | Description | Dimension sets | 
| --- | --- | --- | --- | 
| SnapshotStorage | Megabytes | The number of megabytes used, in disk or storage space, for Snapshots\. | \{\} | 

Dimension sets are the grouping dimensions applied to your metrics\. You can use these dimension groups to specify how your statistics are retrieved\.

The following table details dimensions and dimension values for specific metrics:


**CloudWatch dimensions and dimension values**  

| Dimension | Description and values | 
| --- | --- | 
| DatabaseName | The name of the database\. A custom value\. | 
| Latency | Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html)  | 
| QueryType | Possible values are INSERT, DELETE, UPDATE, UNLOAD, LOAD, SELECT, CTAS, and OTHER\. | 
| stage | The execution stages for a query\. Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html) | 
| Namespace | The name of the namespace\. A custom value\. | 
| Workgroup | The name of the workgroup\. A custom value\. | 
| UsageLimitId | The identifier of the usage limit\. | 
| UsageType | The Amazon Redshift Serverless feature being limited\. Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html)  | 