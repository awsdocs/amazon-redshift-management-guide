# Managing snapshots using the console<a name="managing-snapshots-console"></a>

Amazon Redshift takes automatic, incremental snapshots of your data periodically and saves them to Amazon S3\. Additionally, you can take manual snapshots of your data whenever you want\. In this section, you can find how to manage your snapshots from the Amazon Redshift console\. For more information about snapshots, see [Amazon Redshift snapshots](working-with-snapshots.md)\.

All snapshot tasks in the Amazon Redshift console start from the snapshot list\. You can filter the list by using a time range, the snapshot type, and the cluster associated with the snapshot\. In addition, you can sort the list by date, size, and snapshot type\.  Depending on the snapshot type that you select, you might have different options available for working with the snapshot\. 

**Topics**
+ [Creating a snapshot schedule](#snapshot-schedule-create)
+ [Creating a manual snapshot](#snapshot-create)
+ [Changing the manual snapshot retention period](#snapshot-manual-retention-period)
+ [Deleting manual snapshots](#snapshot-delete)
+ [Copying an automated snapshot](#snapshot-copy)
+ [Restoring a cluster from a snapshot](#snapshot-restore)
+ [Restoring a serverless namespace from a snapshot](#snapshot-restore-provisioned-to-serverless)
+ [Sharing a cluster snapshot](#snapshot-share)
+ [Configuring cross\-Region snapshot copy for a nonencrypted cluster](#snapshot-crossregioncopy-configure)
+ [Configure cross\-Region snapshot copy for an AWS KMS–encrypted cluster](#xregioncopy-kms-encrypted-snapshot)
+ [Modifying the retention period for cross\-Region snapshot copy](#snapshot-crossregioncopy-modify)

## Creating a snapshot schedule<a name="snapshot-schedule-create"></a>

To precisely control when snapshots are taken, you can create a snapshot schedule and attach it to one or more clusters\. You can attach a schedule when you create a cluster or by modifying the cluster\. For more information, see [Automated snapshot schedules](working-with-snapshots.md#automated-snapshot-schedules)\.

**To create a snapshot schedule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the **Snapshot schedules** tab\. The snapshot schedules are displayed\. 

1. Choose **Add schedule** to display the page to add a schedule\. 

1. Enter the properties of the schedule definition, then choose **Add schedule**\. 

1. On the page that appears, you can attach clusters to your new snapshot schedule, then choose **OK**\. 

## Creating a manual snapshot<a name="snapshot-create"></a>

You can create a manual snapshot of a cluster from the snapshots list as follows\. Or, you can take a snapshot of a cluster in the cluster configuration pane\. For more information, see [Creating a snapshot of a cluster](managing-clusters-console.md#snapshot-cluster)\.

**To create a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose **Create snapshot**\. The snapshot page to create a manual snapshot is displayed\. 

1. Enter the properties of the snapshot definition, then choose **Create snapshot**\. It might take some time for the snapshot to be available\. 

## Changing the manual snapshot retention period<a name="snapshot-manual-retention-period"></a>

You can change the retention period for a manual snapshot by modifying the snapshot settings\.

**To change the manual snapshot retention period**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the manual snapshot to change\. 

1. For **Actions**, choose **Manual snapshot settings** to display the properties of the manual snapshot\. 

1. Enter the revised properties of the snapshot definition, then choose **Save**\. 

## Deleting manual snapshots<a name="snapshot-delete"></a>

You can delete manual snapshots by selecting one or more snapshots in the snapshot list\.

**To delete a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the snapshot to delete\. 

1. For **Actions**, choose **Delete snapshot** to delete the snapshot\. 

1. Confirm the deletion of the listed snapshots, then choose **Delete**\. 

## Copying an automated snapshot<a name="snapshot-copy"></a>

Automated snapshots are automatically deleted when their retention period expires, when you disable automated snapshots, or when you delete a cluster\. If you want to keep an automated snapshot, you can copy it to a manual snapshot\. 

**To copy an automated snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the snapshot to copy\. 

1. For **Actions**, choose **Copy automated snapshot** to copy the snapshot\. 

1. Update the properties of the new snapshot, then choose **Copy**\. 

## Restoring a cluster from a snapshot<a name="snapshot-restore"></a>

When you restore a cluster from a snapshot, Amazon Redshift creates a new cluster with all the snapshot data on the new cluster\.

**To restore a cluster from a snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the snapshot to restore\. 

1. Choose **Restore from snapshot** to view the **Cluster configuration** and **Cluster details** values of the new cluster to be created using the snapshot information\. 

1. Update the properties of the new cluster, then choose **Restore cluster from snapshot**\. 

If you have reserved nodes, for example DS2 or DC2 reserved nodes, you can upgrade to RA3 reserved nodes\. You can do this when you restore from a snapshot or perform an elastic resize\. You can use the console to guide you through this process\. For more information about upgrading to RA3 nodes, see [Upgrading to RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-upgrading-to-ra3)\. 

## Restoring a serverless namespace from a snapshot<a name="snapshot-restore-provisioned-to-serverless"></a>

 Restoring a serverless namespace from a snapshot replaces all of the namespace’s databases with databases in the snapshot\. For more information about serverless snapshots, see [ Working with snapshots and recovery points](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-snapshots-recovery.html)\. 

To restore a snapshot from your provisioned cluster to your Amazon Redshift Serverless instance\.

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the snapshot to use\.

1. Choose **Restore from snapshot**, **Restore to serverless namespace**\.

1. Choose the namespace you want to restore to\.

1. Confirm you want to restore from your snapshot\. Choose **restore**\. This action replaces all the databases in your Amazon Redshift Serverless instance with the data from your provisioned cluster\.

## Sharing a cluster snapshot<a name="snapshot-share"></a>

You can authorize other users to access a manual snapshot you own, and you can later revoke that access when it is no longer required\.

**To share a snapshot with another account**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the manual snapshot to share\. 

1. For **Actions**, choose **Manual snapshot settings** to display the properties of the manual snapshot\. 

1. Enter the account or accounts to share with in the **Manage access** section, then choose **Save**\. 

### Security considerations for sharing encrypted snapshots<a name="snapshot-share-access-kms-key"></a>

 When you provide access to an encrypted snapshot, Redshift requires that the AWS KMS customer managed key used to create the snapshot is shared with the account or accounts performing the restore\. If the key isn't shared, attempting to restore the snapshot results in an access\-denied error\. When you authorize snapshot access and share the key, the identity authorizing access must have `kms:DescribeKey` permissions on the key used to encrypt the snapshot\. This permission is described in more detail in [AWS KMS permissions](https://docs.aws.amazon.com/kms/latest/developerguide/kms-api-permissions-reference.html)\. For more information, see [DescribeKey](https://docs.aws.amazon.com/kms/latest/APIReference/API_DescribeKey.html) in the Amazon Redshift API reference documentation\. 

The customer managed key policy can be updated programmatically or on the console, using the AWS Key Management Service\.

#### Allowing access to the AWS KMS key for an encrypted snapshot<a name="snapshot-share-access-kms-key-allowing-access"></a>

To share the AWS KMS customer managed key for an encrypted snapshot, update the key policy by performing the following steps:

1. Update the KMS key policy with the Amazon Resource Name \(ARN\) of the AWS account that you are sharing to as `Principal` in the KMS key policy\.

1.  Allow the `kms:Decrypt` action\. 

In the following key\-policy example, user `111122223333` is the owner of the KMS key, and user `444455556666` is the account that the key is shared with\. This key policy gives the AWS account access to the sample KMS key by including the ARN for the root AWS account identity for user `444455556666` as a `Principal` for the policy, and by allowing the `kms:Decrypt` action\. 

```
{
    "Id": "key-policy-1",
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Allow use of the key",
            "Effect": "Allow",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::111122223333:user/KeyUser",
                    "arn:aws:iam::444455556666:root"
                ]
            },
            "Action": [
                "kms:Decrypt"
            ],
            "Resource": "*"
        }
    ]
}
```

After access is granted to the customer managed KMS key, the account that restores the encrypted snapshot must create an AWS Identity and Access Management \(IAM\) role, or user, if it doesn't already have one\. In addition, that AWS account must also attach an IAM policy to that IAM role or user that allows them to restore an encrypted database snapshot, using your KMS key\.   

For more information about giving access to an AWS KMS key, see [Allowing users in other accounts to use a KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying-external-accounts.html#cross-account-console), in the AWS Key Management Service developer guide\.

For an overview of key policies, see [How Amazon Redshift uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-redshift.html)\.

## Configuring cross\-Region snapshot copy for a nonencrypted cluster<a name="snapshot-crossregioncopy-configure"></a>

You can configure Amazon Redshift to copy snapshots for a cluster to another AWS Region\. To configure cross\-Region snapshot copy, you need to enable this copy feature for each cluster and configure where to copy snapshots and how long to keep copied automated or manual snapshots in the destination AWS Region\. When cross\-Region copy is enabled for a cluster, all new manual and automated snapshots are copied to the specified AWS Region\. Copied snapshot names are prefixed with **copy:**\.

**To configure a cross\-Region snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot**\.

   The Configure cross\-Region dialog box appears\.

1. For **Copy snapshots**, choose **Yes**\.

1. In **Destination AWS Region**, choose the AWS Region to which to copy snapshots\.

1. In **Automated snapshot retention period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination AWS Region before they are deleted\.

1. In **Manual snapshot retention period**, choose the value that represents the number of days for which you want manual snapshots to be retained in the destination AWS Region before they are deleted\. If you choose **Custom value**, the retention period must be between 1 to 3653 days\.

1. Choose **Save**\.

## Configure cross\-Region snapshot copy for an AWS KMS–encrypted cluster<a name="xregioncopy-kms-encrypted-snapshot"></a>

 When you launch an Amazon Redshift cluster, you can choose to encrypt it with a root key from the AWS Key Management Service \(AWS KMS\)\. AWS KMS keys are specific to an AWS Region\. If you want to enable cross\-Region snapshot copy for an AWS KMS–encrypted cluster, you must configure a *snapshot copy grant* for a root key in the destination AWS Region\. By doing this, you enable Amazon Redshift to perform encryption operations in the destination AWS Region\.

The following procedure describes the process of enabling cross\-Region snapshot copy for an AWS KMS\-encrypted cluster\. For more information about encryption in Amazon Redshift and snapshot copy grants, see [Copying AWS KMS–encrypted snapshots to another AWS Region](working-with-db-encryption.md#configure-snapshot-copy-grant)\. 

**To configure a cross\-Region snapshot for an AWS KMS–encrypted cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot**\.

   The Configure cross\-Region dialog box appears\.

1. For **Copy snapshots**, choose **Yes**\.

1. In **Destination AWS Region**, choose the AWS Region to which to copy snapshots\.

1. In **Automated snapshot retention period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination AWS Region before they are deleted\.

1. In **Manual snapshot retention period**, choose the value that represents the number of days for which you want manual snapshots to be retained in the destination AWS Region before they are deleted\. If you choose **Custom value**, the retention period must be between 1 to 3653 days\.

1. Choose **Save**\.

## Modifying the retention period for cross\-Region snapshot copy<a name="snapshot-crossregioncopy-modify"></a>

After you configure cross\-Region snapshot copy, you might want to change the settings\. You can easily change the retention period by selecting a new number of days and saving the changes\. 

**Warning**  
You can't modify the destination AWS Region after cross\-Region snapshot copy is configured\.   
If you want to copy snapshots to a different AWS Region, first disable cross\-Region snapshot copy\. Then re\-enable it with a new destination AWS Region and retention period\. Any copied automated snapshots are deleted after you disable cross\-Region snapshot copy\. Thus, you should determine if there are any that you want to keep and copy them to manual snapshots before disabling cross\-Region snapshot copy\.

**To modify a cross\-Region snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to modify snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot** to display the properties of the snapshot\. 

1. Enter the revised properties of the snapshot definition, then choose **Save**\. 