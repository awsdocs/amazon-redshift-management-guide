# Amazon Redshift management overview<a name="overview"></a>

The Amazon Redshift service manages all of the work of setting up, operating, and scaling a data warehouse\. These tasks include provisioning capacity, monitoring and backing up the cluster, and applying patches and upgrades to the Amazon Redshift engine\.

## Cluster management<a name="rs-overview-cluster-management"></a>

An Amazon Redshift cluster is a set of nodes, which consists of a leader node and one or more compute nodes\. The type and number of compute nodes that you need depends on the size of your data, the number of queries you will execute, and the query execution performance that you need\.

### Creating and managing clusters<a name="rs-overview-create-and-manage-clusters"></a>

Depending on your data warehousing needs, you can start with a small, single\-node cluster and easily scale up to a larger, multi\-node cluster as your requirements change\. You can add or remove compute nodes to the cluster without any interruption to the service\. For more information, see [Amazon Redshift clusters](working-with-clusters.md)\.

### Reserving compute nodes<a name="rs-overview-reserve-compute-nodes"></a>

If you intend to keep your cluster running for a year or longer, you can save money by reserving compute nodes for a one\-year or three\-year period\. Reserving compute nodes offers significant savings compared to the hourly rates that you pay when you provision compute nodes on demand\. For more information, see [Purchasing Amazon Redshift reserved nodes](purchase-reserved-node-instance.md)\.

### Creating cluster snapshots<a name="rs-overview-create-cluster-snapshots"></a>

Snapshots are point\-in\-time backups of a cluster\. There are two types of snapshots: automated and manual\. Amazon Redshift stores these snapshots internally in Amazon Simple Storage Service \(Amazon S3\) by using an encrypted Secure Sockets Layer \(SSL\) connection\. If you need to restore from a snapshot, Amazon Redshift creates a new cluster and imports data from the snapshot that you specify\. For more information about snapshots, see [Amazon Redshift snapshots](working-with-snapshots.md)\.

## Cluster access and security<a name="rs-overview-cluster-access-and-security"></a>

