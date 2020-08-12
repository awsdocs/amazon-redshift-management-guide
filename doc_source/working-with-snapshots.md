# Amazon Redshift snapshots<a name="working-with-snapshots"></a>

**Topics**
+ [Overview](#working-with-snapshots-overview)
+ [Automated snapshots](#about-automated-snapshots)
+ [Automated snapshot schedules](#automated-snapshot-schedules)
+ [Snapshot schedule format](#working-with-snapshot-scheduling)
+ [Manual snapshots](#about-manual-snapshots)
+ [Managing snapshot storage](#managing-snapshot-storage)
+ [Excluding tables from snapshots](#snapshots-no-backup-tables)
+ [Copying snapshots to another AWS Region](#cross-region-snapshot-copy)
+ [Restoring a cluster from a snapshot](#working-with-snapshot-restore-cluster-from-snapshot)
+ [Restoring a table from a snapshot](#working-with-snapshot-restore-table-from-snapshot)
+ [Sharing snapshots](#working-with-snapshot-share-snapshot)
+ [Managing snapshots using the console](managing-snapshots-console.md)
+ [Managing snapshots using the AWS SDK for Java](managing-snapshots-java.md)
+ [Managing snapshots using the Amazon Redshift CLI and API](manage-snapshots-api-cli.md)

## Overview<a name="working-with-snapshots-overview"></a>

Snapshots are point\-in\-time backups of a cluster\. There are two types of snapshots: *automated* and *manual*\. Amazon Redshift stores these snapshots internally in Amazon S3 by using an encrypted Secure Sockets Layer \(SSL\) connection\. 

Amazon Redshift automatically takes incremental snapshots that track changes to the cluster since the previous automated snapshot\. Automated snapshots retain all of the data required to restore a cluster from a snapshot\. You can create a snapshot schedule to control when automated snapshots are taken, or you can take a manual snapshot any time\.

When you restore from a snapshot, Amazon Redshift creates a new cluster and makes the new cluster available before all of the data is loaded, so you can begin querying the new cluster immediately\. The cluster streams data on demand from the snapshot in response to active queries, then loads the remaining data in the background\. 

When you launch a cluster, you can set the retention period for automated and manual snapshots\. You can change the retention period for automated and manual snapshots by modifying the cluster\. You can change the retention period for a manual snapshot when you create the snapshot or by modifying the snapshot\. 

You can monitor the progress of snapshots by viewing the snapshot details in the AWS Management Console, or by calling [describe\-cluster\-snapshots](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-cluster-snapshots.html) in the CLI or the [DescribeClusterSnapshots](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusterSnapshots.html) API action\. For an in\-progress snapshot, these display information such as the size of the incremental snapshot, the transfer rate, the elapsed time, and the estimated time remaining\. 

To ensure that your backups are always available to your cluster, Amazon Redshift stores snapshots in an internally managed Amazon S3 bucket that is managed by Amazon Redshift\. Amazon Redshift provides free storage for snapshots that is equal to the storage capacity of your cluster until you delete the cluster\. After you reach the free snapshot storage limit, you are charged for any additional storage at the normal rate\. Because of this, you should evaluate how many days you need to keep snapshots and configure their retention period accordingly, and delete any manual snapshots that you no longer need\. For pricing information, see the Amazon Redshift [product detail page](https://aws.amazon.com/redshift/)\. 

## Automated snapshots<a name="about-automated-snapshots"></a>

When automated snapshots are enabled for a cluster, Amazon Redshift periodically takes snapshots of that cluster\. By default Amazon Redshift takes a snapshot about every eight hours or following every 5 GB per node of data changes, or whichever comes first\. Alternatively, you can create a snapshot schedule to control when automated snapshots are taken\. Automated snapshots are enabled by default when you create a cluster\. 

Automated snapshots are deleted at the end of a retention period\. The default retention period is one day, but you can modify it by using the Amazon Redshift console or programmatically by using the Amazon Redshift API or CLI\.

To disable automated snapshots, set the retention period to zero\. If you disable automated snapshots, Amazon Redshift stops taking snapshots and deletes any existing automated snapshots for the cluster\.

Only Amazon Redshift can delete an automated snapshot; you cannot delete them manually\. Amazon Redshift deletes automated snapshots at the end of a snapshot's retention period, when you disable automated snapshots for the cluster, or when you delete the cluster\. *Amazon Redshift retains the latest automated snapshot until you disable automated snapshots or delete the cluster\.*

If you want to keep an automated snapshot for a longer period, you can create a copy of it as a manual snapshot\. The automated snapshot is retained until the end of the retention period, but the corresponding manual snapshot is retained until you manually delete it or until the end of the retention period\.

## Automated snapshot schedules<a name="automated-snapshot-schedules"></a>

To precisely control when snapshots are taken, you can create a snapshot schedule and attach it to one or more clusters\. When you modify a snapshot schedule, the schedule is modified for all associated clusters\. If a cluster doesn't have a snapshot schedule attached, the cluster uses the default automated snapshot schedule\. 

A *snapshot schedule* is a set of schedule rules\. You can define a simple schedule rule based on a specified interval, such as every 8 hours or every 12 hours\. You can also add rules to take snapshots on certain days of the week, at specific times, or during specific periods\. Rules can also be defined using Unix\-like cron expressions\. 

## Snapshot schedule format<a name="working-with-snapshot-scheduling"></a>

On the Amazon Redshift console, you can create a snapshot schedule\. Then, you can attach a schedule to a cluster to trigger the creation of a system snapshot\. A schedule can be attached to multiple clusters, and you can create multiple cron definitions in a schedule to trigger a snapshot\.

You can define a schedule for your snapshots using a cron syntax\. The definition of these schedules uses a modified Unix\-like  [cron](http://en.wikipedia.org/wiki/Cron) syntax\. You specify time in [Coordinated universal time \(UTC\)](http://en.wikipedia.org/wiki/Coordinated_Universal_Time)\. You can create schedules with a maximum frequency of one hour and minimum precision of one minute\.

Amazon Redshift modified cron expressions have 3 required fields, which are separated by white space\. 

**Syntax**

```
cron(Minutes Hours Day-of-week)
```


| **Fields** | **Values** | **Wildcards** | 
| --- | --- | --- | 
|  Minutes  |  0–59  |   | 
|  Hours  |  0–23  |  , \- \* /   | 
|  Day\-of\-week  |  1–7 or SUN\-SAT  |  , \- \* /   | 

**Wildcards**
+ The **,** \(comma\) wildcard includes additional values\. In the `Day-of-week` field, `MON,WED,FRI` would include Monday, Wednesday, and Friday\. Total values are limited to 24 per field\.
+ The **\-** \(dash\) wildcard specifies ranges\. In the `Hour` field, 1–15 would include hours 1 through 15 of the specified day\.
+ The **\*** \(asterisk\) wildcard includes all values in the field\. In the `Hours` field, **\*** would include every hour\.
+ The **/** \(forward slash\) wildcard specifies increments\. In the `Hours` field, you could enter **1/10** to specify every 10th hour, starting from the first hour of the day \(for example, the 01:00, 11:00, and 21:00\)\.

**Limits**
+ Snapshot schedules that lead to backup frequencies less than 1 hour or greater than 24 hours are not supported\. If you have overlapping schedules that result in scheduling snapshots within a 1 hour window, a validation error results\. 

When creating a schedule, you can use the following sample cron strings\.


| Minutes | Hours | Day of week | Meaning | 
| --- | --- | --- | --- | 
|  0  |  14\-20/1  |  TUE  |  Every hour between 2pm and 8pm on Tuesday\.  | 
|  0  |  21  |  MON\-FRI  |  Every night at 9pm Monday–Friday\.  | 
|  30  |  0/6  |  SAT\-SUN  |  Every 6 hour increment on Saturday and Sunday starting at 30 minutes after midnight \(00:30\) that day\. This results in a snapshot at \[00:30, 06:30, 12:30, and 18:30\] each day\.  | 
|  30  |  12/4  |  \*  |  Every 4 hour increment starting at 12:30 each day\. This resolves to \[12:30, 16:30, 20:30\]\.  | 

For example to run on a schedule on an every 2 hour increment starting at 15:15 each day\. This resolves to \[15:15, 17:15, 19:15, 21:15, 23:15\] , specify:

```
cron(15 15/2 *)   
```

You can create multiple cron schedule definitions within as schedule\. For example the following AWS CLI command contains two cron schedules in one schedule\.

```
create-snapshot-schedule --schedule-identifier "my-test" --schedule-definition "cron(0 17 SAT,SUN)" "cron(0 9,17 MON-FRI)"   
```

## Manual snapshots<a name="about-manual-snapshots"></a>

You can take a manual snapshot any time\. By default, manual snapshots are retained indefinitely, even after you delete your cluster\. You can specify the retention period when you create a manual snapshot, or you can change the retention period by modifying the snapshot\. 

If a snapshot is deleted, you can't start any new operations that reference that snapshot\. However, if a restore operation is in progress, that restore operation will run to completion\. 

Amazon Redshift has a quota that limits the total number of manual snapshots that you can create; this quota is per AWS account per AWS Region\. The default quota is listed at [AWS service limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift)\. 

## Managing snapshot storage<a name="managing-snapshot-storage"></a>

Because snapshots accrue storage charges, it's important that you delete them when you no longer need them\. Amazon Redshift deletes automated and manual snapshots at the end of their respective snapshot retention periods\. You can also delete manual snapshots using the AWS Management Console or with the [batch\-delete\-cluster\-snapshots](https://docs.aws.amazon.com/cli/latest/reference/redshift/batch-delete-cluster-snapshots.html) CLI command\. 

You can change the retention period for a manual snapshot by modifying the manual snapshot settings\. 

You can get information about how much storage your snapshots are consuming using the Amazon Redshift Console or using the [describe\-storage](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-storage.html) CLI command\. 

## Excluding tables from snapshots<a name="snapshots-no-backup-tables"></a>

By default, all user\-defined permanent tables are included in snapshots\. If a table, such as a staging table, doesn't need to be backed up, you can significantly reduce the time needed to create snapshots and restore from snapshots\. You also reduce storage space on Amazon S3 by using a no\-backup table\. To create a no\-backup table, include the BACKUP NO parameter when you create the table\. For more information, see [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) and [CREATE TABLE AS](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_AS.html) in the *Amazon Redshift Database Developer Guide*\.

## Copying snapshots to another AWS Region<a name="cross-region-snapshot-copy"></a>

You can configure Amazon Redshift to automatically copy snapshots \(automated or manual\) for a cluster to another AWS Region\. When a snapshot is created in the cluster's primary AWS Region, it's copied to a secondary AWS Region\. The two AWS Regions are known respectively as the *source AWS Region* and *destination AWS Region*\. If you store a copy of your snapshots in another AWS Region, you can restore your cluster from recent data if anything affects the primary AWS Region\. You can configure your cluster to copy snapshots to only one destination AWS Region at a time\. For a list of Amazon Redshift Regions, see [Regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

When you enable Amazon Redshift to automatically copy snapshots to another AWS Region, you specify the destination AWS Region to copy the snapshots to\. For automated snapshots, you can also specify the retention period to keep them in the destination AWS Region\. After an automated snapshot is copied to the destination AWS Region and it reaches the retention time period there, it's deleted from the destination AWS Region\. Doing this keeps your snapshot usage low\. To keep the automated snapshots for a shorter or longer time in the destination AWS Region, change this retention period\.

The retention period that you set for automated snapshots that are copied to the destination AWS Region is separate from the retention period for automated snapshots in the source AWS Region\. The default retention period for copied snapshots is seven days\. That seven\-day period applies only to automated snapshots\. In both the source and destination AWS Regions, manual snapshots are deleted at the end of the snapshot retention period or when you manually delete them\.

You can disable automatic snapshot copy for a cluster at any time\. When you disable this feature, snapshots are no longer copied from the source AWS Region to the destination AWS Region\. Any automated snapshots copied to the destination AWS Region are deleted as they reach the retention period limit, unless you create manual snapshot copies of them\. These manual snapshots, and any manual snapshots that were copied from the destination AWS Region, are kept in the destination AWS Region until you manually delete them\.

To change the destination AWS Region that you copy snapshots to, first disable the automatic copy feature\. Then re\-enable it, specifying the new destination AWS Region\.

After a snapshot is copied to the destination AWS Region, it becomes active and available for restoration purposes\.

To copy snapshots for AWS KMS–encrypted clusters to another AWS Region, create a grant for Amazon Redshift to use a KMS customer master key \(CMK\) in the destination AWS Region\. Then choose that grant when you enable copying of snapshots in the source AWS Region\. For more information about configuring snapshot copy grants, see [Copying AWS KMS–encrypted snapshots to another AWS Region](working-with-db-encryption.md#configure-snapshot-copy-grant)\.

## Restoring a cluster from a snapshot<a name="working-with-snapshot-restore-cluster-from-snapshot"></a>

A snapshot contains data from any databases that are running on your cluster\. It also contains information about your cluster, including the number of nodes, node type, and master user name\. If you restore your cluster from a snapshot, Amazon Redshift uses the cluster information to create a new cluster\. Then it restores all the databases from the snapshot data\. 

For the new cluster created from the original snapshot, you can choose the configuration, such as node type and number of nodes\. The cluster is restored in the same AWS Region and a random, system\-chosen Availability Zone, unless you specify another Availability Zone in your request\. When you restore a cluster from a snapshot, you can optionally choose a compatible maintenance track for the new cluster\.

**Note**  
When you restore a snapshot to a cluster with a different configuration, the snapshot must have been taken on a cluster with cluster version 1\.0\.10013, or later\. 

When a restore is in progress, events are typically emitted in the following order:

1. RESTORE\_STARTED – REDSHIFT\-EVENT\-2008 sent when the restore process begins\. 

1. RESTORE\_SUCCEEDED – REDSHIFT\-EVENT\-3003 sent when the new cluster has been created\. 

   The cluster is available for queries\. 

1. DATA\_TRANSFER\_COMPLETED – REDSHIFT\-EVENT\-3537 sent when data transfer complete\.  

**Note**  
RA3 clusters only emit RESTORE\_STARTED and RESTORE\_SUCCEEDED events\. There is no explicit data transfer to be done after a RESTORE succeeds because RA3 node types store data in Amazon Redshift managed storage\. With RA3 nodes, data is continuously transferred between RA3 nodes and Amazon Redshift managed storage as part of normal query processing\. RA3 nodes cache hot data locally and keep less frequently queried blocks in Amazon Redshift managed storage automatically\. 

You can monitor the progress of a restore by either calling the [DescribeClusters](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusters.html) API operation, or viewing the cluster details in the AWS Management Console\. For an in\-progress restore, these display information such as the size of the snapshot data, the transfer rate, the elapsed time, and the estimated time remaining\. For a description of these metrics, see [RestoreStatus](https://docs.aws.amazon.com/redshift/latest/APIReference/API_RestoreStatus.html)\.

You can't use a snapshot to revert an active cluster to a previous state\.

**Note**  
When you restore a snapshot into a new cluster, the default security group and parameter group are used unless you specify different values\. 

You might want to restore a snapshot to a cluster with a different configuration for these reasons:
+ When a cluster is made up of smaller node types and you want to consolidate it into a larger node type with fewer nodes\. 
+ When you have monitored your workload and determined the need to move to a node type with more CPU and storage\. 
+ When you want to measure performance of test workloads with different node types\. 

Restore has the following constraints: 
+ The new node configuration must have enough storage for existing data\. Even when you add nodes, your new configuration might not have enough storage because of the way that data is redistributed\. 
+ The restore operation checks if the snapshot was created on a cluster version that is compatible with the cluster version of the new cluster\. If the new cluster has a version level that is too early, then the restore operation fails and reports more information in an error message\.
+ The possible configurations \(number of nodes and node type\) you can restore to is determined by the number of nodes in the original cluster and the target node type of the new cluster\. To determine the possible configurations available, you can use the Amazon Redshift console or the `describe-node-configuration-options` AWS CLI command with `action-type restore-cluster`\. For more information about the restoring using the Amazon Redshift console, see [Restoring a cluster from a snapshot](managing-snapshots-console.md#snapshot-restore)\. 

The following steps take a cluster with many nodes and consolidate it into a bigger node type with a smaller number of nodes using the AWS CLI\. For this example, we start with a source cluster of 24 `ds2.xlarge` nodes\. In this case, suppose that we already created a snapshot of this cluster and want to restore it into a bigger node type\.

1.  Run the following command to get the details of our 24\-node `ds2.xlarge` cluster\. 

   ```
   aws redshift describe-clusters --region eu-west-1 -—cluster-identifier mycluster-123456789012
   ```

1. Run the following command to get the details of the snapshot\. 

   ```
   aws redshift describe-cluster-snapshots --region eu-west-1 -—snapshot-identifier mycluster-snapshot
   ```

1. Run the following command to describe the options available for this snapshot\. 

   ```
   aws redshift describe-node-configuration-options --snapshot-identifier mycluster-snapshot --region eu-west-1 -—action-type restore-cluster
   ```

   This command returns an option list with recommended node types, number of nodes, and disk utilization for each option\. For this example, the preceding command lists the following possible node configurations\. We choose to restore into a three\-node `ds2.8xlarge` cluster\.

   ```
   {
       "NodeConfigurationOptionList": [
           {
               "EstimatedDiskUtilizationPercent": 65.26134808858235,
               "NodeType": "ds2.xlarge",
               "NumberOfNodes": 24
           },
           {
               "EstimatedDiskUtilizationPercent": 32.630674044291176,
               "NodeType": "ds2.xlarge",
               "NumberOfNodes": 48
           },
           {
               "EstimatedDiskUtilizationPercent": 65.26134808858235,
               "NodeType": "ds2.8xlarge",
               "NumberOfNodes": 3
           },
           {
               "EstimatedDiskUtilizationPercent": 48.94601106643677,
               "NodeType": "ds2.8xlarge",
               "NumberOfNodes": 4
           },
           {
               "EstimatedDiskUtilizationPercent": 39.156808853149414,
               "NodeType": "ds2.8xlarge",
               "NumberOfNodes": 5
           },
           {
               "EstimatedDiskUtilizationPercent": 32.630674044291176,
               "NodeType": "ds2.8xlarge",
               "NumberOfNodes": 6
           }
       ]
   }
   ```

1. Run the following command to restore the snapshot into the cluster configuration that we chose\. After this cluster is restored, we have the same content as the source cluster, but the data has been consolidated into three `ds2.8xlarge` nodes\. 

   ```
   aws redshift restore-from-cluster-snapshot --region eu-west-1 --snapshot-identifier mycluster-snapshot -—cluster-identifier mycluster-123456789012-x --node-type ds2.8xlarge --number-of-nodes 3
   ```

## Restoring a table from a snapshot<a name="working-with-snapshot-restore-table-from-snapshot"></a>

You can restore a single table from a snapshot instead of restoring an entire cluster\. When you restore a single table from a snapshot, you specify the source snapshot, database, schema, and table name, and the target cluster, schema, and a new table name for the restored table\.

The new table name cannot be the name of an existing table\. To replace an existing table with a restored table from a snapshot, rename or drop the existing table before you restore the table from the snapshot\.

The target table is created using the source table's column definitions, table attributes, and column attributes except for foreign keys\. To prevent conflicts due to dependencies, the target table doesn't inherit foreign keys from the source table\. Any dependencies, such as views or permissions granted on the source table, are not applied to the target table\. 

If the owner of the source table exists, then that user is the owner of the restored table, provided that the user has sufficient permissions to become the owner of a relation in the specified database and schema\. Otherwise, the restored table is owned by the master user that was created when the cluster was launched\.

The restored table returns to the state it was in at the time the backup was taken\. This includes transaction visibility rules defined by the Amazon Redshift adherence to [serializable isolation](https://docs.aws.amazon.com/redshift/latest/dg/c_serial_isolation.html), meaning that data will be immediately visible to in flight transactions started after the backup\.

Restoring a table from a snapshot has the following limitations:
+ You can restore a table only to the current, active running cluster and from a snapshot that was taken of that cluster\.
+ You can restore only one table at a time\.
+ You cannot restore a table from a cluster snapshot that was taken prior to a cluster being resized\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-table-restore"></a>

**To restore a table from a snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to use to restore a table\. 

1. For **Actions**, choose **Restore table** to display the **Restore table** page\. 

1. Enter the information about which snapshot, source table, and target table to use, and then choose **Restore table**\. 

### Original console<a name="snapshot-table-restore-originalconsole"></a>

**To restore a table from a snapshot using the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose **Clusters** and choose a cluster\.

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

**Example Example: Restoring a table from a snapshot using the AWS CLI**  
The following example uses the `restore-table-from-cluster-snapshot` AWS CLI command to restore the `my-source-table` table from the `sample-database` schema in the `my-snapshot-id`\. The example restores the snapshot to the `mycluster-example` cluster with a new table name of `my-new-table`\.  

```
aws redshift restore-table-from-cluster-snapshot --cluster-identifier mycluster-example 
                                                 --new-table-name my-new-table 
                                                 --snapshot-identifier my-snapshot-id 
                                                 --source-database-name sample-database 
                                                 --source-table-name my-source-table
```

## Sharing snapshots<a name="working-with-snapshot-share-snapshot"></a>

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
  + After access to a snapshot has been revoked from an AWS account, no users in that account can access the snapshot\. This is so even if they have IAM policies that allow actions on the previously shared snapshot resource\.