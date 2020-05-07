# Quotas and limits in Amazon Redshift<a name="amazon-redshift-limits"></a>

## Amazon Redshift quotas<a name="amazon-redshift-limits-quota"></a>

Amazon Redshift has quotas that limit the use of several resources in your AWS account per AWS Region\. There is a default value for each quota and some quotas are adjustable\. For adjustable quotas, you can request an increase for your AWS account in an AWS Region by submitting an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 


| Quota name | AWS default value | Adjustable | Description | 
| --- | --- | --- | --- | 
| AWS accounts you can authorize to restore a snapshot per snapshot | 20 | No | The maximum number of AWS accounts that you can authorize to restore a snapshot, per snapshot\.  | 
| AWS accounts you can authorize to restore a snapshot per AWS KMS key | 100 | No | The maximum number of AWS accounts that you can authorize to restore a snapshot, per AWS KMS key\. That is, if you have 10 snapshots that are encrypted with a single KMS key, then you can authorize 10 AWS accounts to restore each snapshot, or other combinations that add up to 100 accounts and do not exceed 20 accounts for each snapshot\.  | 
| Cluster IAM roles for Amazon Redshift to access other AWS services | 10 | No | The maximum number of IAM roles that you can associate with a cluster to authorize Amazon Redshift to access other AWS services for the user that owns the cluster and IAM roles\.  | 
| Concurrency level \(query slots\) for all user\-defined manual WLM queues | 50 | No | The maximum query slots for all user\-defined queues defined by manual workload management\.  | 
| Concurrency scaling clusters | 10 | Yes | The maximum number of concurrency scaling clusters\.  | 
| DC2 nodes in a cluster | 128 | Yes | The maximum number of DC2 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.  | 
| DS2 nodes in a cluster | 128 | Yes | The maximum number of DS2 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.  | 
| Event subscriptions | 20 | Yes | The maximum number of event subscriptions for this account in the current AWS Region\.  | 
| Nodes | 200 | Yes | The maximum number of nodes across all database instances for this account in the current AWS Region\.  | 
| Parameter groups | 20 | No | The maximum number of parameter groups for this account in the current AWS Region\.  | 
| RA3 nodes in a cluster | 128 | Yes | The maximum number of RA3 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\. | 
| Reserved nodes | 200 | Yes | The maximum number of reserved nodes for this account in the current AWS Region\.  | 
| Schemas in each database per cluster | 9,900 | No | The maximum number of schemas that you can create in each database, per cluster\. However, `pg_temp_*` schemas do not count towards this quota\. | 
| Security groups | 20 | Yes | The maximum number of security groups for this account in the current AWS Region\.  | 
| Single row size when loading by COPY | 4 | No | The maximum size \(in MB\) of a single row when loading by using the COPY command\.  | 
| Snapshots | 20 | Yes | The maximum number of user snapshots for this account in the current AWS Region\.  | 
| Subnet groups | 20 | Yes | The maximum number of subnet groups for this account in the current AWS Region\.  | 
| Subnets in a subnet group | 20 | Yes | The maximum number of subnets for a subnet group\.  | 
| Tables for `large` cluster node type | 9,900 | No | The maximum number of tables for the large cluster node type\. This limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views aren't included in this limit\.  | 
| Tables for `xlarge` cluster node type | 9,900 | No | The maximum number of tables for the `xlarge` cluster node type\. This limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views aren't included in this limit\.  | 
| Tables for `4xlarge` cluster node type | 20,000 | No | The maximum number of tables for the 4xlarge cluster node type\.  | 
| Tables for `8xlarge` cluster node type | 20,000 | No | The maximum number of tables for the 8xlarge cluster node type\. This limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views aren't included in this limit\.  | 
| Tables for `16xlarge` cluster node type | 20,000 | No | The maximum number of tables for the 16xlarge cluster node type\. This limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views aren't included in this limit\.  | 
| User\-defined databases in a cluster | 60 | No | The maximum number of user\-defined databases that you can create per cluster\.  | 

## Amazon Redshift Spectrum quotas and limits<a name="amazon-redshift-limits-spectrum"></a>

Amazon Redshift Spectrum has the following quotas and limits: 
+ The maximum number of databases per AWS account when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of tables per database when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of partitions per table when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of partitions per AWS account when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of columns for external tables when using an AWS Glue Data Catalog, 1,598 when pseudocolumns are enabled, and 1,600 when pseudocolumns aren't enabled\. 
+ The maximum size of a string value in an ION or JSON file when using an AWS Glue Data Catalog is 16 KB\. 
+ You can add a maximum of 100 partitions using a single ALTER TABLE statement\.
+ All S3 data must be located in the same AWS Region as the Amazon Redshift cluster\. 
+ Timestamps in ION and JSON must use [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) format\. 
+ External compression of ORC files is not supported\. 
+ Text, OpenCSV, and Regex SERDEs do not support octal delimiters larger than '\\177'\. 
+ You must specify a predicate on the partition column to avoid reads from all partitions\. 

  For example, the following predicate filters on the column `ship_dtm`, but doesn't apply the filter to the partition column `ship_yyyymm`:

  `WHERE ship_dtm > '2018-04-01'`\.

  To skip unneeded partitions you need to add a predicate `WHERE ship_yymmm = '201804'`\. This predicate limits read operations to the partition `\ship_yyyymm=201804\`\.

These limits don't apply to an Apache Hive metastore\.

## Naming constraints<a name="amazon-redshift-limits-naming"></a>

 The following table describes naming constraints within Amazon Redshift\. 


|  |  | 
| --- |--- |
| Cluster identifier |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Database name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Master user name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Master password  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Parameter group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Cluster security group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Subnet group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Cluster snapshot identifier  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 