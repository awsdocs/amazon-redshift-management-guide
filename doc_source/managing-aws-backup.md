# Working with AWS Backup<a name="managing-aws-backup"></a>

AWS Backup is a fully managed service that helps you centralize and automate data protection across AWS services, in the cloud, and on premises\. 

Using AWS Backup for Amazon Redshift, you can configure data protection policies and monitor activity for different Amazon Redshift resources in one place\. You can also create and store snapshots on Amazon Redshift provisioned clusters\. This lets you automate and consolidate backup tasks that you had to do separately before, without any manual processes\.

A backup, or *recovery point*, represents the content of a resource, such as an Amazon Redshift cluster, at a specified time\. A backup generally refers to the different backups in AWS services, such as Amazon Redshift snapshots\. AWS Backup saves backups in backup vaults, which you can organize according to your business needs\. The terms *recovery point* and *backup* are used interchangeably\. For more information about AWS Backup, see [Working with backups](https://docs.aws.amazon.com/aws-backup/latest/devguide/recovery-points.html)\.

Amazon Redshift is natively integrated with AWS Backup\. That lets you define your backup plans and assign Amazon Redshift resources to the backup plans\. AWS Backup automates the creation of Amazon Redshift manual snapshots, and securely stores these snapshots in an encrypted backup vault that you designate in your backup plan\. For information about vaults, see [Working with backup vaults](https://docs.aws.amazon.com/aws-backup/latest/devguide/vaults.html)\. In the backup plan, you can define the backup frequency, backup window, lifecycle, or backup vault\. For information about backup plans, see [Managing backups using backup plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/about-backup-plans.html)\.

**Topics**
+ [Considerations for using AWS Backup with Amazon Redshift](#managing-aws-backup-considerations)
+ [Managing AWS Backup with Amazon Redshift](managing-aws-backup-overview.md)

## Considerations for using AWS Backup with Amazon Redshift<a name="managing-aws-backup-considerations"></a>

The following sections describe considerations and limitations for using AWS Backup with Amazon Redshift\.

### Considerations for using AWS Backup with Amazon Redshift<a name="managing-aws-backup-considerations"></a>

Following are considerations for using AWS Backup with Amazon Redshift:
+ AWS Backup for Amazon Redshift is available where both AWS Backup and Amazon Redshift are available in the same AWS Regions\. For information on where AWS Backup is available, see [Feature availability by AWS Regions](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html#features-by-region)\.
+ To get started using AWS Backup, verify that you have met all the prerequisites\. For more information, see [Prerequisites](https://docs.aws.amazon.com/aws-backup/latest/devguide/getting-started.html#gs-assumptions)\.
+ Affirmatively opt in to AWS Backup service\. Opt\-in choices apply to the specific account and AWS Region\. You might have to opt in to multiple Regions using the same account\. For more information, see [Getting started 1: Service Opt\-in](https://docs.aws.amazon.com/aws-backup/latest/devguide/service-opt-in.html)\.
+ From the Amazon Redshift console, you can create manual and automated snapshots\. AWS Backup only supports manual snapshots at this time\. 
+ Once you use AWS Backup to manage snapshot settings, you can't continue to manage manual snapshot settings using Amazon Redshift\. Instead, you can continue to manage the settings using an AWS Backup plan\. For more information, see [Managing backups using backup plans](https://docs.aws.amazon.com/aws-backup/latest/devguide/about-backup-plans.html)\.
+ To save storage costs when you have versioning\-enabled Amazon S3 buckets to be backed up, we recommend that you set a lifecycle expiration rule\. For information on specifying a lifecycle rule, see [Example 6: Specifying a lifecycle rule for a versioning\-enabled bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/lifecycle-configuration-examples.html#lifecycle-config-conceptual-ex6)\. If you don't set a lifecycle expiration period, your Amazon Redshift storage costs might increase, because AWS Backup retains all versions of your Amazon Redshift data\.

### Limitations<a name="managing-aws-backup-limitations"></a>

Following are limitations for using AWS Backup in Amazon Redshift:
+ You can't use AWS Backup to manage Amazon Redshift automated snapshots\. To manage automated snapshots, use tags\. For information about tagging resources, see [Tagging resources in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/amazon-redshift-tagging.html)\.
+ AWS Backup doesn't support Amazon Redshift Serverless\.