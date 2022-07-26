# Billing for Amazon Redshift Serverless<a name="serverless-billing"></a>

## Understanding Amazon Redshift Serverless billing<a name="serverless_billing"></a>

### Billing for compute capacity<a name="serverless-rpu-billing"></a>

**Base capacity and its affect on billing**

When queries run, you're billed according to the capacity used in a given duration, in RPU hours on a per\-second basis\. When no queries are running, you aren't billed for compute capacity\. You are also charged for Redshift managed storage, based on the amount of data stored\. You can set the **Base capacity** when you create your workgroup\. You can adjust the base capacity higher or lower for an existing workgroup to meet the price/performance requirements of your workload at a workgroup level\. As the number of queries increase, Amazon Redshift Serverless scales automatically to provide consistent performance\. You can change the base capacity using the console by selecting the workgroup from **Workgroup configuration** and choosing the **Limits** tab\.

**Maximum RPU hours**

To keep costs predictable for Amazon Redshift Serverless, you can set the **Maximum RPU hours** used per day, per week, or per month\. You can set this using the console, or with the API\. When a limit is reached, you can specify to write a log entry to a system table, or receive an alert, or turn off user queries\. Setting the maximum RPU hours helps keep your cost under control\. Settings for maximum RPU hours apply to your workgroup for both queries that access data in your data warehouse and queries that access external data, such as in an external table in Amazon S3\.

Setting the maximum RPU hours for the workgroup doesn't limit the performance\. You can adjust the setting at any time without an interruption to query processing\. 

