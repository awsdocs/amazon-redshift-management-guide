# Managing AWS Backup with Amazon Redshift<a name="managing-aws-backup-overview"></a>

To protect resources on your Amazon Redshift provisioned clusters, you can use the AWS Backup console, or programmatically use the AWS Backup API or AWS Command Line Interface \(AWS CLI\)\. When you need to recover a resource, you can use either the AWS Backup console or the AWS CLI to find and recover the resource you need\. For more information, see [AWS Command Line Interface](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/backup/index.html)\.

When using AWS Backup for Amazon Redshift, you can perform the following actions:
+ Create periodic backups that automatically initiate Amazon Redshift snapshots\. Periodic backups are useful to meet your long\-term data retention needs\. For more information, see [Amazon Redshift backups](https://docs.aws.amazon.com/aws-backup/latest/devguide/redshift-backups.html)\.
+ Automate backup scheduling and retention by centrally configuring backup plans\.
+ Restore a cluster to the saved backup you choose\. You set how often to back up your resources\. For more information, see [Restore an Amazon Redshift cluster](https://docs.aws.amazon.com/aws-backup/latest/devguide/redshift-restores.html)\.