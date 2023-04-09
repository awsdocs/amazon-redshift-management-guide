# Quotas and limits in Amazon Redshift<a name="amazon-redshift-limits"></a>

Amazon Redshift has quotas that limit the use of several resources in your AWS account per AWS Region\. There is a default value for each quota and some quotas are adjustable\. For adjustable quotas, you can request an increase for your AWS account in an AWS Region by submitting an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

## Quotas for Amazon Redshift objects<a name="amazon-redshift-limits-quota"></a>

Amazon Redshift has quotas that limit the use of several object types\. There is a default value for each\.


| Quota name | AWS default value | Adjustable | Description | 
| --- | --- | --- | --- | 
| AWS accounts that you can authorize to restore a snapshot per snapshot  | 20 | No | The maximum number of AWS accounts that you can authorize to restore a snapshot, per snapshot\.  | 
| AWS accounts that you can authorize to restore a snapshot per AWS KMS key | 100 | No | The maximum number of AWS accounts that you can authorize to restore a snapshot, per KMS key\. That is, if you have 10 snapshots that are encrypted with a single KMS key, then you can authorize 10 AWS accounts to restore each snapshot, or other combinations that add up to 100 accounts and do not exceed 20 accounts for each snapshot\.  | 
| Cluster IAM roles for Amazon Redshift to access other AWS services | 501 | No | The maximum number of IAM roles that you can associate with a cluster to authorize Amazon Redshift to access other AWS services for the user that owns the cluster and IAM roles\. 1The quota is 10 in the following AWS Regions: ap\-northeast\-3, af\-south\-1, eu\-south\-1, ap\-southeast\-3, us\-gov\-east\-1, us\-gov\-west\-1, us\-iso\-east\-1, us\-isob\-east\-1\.  | 
| Concurrency level \(query slots\) for all user\-defined manual WLM queues | 50 | No | The maximum query slots for all user\-defined queues defined by manual workload management\.  | 
| Concurrency scaling clusters | 10 | Yes | The maximum number of concurrency scaling clusters\.  | 
| DC2 nodes in a cluster | 128 | Yes | The maximum number of DC2 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.  | 
| DS2 nodes in a cluster | 128 | Yes | The maximum number of DS2 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.  | 
| Event subscriptions | 20 | Yes | The maximum number of event subscriptions for this account in the current AWS Region\.  | 
| Nodes | 200 | Yes | The maximum number of nodes across all database instances for this account in the current AWS Region\.  | 
| Parameter groups | 20 | No | The maximum number of parameter groups for this account in the current AWS Region\. | 
| RA3 nodes in a cluster | 128 | Yes | The maximum number of RA3 nodes that you can allocate to a cluster\. For more information about node limits for each node type, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\. | 
| Redshift\-managed VPC endpoints connected to a cluster | 30 | Yes | The maximum number of Redshift\-managed VPC endpoints that you can connect to a cluster\. For more information about Redshift\-managed VPC endpoints, see [Working with Redshift\-managed VPC endpoints in Amazon Redshift ](managing-cluster-cross-vpc.md)\. | 
| Grantees to cluster accessed through a Redshift\-managed VPC endpoint | 5 | Yes | The maximum number of grantees that a cluster owner can authorize to create a Redshift\-managed VPC endpoint for a cluster\. For more information about Redshift\-managed VPC endpoints, see [Working with Redshift\-managed VPC endpoints in Amazon Redshift ](managing-cluster-cross-vpc.md)\. | 
| Redshift\-managed VPC endpoints per authorization | 5 | Yes | The maximum number of Redshift\-managed VPC endpoints that you can create per authorization\. For more information about Redshift\-managed VPC endpoints, see [Working with Redshift\-managed VPC endpoints in Amazon Redshift ](managing-cluster-cross-vpc.md)\. | 
| Reserved nodes | 200 | Yes | The maximum number of reserved nodes for this account in the current AWS Region\. | 
| Schemas in each database per cluster | 9,900 | No | The maximum number of schemas that you can create in each database, per cluster\. However, `pg_temp_*` schemas do not count towards this quota\. | 
| Security groups | 20 | Yes | The maximum number of security groups for this account in the current AWS Region\. | 
| Single row size when loading by COPY | 4 | No | The maximum size \(in MB\) of a single row when loading by using the COPY command\. | 
| Snapshots | 20 | Yes | The maximum number of user snapshots for this account in the current AWS Region\. | 
| Subnet groups | 20 | Yes | The maximum number of subnet groups for this account in the current AWS Region\. | 
| Subnets in a subnet group | 20 | Yes | The maximum number of subnets for a subnet group\.  | 
| Tables for `large` cluster node type | 9,900 | No | The maximum number of tables for the large cluster node type\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `xlarge` cluster node type | 9,900 | No | The maximum number of tables for the `xlarge` cluster node type\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `xlplus` cluster node type with a single\-node cluster\. | 9,900 | No | The maximum number of tables for the `xlplus` cluster node type with a single\-node cluster\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `xlplus` cluster node type with a multiple\-node cluster\. | 20,000 | No | The maximum number of tables for the `xlplus` cluster node type with a multiple\-node cluster\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `4xlarge` cluster node type | 200,000 | No | The maximum number of tables for the `4xlarge` cluster node type\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `8xlarge` cluster node type | 200,000 | No | The maximum number of tables for the `8xlarge` cluster node type\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| Tables for `16xlarge` cluster node type | 200,000 | No | The maximum number of tables for the `16xlarge` cluster node type\. This limit includes permanent tables, temporary tables, datashare tables, and materialized views\. External tables are counted as temporary tables\. Temporary tables include user\-defined temporary tables and temporary tables created by Amazon Redshift during query processing or system maintenance\. Views and system tables aren't included in this limit\.  | 
| User\-defined databases in a cluster | 60 | No | The maximum number of user\-defined databases that you can create per cluster\. | 
| Timeout for idle or inactive sessions | 4 hours | No | This setting applies to the cluster\. For information about setting the idle\-session timeout value for a user, see [ ALTER USER](https://docs.aws.amazon.com/redshift/latest/dg/r_ALTER_USER.html) in the *Amazon Redshift Database Developer Guide*\. The user setting takes precedence over the cluster setting\. | 
| Timeout for idle transactions | 6 hours | No | The maximum period of inactivity for an open transaction before Amazon Redshift ends the session associated with the transaction\. This setting takes precedence over any user\-defined idle timeout setting\. It applies to the cluster\. | 
| Stored procedures in a database | 10,000 | No | The maximum number of stored procedures\. See [Limits and differences for stored procedure support](https://docs.aws.amazon.com/redshift/latest/dg/stored-procedure-constraints.html) for more limits\. | 
| Maximum number of connections | 2,000 | No | The maximum number of connections to an RA3 cluster\. \(This applies specifically to the ra3\.xlplus, ra3\.4xlarge and ra3\.16xlarge node types\.\) The maximum connections allowed varies by node type\. | 

## Quotas for Amazon Redshift Serverless objects<a name="serverless-limits-account"></a>

Amazon Redshift has quotas that limit the use of several object types in your Amazon Redshift Serverless instance\. There is a default value for each\.


| Quota name | AWS default value | Adjustable | Description | 
| --- | --- | --- | --- | 
| Number of databases | 100 | No | The maximum allowed count of databases in an Amazon Redshift Serverless instance\. | 
| Number of schemas | 9,900 | No | The maximum allowed count of schemas in an Amazon Redshift Serverless instance\. | 
| Number of tables | 200,000 | No | The maximum allowed count of tables in an Amazon Redshift Serverless instance\. | 
| Timeout for idle or inactive sessions | 4 hours | No | For information about setting the idle\-session timeout value for a user, see [ ALTER USER](https://docs.aws.amazon.com/redshift/latest/dg/r_ALTER_USER.html) in the *Amazon Redshift Database Developer Guide*\. The user setting takes precedence\. | 
| Timeout for a running query | 86,399 seconds \(24 hours\) | No | The maximum time for a running query before Amazon Redshift ends it\. | 
| Timeout for idle transactions | 6 hours | No | The maximum period of inactivity for an open transaction before Amazon Redshift Serverless ends the session associated with the transaction\. This setting takes precedence over any user\-defined idle timeout setting\. | 
| Number of maximum connections | 2000 | No | The maximum number of connections allowed to connect to a workgroup\. | 
| Number of workgroups | 25 | Yes | The number of workgroups supported\. | 
| Number of namespaces | 25 | Yes | The number of namespaces supported\. | 

For more information about how Amazon Redshift Serverless billing is affected by timeout configuration, see [Billing for Amazon Redshift Serverless](serverless-billing.md)\.

## Quotas for query editor v2 objects<a name="query-editor-limits-account"></a>

Amazon Redshift has quotas that limit the use of several object types in your Amazon Redshift query editor v2\. There is a default value for each\.


| Quota name | AWS default value | Adjustable | Description | 
| --- | --- | --- | --- | 
| Connections | 500 | Yes | Maximum number of connections that you can create using the query editor v2 in this account in the current Region\.  | 
| Saved queries | 2,500 | Yes | Maximum number of saved queries that you can create using the query editor v2 in this account in the current Region\.  | 
| Query versions | 20 | Yes | Maximum number of versions per query that you can create using the query editor v2 in this account in the current Region\.  | 
| Saved charts | 500 | Yes | Maximum number of saved charts that you can create using the query editor v2 in this account in the current Region\.  | 
| Rows fetched per query | 100,000 | Yes | Maximum number of rows fetched per query by the query editor v2 in this account in the current Region\.  | 
| Data fetched size per query | 5 | Yes | Maximum size, in megabytes, of the data fetched per query by the query editor v2 in this account in the current Region\.  | 
| Simultaneous socket connections per principal | 10 | Yes | Maximum number of simultaneous socket connections to query editor v2 that a single principal can establish in the current Region\. Evaluate whether to increase this quota if you receive errors that your socket connections are over the limit\. | 
| Simultaneous socket connections per account | 250 | Yes | Maximum number of simultaneous socket connections to query editor v2 that all principals in the account can establish in the current Region\. Evaluate whether to increase this quota if you receive errors that your socket connections are over the limit\. | 
| Maximum concurrent connections | 3 | No | Maximum database connections per user \(includes isolated sessions\)\. This value can be set from 1–10 by the query editor v2 administrator in **Account settings**\. If you reach the limit set by your administrator, consider using shared sessions instead of isolated sessions when running your SQL\. For more information about connections, see [Opening query editor v2](query-editor-v2-using.md#query-editor-v2-open)\. For more information about setting the limit, see [Changing account settings](query-editor-v2-using.md#query-editor-v2-settings)\. | 

## Quotas and limits for Amazon Redshift Spectrum objects<a name="amazon-redshift-limits-spectrum"></a>

Amazon Redshift Spectrum has the following quotas and limits: 
+ The maximum number of databases per AWS account when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of tables per database when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of partitions per table when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of partitions per AWS account when using an AWS Glue Data Catalog\. For this value, see [AWS Glue service quotas](https://docs.aws.amazon.com/general/latest/gr/glue.html#limits_glue) in the *Amazon Web Services General Reference*\. 
+ The maximum number of columns for external tables when using an AWS Glue Data Catalog, 1,597 when pseudocolumns are enabled, and 1,600 when pseudocolumns aren't enabled\. 
+ The maximum size of a string value in an ION or JSON file when using an AWS Glue Data Catalog is 16 KB\. The string can be truncated if you reach this limit\.
+ You can add a maximum of 100 partitions using a single ALTER TABLE statement\.
+ All S3 data must be located in the same AWS Region as the Amazon Redshift cluster\. 
+ Timestamps in ION and JSON must use [ISO8601](https://www.iso.org/iso-8601-date-and-time-format.html) format\. 
+ External compression of ORC files is not supported\. 
+ Text, OpenCSV, and Regex SERDEs do not support octal delimiters larger than '\\177'\. 
+ You must specify a predicate on the partition column to avoid reads from all partitions\. 

  For example, the following predicate filters on the column `ship_dtm`, but doesn't apply the filter to the partition column `ship_yyyymm`:

  `WHERE ship_dtm > '2018-04-01'`\.

  To skip unneeded partitions you need to add a predicate `WHERE ship_yyyymm = '201804'`\. This predicate limits read operations to the partition `\ship_yyyymm=201804\`\.

These limits don't apply to an Apache Hive metastore\.

## Naming constraints<a name="amazon-redshift-limits-naming"></a>

 The following table describes naming constraints within Amazon Redshift\. 


|  |  | 
| --- |--- |
| Cluster identifier |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Database name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Endpoint name of a Redshift\-managed VPC endpoint  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Admin user name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Admin password  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Parameter group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Cluster security group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Subnet group name  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 
|  Cluster snapshot identifier  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-limits.html)  | 