There are several features related to cluster access and security in Amazon Redshift\. These features help you to control access to your cluster, define connectivity rules, and encrypt data and connections\. These features are in addition to features related to database access and security in Amazon Redshift\. For more information about database security, see [Managing Database Security](https://docs.aws.amazon.com/redshift/latest/dg/r_Database_objects.html) in the *Amazon Redshift Database Developer Guide*\.

### AWS accounts and IAM credentials<a name="rs-overview-aws-accounts-and-iam-credentials"></a>

By default, an Amazon Redshift cluster is only accessible to the AWS account that creates the cluster\. The cluster is locked down so that no one else has access\. Within your AWS account, you use the AWS Identity and Access Management \(IAM\) service to create user accounts and manage permissions for those accounts to control cluster operations\. For more information, see [Security in Amazon Redshift](iam-redshift-user-mgmt.md)\.

### Security groups<a name="rs-overview-security-groups"></a>

By default, any cluster that you create is closed to everyone\. IAM credentials only control access to the Amazon Redshift API\-related resources: the Amazon Redshift console, command line interface \(CLI\), API, and SDK\. To enable access to the cluster from SQL client tools via JDBC or ODBC, you use security groups: 
+ If you are using the EC2\-VPC platform for your Amazon Redshift cluster, you must use VPC security groups\. We recommend that you launch your cluster in an EC2\-VPC platform\.

  You cannot move a cluster to a VPC after it has been launched with EC2\-Classic\. However, you can restore an EC2\-Classic snapshot to an EC2\-VPC cluster using the Amazon Redshift console\. For more information, see [Restoring a cluster from a snapshot](managing-snapshots-console.md#snapshot-restore)\.
+ If you are using the EC2\-Classic platform for your Amazon Redshift cluster, you must use Amazon Redshift security groups\.

In either case, you add rules to the security group to grant explicit inbound access to a specific range of CIDR/IP addresses or to an Amazon Elastic Compute Cloud \(Amazon EC2\) security group if your SQL client runs on an Amazon EC2 instance\. For more information, see [Amazon Redshift cluster security groups](working-with-security-groups.md)\.

In addition to the inbound access rules, you create database users to provide credentials to authenticate to the database within the cluster itself\. For more information, see [Databases](#rs-overview-databases) in this topic\.

### Encryption<a name="rs-overview-encryption"></a>

When you provision the cluster, you can optionally choose to encrypt the cluster for additional security\. When you enable encryption, Amazon Redshift stores all data in user\-created tables in an encrypted format\. You can use AWS Key Management Service \(AWS KMS\) to manage your Amazon Redshift encryption keys\. 

Encryption is an immutable property of the cluster\. The only way to switch from an encrypted cluster to a cluster that is not encrypted is to unload the data and reload it into a new cluster\. Encryption applies to the cluster and any backups\. When you restore a cluster from an encrypted snapshot, the new cluster is encrypted as well\.

For more information about encryption, keys, and hardware security modules, see [Amazon Redshift database encryption](working-with-db-encryption.md)\.

### SSL connections<a name="rs-overview-ssl-connections"></a>

You can use Secure Sockets Layer \(SSL\) encryption to encrypt the connection between your SQL client and your cluster\. For more information, see [Configuring security options for connections](connecting-ssl-support.md)\.

## Monitoring clusters<a name="rs-overview-monitoring-clusters"></a>

There are several features related to monitoring in Amazon Redshift\. You can use database audit logging to generate activity logs, configure events and notification subscriptions to track information of interest,\. Use the metrics in Amazon Redshift and Amazon CloudWatch to learn about the health and performance of your clusters and databases\.

### Database audit logging<a name="rs-overview-database-audit-logging"></a>

You can use the database audit logging feature to track information about authentication attempts, connections, disconnections, changes to database user definitions, and queries run in the database\. This information is useful for security and troubleshooting purposes in Amazon Redshift\. The logs are stored in Amazon S3 buckets\. For more information, see [Database audit logging](db-auditing.md)\.

### Events and notifications<a name="rs-overview-events-and-notifications"></a>

Amazon Redshift tracks events and retains information about them for a period of several weeks in your AWS account\. For each event, Amazon Redshift reports information such as the date the event occurred, a description, the event source \(for example, a cluster, a parameter group, or a snapshot\), and the source ID\. You can create Amazon Redshift event notification subscriptions that specify a set of event filters\. When an event occurs that matches the filter criteria, Amazon Redshift uses Amazon Simple Notification Service to actively inform you that the event has occurred\. For more information about events and notifications, see [Amazon Redshift events](working-with-events.md)\.

### Performance<a name="rs-overview-performance"></a>

Amazon Redshift provides performance metrics and data so that you can track the health and performance of your clusters and databases\. Amazon Redshift uses Amazon CloudWatch metrics to monitor the physical aspects of the cluster, such as CPU utilization, latency, and throughput\. Amazon Redshift also provides query and load performance data to help you monitor the database activity in your cluster\. For more information about performance metrics and monitoring, see [Monitoring Amazon Redshift cluster performance](metrics.md)\.

## Databases<a name="rs-overview-databases"></a>

Amazon Redshift creates one database when you provision a cluster\. This is the database you use to load data and run queries on your data\. You can create additional databases as needed by running a SQL command\. For more information about creating additional databases, go to [Step 1: Create a database](https://docs.aws.amazon.com/redshift/latest/dg/t_creating_database.html) in the *Amazon Redshift Database Developer Guide*\.

When you provision a cluster, you specify a master user who has access to all of the databases that are created within the cluster\. This master user is a superuser who is the only user with access to the database initially, though this user can create additional superusers and users\. For more information, go to [Superusers](https://docs.aws.amazon.com/redshift/latest/dg/r_superusers.html) and [Users](https://docs.aws.amazon.com/redshift/latest/dg/r_Users.html) in the *Amazon Redshift Database Developer Guide*\.

Amazon Redshift uses parameter groups to define the behavior of all databases in a cluster, such as date presentation style and floating\-point precision\. If you don’t specify a parameter group when you provision your cluster, Amazon Redshift associates a default parameter group with the cluster\. For more information, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\.

For more information about databases in Amazon Redshift, go to the [Amazon Redshift Database Developer Guide](https://docs.aws.amazon.com/redshift/latest/dg/)\.