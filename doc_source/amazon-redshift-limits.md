# Limits in Amazon Redshift<a name="amazon-redshift-limits"></a>

## Quotas and Limits<a name="amazon-redshift-limits-quota"></a>

Amazon Redshift has quotas that limit the total number of nodes that you can provision, and the number of snapshots that you can create; these quotas are per AWS account per region\. Amazon Redshift has a default quota for each of these, which are listed at [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift)\. If you attempt to exceed any of these quotas, the attempt will fail\. To increase these Amazon Redshift quota limits for your account in a region, request a change by submitting an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

Amazon Redshift Spectrum has the following quotas when using the Athena data catalog:

+ A maximum of 100 databases per account\.

+ A maximum of 100 tables per database\.

+ A maximum of 20,000 partitions per table\.

You can request a limit increase by contacting AWS Support\.

These limits don’t apply to a Hive metastore\.

In addition to quotas, Amazon Redshift has limits for the following per\-cluster values\. These limits cannot be increased:

+ The number of nodes that you can allocate per cluster, which is based on the cluster's node type\. This limit is separate from the limit for your AWS account per region\. For more information about the current node limits for each node type, see [Clusters and Nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\. 

+ The maximum number of tables is 9,900 for large and xlarge cluster node types and 20,000 for 8xlarge cluster node types\. The limit includes temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views are not included in this limit\. For more information about creating a table, see [Create Table Usage Notes](http://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_usage.html) in the *Amazon Redshift Database Developer Guide*\. For more information about cluster node types, see [Clusters and Nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.

+ The number of user\-defined databases you can create per cluster is 60\. For more information about creating a database, see [Create Database](http://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_DATABASE.html) in the *Amazon Redshift Database Developer Guide*\.

+ The number of schemas you can create per cluster is 9,900\. For more information about creating a schema, see [Create Schema](http://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_SCHEMA.html) in the *Amazon Redshift Database Developer Guide*\.

+ The number of concurrent user connections that can be made to a cluster is 500\. For more information, see [Connecting to a Cluster](connecting-to-cluster.md) in the *Amazon Redshift Cluster Management Guide*\.

+ A workload management \(WLM\) configuration can define a total concurrency level of 50 for all user\-defined queues\. For more information, see [Defining Query Queues](http://docs.aws.amazon.com/redshift/latest/dg/cm-c-defining-query-queues.html) in the *Amazon Redshift Database Developer Guide*\.

+ The number of AWS accounts you can authorize to restore a snapshot is 20 for each snapshot and 100 for each AWS Key Management Service \(AWS KMS\) key\. That is, if you have 10 snapshots that are encrypted with a single KMS key, then you can authorize 10 AWS accounts to restore each snapshot, or other combinations that add up to 100 accounts and do not exceed 20 accounts for each snapshot\. For more information, see [Sharing Snapshots](working-with-snapshots.md#working-with-snapshot-share-snapshot) in the *Amazon Redshift Cluster Management Guide*\.

+ The maximum size of a single row loaded by using the COPY command is 4 MB\. For more information, see [COPY](http://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) in the *Amazon Redshift Database Developer Guide*\. 

+ A maximum of 10 IAM roles can be associated with a cluster to authorize Amazon Redshift to access other AWS services on behalf of the user that owns the cluster and IAM role\. For more information, see [Authorizing Amazon Redshift to Access Other AWS Services on Your Behalf](authorizing-redshift-service.md)\.

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