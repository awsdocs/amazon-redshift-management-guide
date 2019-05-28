# Limits in Amazon Redshift<a name="amazon-redshift-limits"></a>

## Quotas and Limits<a name="amazon-redshift-limits-quota"></a>

Amazon Redshift has quotas that limit the total number of nodes that you can provision, and the number of snapshots that you can create; these quotas are per AWS account per region\. Amazon Redshift has a default quota for each of these, which are listed at [AWS Service Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift)\. If you attempt to exceed any of these quotas, the attempt will fail\. To increase these Amazon Redshift quota limits for your account in a region, request a change by submitting an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

In addition to quotas, Amazon Redshift has limits for the following per\-cluster values\. These limits cannot be increased:
+ The number of nodes that you can allocate per cluster, which is based on the cluster's node type\. This limit is separate from the limit for your AWS account per region\. For more information about the current node limits for each node type, see [Clusters and Nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\. 
+ The maximum number of tables is 9,900 for large and xlarge cluster node types and 20,000 for 8xlarge cluster node types\. The limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views are not included in this limit\. For more information about creating a table, see [Create Table Usage Notes](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_usage.html) in the *Amazon Redshift Database Developer Guide*\. For more information about cluster node types, see [Clusters and Nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.
+ The number of user\-defined databases you can create per cluster is 60\. For more information about creating a database, see [Create Database](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_DATABASE.html) in the *Amazon Redshift Database Developer Guide*\.
+ The number of schemas you can create in each database in a cluster is 9,900, however, pg\_temp\_\* schemas do not count towards this quota\. For more information about creating a schema, see [Create Schema](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_SCHEMA.html) in the *Amazon Redshift Database Developer Guide*\.
+ A workload management \(WLM\) configuration can define a total concurrency level of 50 for all user\-defined queues\. For more information, see [Defining Query Queues](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-defining-query-queues.html) in the *Amazon Redshift Database Developer Guide*\.
+ The number of AWS accounts you can authorize to restore a snapshot is 20 for each snapshot and 100 for each AWS Key Management Service \(AWS KMS\) key\. That is, if you have 10 snapshots that are encrypted with a single KMS key, then you can authorize 10 AWS accounts to restore each snapshot, or other combinations that add up to 100 accounts and do not exceed 20 accounts for each snapshot\. For more information, see [Sharing Snapshots](working-with-snapshots.md#working-with-snapshot-share-snapshot) in the *Amazon Redshift Cluster Management Guide*\.
+ The maximum size of a single row loaded by using the COPY command is 4 MB\. For more information, see [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the *Amazon Redshift Database Developer Guide*\. 
+ A maximum of 10 IAM roles can be associated with a cluster to authorize Amazon Redshift to access other AWS services on behalf of the user that owns the cluster and IAM role\. For more information, see [Authorizing Amazon Redshift to Access Other AWS Services on Your Behalf](authorizing-redshift-service.md)\.

## Spectrum limits<a name="amazon-redshift-limits-spectrum"></a>

Amazon Redshift Spectrum has the following limits when using the Athena or AWS Glue data catalog:
+ A maximum of 10,000 databases per account\.
+ A maximum of 100,000 tables per database\.
+ A maximum of 1,000,000 partitions per table\.
+ A maximum of 10,000,000 partitions per account\.
+ All S3 data must be located in the same region as the Redshift cluster\. 
+ Timestamps in ION and JSON must use [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) format\. 
+ The maximum allowed size of a row object in a ION/JSON file is 8MB\. 

  Objects in ION or JSON files are the data enclosed by brackets, for example \(`\{ "col1": "data1", "col2": "data2" \}`\)\.
+ The maximum allowed size of a string value in an ION or JSON file is 16KB\. 
+ External compression of ORC files is not supported\. 
+ Text, OpenCSV and Regex SERDEs do not support octal delimiters larger than `\\177` 
+ You must specify a predicate on the partition column to avoid reads from all partitions\. 

  For example, the following predicate will filter on the column `ship_dtm` but will not apply the filter to the partition column `ship_yyyymm`:

  `WHERE ship_dtm > '2018-04-01'`\.

  To skip unneeded partitions you need to add a predicate `WHERE ship_yymmm = '201804'`\. This predicate will limit read operations to the partition `\ship_yyyymm=201804\`\.
+ The column limit for external tables is 1598 with pseudo columns enabled and 1600 without\. 

You can request a limit increase by contacting AWS Support\.

These limits don’t apply to a Hive metastore\.

## Naming Constraints<a name="amazon-redshift-limits-naming"></a>

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