Setting the base capacity and maximum RPU hours can help you meet your price/performance requirements while maintaining predictable costs\. For more information about the base capacity setting, see [Understanding Amazon Redshift Serverless capacity](serverless-capacity.md#serverless-rpu-capacity)\. For more information about serverless billing, see [Amazon Redshift pricing](http://aws.amazon.com/redshift/pricing/)\.

Another way to keep the cost for Amazon Redshift Serverless predictable is to use AWS [Cost Anomaly Detection](https://aws.amazon.com/aws-cost-management/aws-cost-anomaly-detection/) to reduce surprises in billing and provide more control\.

#### Illustrating compute cost billing scenario<a name="serverless-billing-scenarios"></a>

**A long running job**

 The following is a sample scenario, for illustrative purposes, without consideration of minimum billing requirements: You run a data\-processing job every hour between 7:00am and 7:00pm on your Amazon Redshift data warehouse in the US East \(N\. Virginia\) Region\. Assume that each time the job runs, it takes 10 minutes and 30 seconds to complete, which doesn't change\. And assume Amazon Redshift runs at 128 RPU capacity during the job\. The following results show the day's total usage and cost: 
+ **Query duration** \- The job runs 13 times between 7:00am\-7:00pm, with each run taking 10 minutes and 30 seconds\. This adds up to 8190 seconds\.
+ **Capacity used** \- 128 RPUs
+ **Daily charges** \- $109\.20 \(\(8190 seconds x 128 RPU \* $0\.375 per RPU\-hour for the Region\) / 3600 seconds in an hour\)

For the most up\-to\-date pricing information, see [Amazon Redshift pricing](http://aws.amazon.com/redshift/pricing/)\.

#### Monitoring usage and cost<a name="serverless-billing-visualizing"></a>

There are several ways you can estimate usage and billing for Amazon Redshift Serverless\. System views can be helpful because the system metadata, including query and usage data, is timely and you don't have to do any setup to query it\. CloudWatch can also be useful for monitoring usage for your Amazon Redshift Serverless instance, and has additional features to provide insights and set actions\.

##### Visualizing usage by querying a system view<a name="serverless-billing-visualizing-sysview"></a>

You can query the `sys_serverless_usage` system table to track usage\. Query it to retrieve an approximation of the duration that queries were processed:

```
select
  trunc(start_time) "Day",
  sum(compute_seconds)/60/60 * <Price for 1 RPU>
from sys_serverless_usage
group by trunc(start_time)
order by 1
```

This query approximates the cost per day incurred for Amazon Redshift Serverless\. Variations like how Amazon Redshift Serverless reports to AWS billing, differences in aggregation frequency, and rounding can affect the billing\. It may differ slightly from the results from `sys_serverless_usage`\. Additionally, `sys_serverless_usage` shows your usage in near real time\. The billing report is created after queries complete\. There may be usage logged in `sys_serverless_usage` that isn't reflected in the billing report\. Thus, the information in `sys_serverless_usage`  should be used to approximate to your end\-of\-month billing\. It won't match exactly\. For more information about monitoring tables and views, see [Monitoring queries and workloads with Amazon Redshift Serverless](serverless-monitoring.md)\.

##### Visualizing usage with CloudWatch<a name="serverless-billing-visualizing-cw"></a>

 You can use the metrics available in CloudWatch to track usage\. The metrics generated for CloudWatch are `ComputeSeconds`, indicating the total RPU seconds used in the current minute and `ComputeCapacity`, indicating the total compute capacity for that minute\. Usage metrics can also be found on the Redshift console on the Redshift **Serverless dashboard**\. For more information about CloudWatch, see [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) 

### Billing for storage<a name="serverless-storage-billing"></a>

Primary storage capacity is billed as Redshift Managed Storage \(RMS\)\. Storage is billed by GB / month\. Storage billing is separate from billing for compute resources\. Storage used for user snapshots is billed at the standard backup billing rates, depending on your usage tier\.

Data transfer costs and machine learning \(ML\) costs apply separately, the same as provisioned clusters\. Snapshot replication and data sharing across AWS Regions are billed at the transfer rates outlined on the pricing page\. For more information, see [Amazon Redshift pricing](http://aws.amazon.com/redshift/pricing/)\.

#### Visualizing billing usage with CloudWatch<a name="db-serverless-billing-storage-cw"></a>

The metric `SnapshotStorage`, which tracks snapshot storage usage, is generated and sent to CloudWatch\. For more information about CloudWatch, see [What is Amazon CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)

### Amazon Redshift Serverless free trial<a name="db-serverless-billing-free-trial"></a>

Amazon Redshift Serverless offers a free trial\. If you participate in the free trial, you can view the free trial credit balance in the Redshift console, and check free trial usage in the [SYS\_SERVERLESS\_USAGE](https://docs.aws.amazon.com/redshift/latest/dg/SYS_SERVERLESS_USAGE.html) system view\. Note that billing details for free trial usage does not appear in the billing console\. You can only view usage in the billing console after the free trial ends\.

### Billing usage notes<a name="db-serverless-billing-details"></a>
+ **Recording usage** \- A query or transaction is only metered and recorded after the transaction completes, is rolled back, or stopped\. For instance, if a transaction runs for two days, RPU usage is recorded after it completes\. You can monitor ongoing use in real time by querying `sys_serverless_usage`\. Transaction recording may reflect as RPU usage variation and affect costs for specific hours and for daily use\.
+ **Writing explicit transactions** \- It's important as a best practice to end transactions\. If you don't end or roll back an open transaction, Amazon Redshift Serverless continues to use RPUs\. For example, if you write an explicit `BEGIN TRAN`, it's important to have corresponding `COMMIT` and `ROLLBACK` statements\.
+ **Cancelled queries** \- If you run a query and cancel it before it finishes, you are still billed for the time the query ran\. 
+ **Scaling** \- The Amazon Redshift Serverless instance may initiate scaling for handling periods of higher load, in order to maintain consistent performance\. Your Amazon Redshift Serverless billing includes both base compute and scaled capacity at the same RPU rate\.
+ **Scaling down** \- Amazon Redshift Serverless scales up from its base RPU capacity to handle periods of higher load\. It some cases, RPU capacity can remain at a higher setting for a period after query load falls\. We recommend that you set maximun RPU hours in the console to guard against unexpected cost\. For more information, see [Billing for Amazon Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-billing.html)\.
+ **System tables** \- When you query a system table, the query time is billed\. But automatic optimizations, such as automatic sort and vacuum, are not charged for\.
+ **Redshift Spectrum** \- When you have Amazon Redshift Serverless, and you run queries, there isn't a separate charge for data\-lake queries\. For queries on data stored in Amazon S3, the charge is the same, by transaction time, as queries on local data\.
+ **Federated queries** \- Federated queries are charged in terms of RPUs used over a specific time interval, in the same manner as queries on the data warehouse or data lake\.
+ **Storage** \- Storage is billed separately, by GB / month\.
+ **Minimum charge** \- The minimum charge is for 60 seconds, metered on a per\-second basis\.
+ **Snapshot billing** \- Snapshot billing doesn't change\. It's charged according to storage, billed at a rate of GB / month\. You can restore your data warehouse to specific points in the last 24 hours at a 30 minute granularity, free of charge\. For more information, see [Amazon Redshift pricing](http://aws.amazon.com/redshift/pricing/)\.

#### Amazon Redshift Serverless best practices for keeping billing predictable<a name="db-serverless-billing-session-timeout"></a>

There are a few best practices to follow, and built\-in settings that help keep your billing consistent\.

As mentioned previously in this topic, make sure to end each transaction\. When you use `BEGIN` to start a transaction, it's important to `END` it as well\. And use best\-practice error handling to respond gracefully to errors and end each transaction\. Minimizing open transactions helps to avoid unnecessary RPU use\.

`SESSION TIMEOUT` helps by ending open transactions and idle sessions\. It causes any session kept open for a default of 3600 seconds \(one hour\) to time out\. This timeout setting can be changed explicitly for a specific user, such as when you want to keep a session open for a long\-running query\. The topic [CREATE USER](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_USER.html) shows how to adjust `SESSION TIMEOUT` for a user\.

In most cases, we recommend that you don't extend the `SESSION TIMEOUT` value, unless you have a use case that requires it specifically\. If the session remains idle, with an open transaction, it can result in a case where RPUs are used until the session is closed\. This will result in unnecessary cost\.

Amazon Redshift Serverless has a maximum period of four hours for a running query before it times out\. It also has a maximum value of six hours for an idle transaction before it times out\. For more information, see [Quotas and limits for Amazon Redshift Serverless](serverless-limits.md)\.