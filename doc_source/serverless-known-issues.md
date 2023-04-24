# Considerations when using Amazon Redshift Serverless<a name="serverless-known-issues"></a>

For a list of AWS Regions where the Amazon Redshift Serverless is available, see the endpoints listed for [Redshift Serverless API](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\.

Some resources used by Amazon Redshift Serverless are subject to quotas\. For more information, see [Quotas for Amazon Redshift Serverless objects](amazon-redshift-limits.md#serverless-limits-account)\. 

When you DECLARE a cursor, the result\-set size specifications for Amazon Redshift Serverless is specified in [DECLARE](https://docs.aws.amazon.com/redshift/latest/dg/declare.html)\.

*Maintenance window* – There is no maintenance window with Amazon Redshift Serverless\. Software version updates are automatically applied\. There's no interruption for existing connection or query execution when Amazon Redshift switches versions\. New connections will always connect and work with Amazon Redshift Serverless instantly\.

*Availability Zone IDs* – When you configure your Amazon Redshift Serverless instance, open **Additional considerations**, and make sure that the subnet IDs provided in **Subnet** contain at least three of the supported Availability Zone IDs\. To see the subnet to Availability Zone ID mapping, go to the VPC console and choose **Subnets** to see the list of subnet IDs with their Availability Zone IDs\. Verify that your subnet is mapped to a supported Availability Zone ID\. To create a subnet, see [Create a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\. 

*Three subnets* – You must have at least three subnets, and they must span across three Availability Zones\. For example, you might use three subnets that map to the Availability Zones us\-east\-1a, us\-east\-1b, and us\-east\-1c\. An exception to this is the US West \(N\. California\) Region\. It requires three subnets, in the same manner as the other regions, but these must span across only two Availability Zones\. A condition is that one of the Availability Zones spanned must contain two of the subnets\.

*Free IP address requirements* – You must have free IP addresses available when creating an Amazon Redshift Serverless workgroup\. The minimum number of required IP addresses scales higher as the number of Base Redshift Processing Units \(RPUs\) for your workgroup increases\. You must have the minimum number of IP addresses available for each subnet in each workgroup that you want to create\. For more information on allocating IP addresses, see [IP addressing](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html#vpc-ip-addressing) in the *Amazon VPC User Guide*\.

The number of minimum free IP addresses required when creating a workgroup is are as follows: 


**Number of free IP addresses required when creating a subnet**  

| Redshift Processing Units \(RPUs\) | Free IP addresses required | Minimum CIDR size | 
| --- | --- | --- | 
| 32 | 13 | /27 | 
| 64 | 21 | /27 | 
| 128 | 37 | /26 | 
| 256 | 69 | /25 | 
| 512 | 133 | /24 | 

You also need free IP addresses when updating your workgroup to use more RPUs\. The number of minimum free IP addresses required when updating a workgroup is as follows: 


**Number of free IP addresses required when updating a subnet**  

| Redshift Processing Units \(RPUs\) | Updated Redshift Processing Units \(RPUs\) | Free IP addresses required | 
| --- | --- | --- | 
| 32 | 64 | 16 | 
| 64 | 128 | 28 | 
| 128 | 256 | 52 | 
| 256 | 512 | 100 | 

*Storage space after migration* – When migrating small Amazon Redshift provisioned clusters to Amazon Redshift Serverless, you might see an increase in storage\-space allocation after migration\. This is a result of optimized storage\-space allocation, resulting in preallocated storage space\. This space is used over a period of time as data grows in Amazon Redshift Serverless\.

*Datasharing between Amazon Redshift Serverless and Amazon Redshift provisioned clusters * – When datasharing where Amazon Redshift Serverless is the producer and a provisioned cluster is the consumer, the provisioned cluster must have a cluster version later than 1\.0\.38214\. If you use a cluster version earlier than this, an error occurs when you run a query\. You can view the cluster version on the Amazon Redshift console on the **Maintenance ** tab\. You can also run `SELECT version();`\.

*Max query execution time * – Elapsed execution time for a query, in seconds\. Execution time doesn't include time spent waiting in a queue\. If a query exceeds the set execution time, Amazon Redshift Serverless stops the query\. Valid values are 0–86,399\.

*Migrating for tables with interleaved sort keys * – When migrating Amazon Redshift provisioned clusters to Amazon Redshift Serverless, Redshift converts tables with interleaved sort keys and DISTSTYLE KEY to compound sort keys\. The DISTSTYLE doesn't change\. For more information on distribution styles, see [Working with data distribution styles](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html) in the Amazon Redshift Developer Guide\. For more information on sort keys, see [Working with sort keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)\. 

## Preview when using Amazon Redshift Serverless<a name="serverless-preview"></a>

You can create an Amazon Redshift Serverless workgroup in **Preview** to test new features of Amazon Redshift Serverless\. You can't use those features in production or move your **Preview** workgroup to a production workgroup\. For preview terms and conditions, see *Beta and Previews* in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.

**To create a workgroup in **Preview****

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Severless dashboard**, and choose **Workgroup configuration**\. The workgroups for your account in the current AWS Region are listed\. A subset of properties of each workgroup is displayed in columns in the list\.

1. A banner displays on the **Workgroups** list page that introduces preview\. Choose the button **Create preview workgroup** to open the create workgroup page\.

1. Enter properties for your workgroup\. We recommend entering a name for the workgroup that indicates that it is in preview\. Choose options for your workgroup, including options labeled as **\-preview**, for the features you want to test\. Continue through the pages to enter options for your workgroup and namespace\. For general information about creating workgroups, see [Creating a workgroup with a namespace](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-console-workgroups-create-workgroup-wizard.html) in the *Amazon Redshift Management Guide*\.

1. Choose **Create preview workgroup** to create a workgroup in preview\.

1. When your preview workgroup is available, use your SQL client to load and query data\.

The following features are currently available in preview workgroups:
+ SUPER data type larger than 1 MB – see [Limitations](https://docs.aws.amazon.com/redshift/latest/dg/limitations-super.html) in the *Amazon Redshift Database Developer Guide*\.
+ Auto\-copy – see [Continuous file ingestion from Amazon S3](https://docs.aws.amazon.com/redshift/latest/dg/loading-data-copy-job.html) in the *Amazon Redshift Database Developer Guide*\.
+ Query Data Catalog – see [Querying the AWS Glue Data Catalog \(preview\)](query-editor-v2-glue.md)\.

For information about preview in provisioned clusters, see [Preview features when using Amazon Redshift clusters](working-with-clusters.md#cluster-preview)\.