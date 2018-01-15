# Amazon Redshift Snapshots<a name="working-with-snapshots"></a>


+ [Overview](#working-with-snapshots-overview)
+ [Managing Snapshots Using the Console](managing-snapshots-console.md)
+ [Managing Snapshots Using the AWS SDK for Java](managing-snapshots-java.md)
+ [Managing Snapshots Using the Amazon Redshift CLI and API](manage-snapshots-api-cli.md)

## Overview<a name="working-with-snapshots-overview"></a>

Snapshots are point\-in\-time backups of a cluster\. There are two types of snapshots: *automated* and *manual*\. Amazon Redshift stores these snapshots internally in Amazon S3 by using an encrypted Secure Sockets Layer \(SSL\) connection\. If you need to restore from a snapshot, Amazon Redshift creates a new cluster and imports data from the snapshot that you specify\. 

When you restore from a snapshot, Amazon Redshift creates a new cluster and makes the new cluster available before all of the data is loaded, so you can begin querying the new cluster immediately\. The cluster streams data on demand from the snapshot in response to active queries, then loads the remaining data in the background\. 

Amazon Redshift periodically takes incremental snapshots that track changes to the cluster since the previous snapshot\. Amazon Redshift retains all of the data required to restore a cluster from a snapshot\.

You can monitor the progress of snapshots by viewing the snapshot details in the AWS Management Console\. or by calling [describe\-cluster\-snapshots](http://docs.aws.amazon.com/cli/latest/reference/redshift/describe-cluster-snapshots.html) in the CLI or the [DescribeClusterSnapshots](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusterSnapshots.html) API action\. For an in\-progress snapshot, these display information such as the size of the incremental snapshot, the transfer rate, the elapsed time, and the estimated time remaining\. 

Amazon Redshift provides free storage for snapshots that is equal to the storage capacity of your cluster until you delete the cluster\. After you reach the free snapshot storage limit, you are charged for any additional storage at the normal rate\. Because of this, you should evaluate how many days you need to keep automated snapshots and configure their retention period accordingly, and delete any manual snapshots that you no longer need\. For pricing information, go to the Amazon Redshift [product detail page](https://aws.amazon.com/redshift/)\.

### Automated Snapshots<a name="about-automated-snapshots"></a>

When automated snapshots are enabled for a cluster, Amazon Redshift periodically takes snapshots of that cluster, usually every eight hours or following every 5 GB per node of data changes, or whichever comes first\. Automated snapshots are enabled by default when you create a cluster\. These snapshots are deleted at the end of a retention period\. The default retention period is one day, but you can modify it by using the Amazon Redshift console or programmatically by using the Amazon Redshift API\. 

To disable automated snapshots, set the retention period to zero\. If you disable automated snapshots, Amazon Redshift stops taking snapshots and deletes any existing automated snapshots for the cluster\.

Only Amazon Redshift can delete an automated snapshot; you cannot delete them manually\. Amazon Redshift deletes automated snapshots at the end of a snapshot’s retention period, when you disable automated snapshots, or when you delete the cluster\. *Amazon Redshift retains the latest automated snapshot until you disable automated snapshots or delete the cluster\.*

If you want to keep an automated snapshot for a longer period, you can create a copy of it as a manual snapshot\. The automated snapshot is retained until the end of retention period, but the corresponding manual snapshot is retained until you manually delete it\.

### Manual Snapshots<a name="about-manual-snapshots"></a>

Regardless of whether you enable automated snapshots, you can take a manual snapshot whenever you want\. Amazon Redshift will never automatically delete a manual snapshot\. Manual snapshots are retained even after you delete your cluster\.

Because manual snapshots accrue storage charges, it’s important that you manually delete them if you no longer need them\. If you delete a manual snapshot, you cannot start any new operations that reference that snapshot\. However, if a restore operation is in progress, that restore operation will run to completion\. 

Amazon Redshift has a quota that limits the total number of manual snapshots that you can create; this quota is per AWS account per region\. The default quota is listed at [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift)\. 

### Excluding Tables From Snapshots<a name="snapshots-no-backup-tables"></a>

By default, all user\-defined permanent tables are included in snapshots\. If a table, such as a staging table, doesn't need to be backed up, you can significantly reduce the time needed to create snapshots and restore from snapshots\. You also reduce storage space on Amazon S3 by using a no\-backup table\. To create a no\-backup table, include the BACKUP NO parameter when you create the table\. For more information, see [CREATE TABLE](http://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) and [CREATE TABLE AS](http://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_AS.html) in the *Amazon Redshift Database Developer Guide*\.

### Copying Snapshots to Another Region<a name="cross-region-snapshot-copy"></a>

You can configure Amazon Redshift to automatically copy snapshots \(automated or manual\) for a cluster to another region\. When a snapshot is created in the cluster’s primary region, it will be copied to a secondary region; these are known respectively as the *source region* and *destination region*\. By storing a copy of your snapshots in another region, you have the ability to restore your cluster from recent data if anything affects the primary region\. You can configure your cluster to copy snapshots to only one destination region at a time\. For a list of Amazon Redshift regions, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

When you enable Amazon Redshift to automatically copy snapshots to another region, you specify the destination region where you want snapshots to be copied\. In the case of automated snapshots, you can also specify the retention period that they should be kept in the destination region\. After an automated snapshot is copied to the destination region and it reaches the retention time period there, it is deleted from the destination region, keeping your snapshot usage low\. You can change this retention period if you need to keep the automated snapshots for a shorter or longer period of time in the destination region\.

The retention period that you set for automated snapshots that are copied to the destination region is separate from the retention period for automated snapshots in the source region\. The default retention period for copied snapshots is seven days\. That seven\-day period only applies to automated snapshots\. Manual snapshots are not affected by the retention period in either the source or destination regions, and they remain until you manually delete them\.

You can disable automatic snapshot copy for a cluster at any time\. When you disable this feature, snapshots are no longer copied from the source region to the destination region\. Any automated snapshots copied to the destination region are deleted as they reach the retention period limit, unless you create manual snapshot copies of them\. These manual snapshots, and any manual snapshots that were copied from the destination region, are retained in the destination region until you manually delete them\.

If you want to change the destination region that you copy snapshots to, you have to first disable the automatic copy feature and then re\-enable it, specifying the new destination region\.

Copying snapshots across regions incurs data transfer charges\. Once a snapshot is copied to the destination region, it becomes active and available for restoration purposes\.

If you want to copy snapshots for AWS KMS\-encrypted clusters to another region, you must create a grant for Amazon Redshift to use a AWS KMS customer master key \(CMK\) in the destination region\. Then you must select that grant when you enable copying of snapshots in the source region\. For more information about configuring snapshot copy grants, see [Copying AWS KMS\-Encrypted Snapshots to Another Region](working-with-db-encryption.md#configure-snapshot-copy-grant)\.

### Restoring a Cluster from a Snapshot<a name="working-with-snapshot-restore-cluster-from-snapshot"></a>

A snapshot contains data from any databases that are running on your cluster, and also information about your cluster, including the number of nodes, node type, and master user name\. If you need to restore your cluster from a snapshot, Amazon Redshift uses the cluster information to create a new cluster and then restores all the databases from the snapshot data\. The new cluster that Amazon Redshift creates from the snapshot will have same configuration, including the number and type of nodes, as the original cluster from which the snapshot was taken\. The cluster is restored in the same region and Availability Zone unless you specify another Availability Zone in your request\.

You can monitor the progress of a restore by either calling the [DescribeClusters](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusters.html) API action, or viewing the cluster details in the AWS Management Console\. For an in\-progress restore, these display information such as the size of the snapshot data, the transfer rate, the elapsed time, and the estimated time remaining\. For a description of these metrics, go to [RestoreStatus](http://docs.aws.amazon.com/redshift/latest/APIReference/API_RestoreStatus.html)\.

You cannot use a snapshot to revert an active cluster to a previous state\.

**Note**  
When you restore a snapshot into a new cluster, the default security group and parameter group are used unless you specify different values\. 

### Restoring a Table from a Snapshot<a name="working-with-snapshot-restore-table-from-snapshot"></a>

You can restore a single table from a snapshot instead of restoring an entire cluster\. When you restore a single table from a snapshot, you specify the source snapshot, database, schema, and table name, and the target cluster, schema, and a new table name for the restored table\.

The new table name cannot be the name of an existing table\. To replace an existing table with a restored table from a snapshot, rename or drop the existing table before you restore the table from the snapshot\.

The target table is created using the source table's column definitions, table attributes, and column attributes except for foreign keys\. To prevent conflicts due to dependencies, the target table doesn't inherit foreign keys from the source table\. Any dependencies, such as views or permissions granted on the source table, are not applied to the target table\. 

If the owner of the source table exists, then that user is the owner of the restored table, provided that the user has sufficient permissions to become the owner of a relation in the specified database and schema\. Otherwise, the restored table is owned by the master user that was created when the cluster was launched\.

The restored table returns to the state it was in at the time the backup was taken\. This includes transaction visibility rules defined by Redshift's adherence to [serializable isolation](http://docs.aws.amazon.com/redshift/latest/dg/c_serial_isolation.html), meaning that data will be immediately visible to in flight transactions started after the backup\.

Restoring a table from a snapshot has the following limitations:

+ You can restore a table only to the current, active running cluster and from a snapshot that was taken of that cluster\.

+ You can restore only one table at a time\.

+ You cannot restore a table from a cluster snapshot that was taken prior to a cluster being resized\.

**To restore a table from a snapshot using the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose **Clusters**\.

1. Choose the **Table restore** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-restore-table-from-snapshot.png)

1. Choose **Restore table**\.

1. In the **Table restore** panel, select a date range that contains the cluster snapshot that you want to restore from\. For example, you might select `Last 1 Week` for cluster snapshots taken in the previous week\.

1. Add the following information:

   + **From snapshot** – The identifier of the cluster snapshot that contains the table to restore from\.

   + **Source table to restore from**

     + **Database** – The name of the database from the cluster snapshot that contains the table to restore from\.

     + **Schema** – The name of the database schema from the cluster snapshot that contains the table to restore from\.

     + **Table** – The name of the table from the cluster snapshot to restore from\.

   + **Target table to restore to**

     + **Database** – The name of the database in the target cluster to restore the table to\.

     + **Schema** – The name of the database schema in the target cluster to restore the table to\.

     + **New table name** – The new name of the restored table\. This name cannot be the name of an existing table in the target database\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-restore-table-from-snapshot-2.png)

1. Choose **Restore** to restore the table\.

If you have restored at least one table from a cluster snapshot, you can copy the values from a previous table restore request into a new table restore request\. This approach means you don't have to retype values that will be the same for several table restore operations\.

**To copy from a previous table restore request to a new table restore operation:**

1. In the **Table restore** tab, choose an existing table restore status\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-restore-table-from-snapshot-3.png)

1. Choose **Copy restore request**\.

**Example Example: Restoring a Table from a Snapshot Using the AWS CLI**  
The following example uses the `restore-table-from-cluster-snapshot` AWS CLI command to restore the `my-source-table` table from the `sample-database` schema in the `my-snapshot-id`\. The example restores the snapshot to the `mycluster-example` cluster with a new table name of `my-new-table`\.  

```
aws redshift restore-table-from-cluster-snapshot --cluster-identifier mycluster-example 
                                                 --new-table-name my-new-table 
                                                 --snapshot-identifier my-snapshot-id 
                                                 --source-database-name sample-database 
                                                 --source-table-name my-source-table
```

### Sharing Snapshots<a name="working-with-snapshot-share-snapshot"></a>

You can share an existing manual snapshot with other AWS customer accounts by authorizing access to the snapshot\. You can authorize up to 20 for each snapshot and 100 for each AWS Key Management Service \(AWS KMS\) key\. That is, if you have 10 snapshots that are encrypted with a single KMS key, then you can authorize 10 AWS accounts to restore each snapshot, or other combinations that add up to 100 accounts and do not exceed 20 accounts for each snapshot\. A person logged in as a user in one of the authorized accounts can then describe the snapshot or restore it to create a new Amazon Redshift cluster under their account\. For example, if you use separate AWS customer accounts for production and test, a user can log on using the production account and share a snapshot with users in the test account\. Someone logged on as a test account user can then restore the snapshot to create a new cluster that is owned by the test account for testing or diagnostic work\. 

A manual snapshot is permanently owned by the AWS customer account under which it was created\. Only users in the account owning the snapshot can authorize other accounts to access the snapshot, or to revoke authorizations\. Users in the authorized accounts can only describe or restore any snapshot that has been shared with them; they cannot copy or delete snapshots that have been shared with them\. An authorization remains in effect until the snapshot owner revokes it\. If an authorization is revoked, the previously authorized user loses visibility of the snapshot and cannot launch any new actions referencing the snapshot\. If the account is in the process of restoring the snapshot when access is revoked, the restore runs to completion\. You cannot delete a snapshot while it has active authorizations; you must first revoke all of the authorizations\.

AWS customer accounts are always authorized to access snapshots owned by the account\. Attempts to authorize or revoke access to the owner account will receive an error\. You cannot restore or describe a snapshot that is owned by an inactive AWS customer account\. 

After you have authorized access to an AWS customer account, no IAM users in that account can perform any actions on the snapshot unless they have IAM policies that allow them to do so\.

+ IAM users in the snapshot owner account can authorize and revoke access to a snapshot only if they have an IAM policy that allows them to perform those actions with a resource specification that includes the snapshot\. For example, the following policy allows a user in AWS account `012345678912` to authorize other accounts to access a snapshot named `my-snapshot20130829`:

  ```
  {
    "Version": "2012-10-17",
    "Statement":[
      {
        "Effect":"Allow",
        "Action":[
            "redshift:AuthorizeSnapshotAccess",
            "redshift:RevokeSnapshotAccess"
            ],
        "Resource":[
             "arn:aws:redshift:us-east-1:012345678912:snapshot:*/my-snapshot20130829"
            ]
      }
    ]
  }
  ```

+ IAM users in an AWS account with which a snapshot has been shared cannot perform actions on that snapshot unless they have IAM policies allowing those actions:

  + To list or describe a snapshot, they must have an IAM policy that allows the `DescribeClusterSnapshots` action\. The following code shows an example:

    ```
    {
      "Version": "2012-10-17",
      "Statement":[
        {
          "Effect":"Allow",
          "Action":[
              "redshift:DescribeClusterSnapshots"
              ],
          "Resource":[
               "*"
              ]
        }
      ]
    }
    ```

  + To restore a snapshot, users must have an IAM policy that allows the `RestoreFromClusterSnapshot` action and has a resource element that covers both the cluster they are attempting to create and the snapshot\. For example, if a user in account `012345678912` has shared snapshot `my-snapshot20130829` with account `219876543210`, in order to create a cluster by restoring the snapshot, a user in account `219876543210` must have a policy such as the following:

    ```
    {
      "Version": "2012-10-17",
      "Statement":[
        {
          "Effect":"Allow",
          "Action":[
              "redshift:RestoreFromClusterSnapshot"
              ],
          "Resource":[
               "arn:aws:redshift:us-east-1:012345678912:snapshot:*/my-snapshot20130829",
               "arn:aws:redshift:us-east-1:219876543210:cluster:from-another-account"
              ]
        }
      ]
    }
    ```

  + Once access to a snapshot has been revoked from an AWS account, no users in that account can access the snapshot, even if they have IAM policies that allow actions on the previously shared snapshot resource\.