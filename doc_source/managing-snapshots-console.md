# Managing Snapshots Using the Console<a name="managing-snapshots-console"></a>

**Topics**
+ [Creating a Snapshot Schedule](#snapshot-schedule-create)
+ [Creating a Manual Snapshot](#snapshot-create)
+ [Changing the Manual Snapshot Retention Period](#snapshot-manual-retention-period)
+ [Deleting Manual Snapshots](#snapshot-delete)
+ [Copying an Automated Snapshot](#snapshot-copy)
+ [Restoring a Cluster from a Snapshot](#snapshot-restore)
+ [Sharing a Cluster Snapshot](#snapshot-share)
+ [Configuring Cross\-Region Snapshot Copy for a Non\-Encrypted Cluster](#snapshot-crossregioncopy-configure)
+ [Configure Cross\-Region Snapshot Copy for an AWS KMS\-Encrypted Cluster](#xregioncopy-kms-encrypted-snapshot)
+ [Modifying the Retention Period for Cross\-Region Snapshot Copy](#snapshot-crossregioncopy-modify)
+ [Disabling Cross\-Region Snapshot Copy](#snapshot-crossregioncopy-disable)

Amazon Redshift takes automatic, incremental snapshots of your data periodically and saves them to Amazon S3\. Additionally, you can take manual snapshots of your data whenever you want\. This section explains how to manage your snapshots from the Amazon Redshift console\. For more information about snapshots, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.

All snapshot tasks in the Amazon Redshift console start from the snapshot list\. You can filter the list by using a time range, the snapshot type, and the cluster associated with the snapshot\. In addition, you can sort the list by date, size, and snapshot type\. When you select an existing snapshot, the snapshot details are shown inline in the list, as shown in the example following\. Depending on the snapshot type that you select, you will have different options available for working with the snapshot\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-list-10.png)

## Creating a Snapshot Schedule<a name="snapshot-schedule-create"></a>

To precisely control when snapshots are taken, you can create a snapshot schedule and attach it to one or more clusters\. You can attach a schedule when you create a cluster or by modifying the cluster\. For more information, see [Automated Snapshot Schedules](working-with-snapshots.md#automated-snapshot-schedules)\.<a name="snapshot-schedule-create-task"></a>

**To create a snapshot schedule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. Choose **Snapshot Schedules**\.

1. Choose **Add Schedule**\.

1. Under **When do you want to take automated snapshots?** choose **Configure custom automated snapshot rules** and then add one or more rules\. Alternatively, choose **Take a snapshot every 8 hours** and specify the number of hours\.

1. Choose **Select a snapshot rule to add ** and choose a rule template from the drop\-down list\. You can add multiple rules\.

1. Modify the template fields to customize your schedule\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-sample-schedule.png)

1. To view the schedule, choose **Preview schedule for all rules**\.

1. Choose **Add Schedule**\.

## Creating a Manual Snapshot<a name="snapshot-create"></a>

You can create a manual snapshot of a cluster from the snapshots list as follows\. Or, you can take a snapshot of a cluster in the cluster configuration pane\. For more information, see [Taking a Snapshot of a Cluster](managing-clusters-console.md#snapshot-cluster)\.<a name="snapshot-create-task"></a>

**To create a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. Choose **Create Snapshot**\.

1. In the **Create Snapshot** dialog box, do the following: 

   1. In the **Cluster identifier** box, choose the cluster that you want to take a snapshot of\.

   1. In the **Snapshot identifier** box, type a name for the snapshot\.

   1. For **Snapshot retention period**, enter the number of days to retain the snapshot\. To retain the snapshot indefinitely, enter **\-1**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-create-20.png)

1. Choose **Create**\.

   The snapshot might take some time to complete\. The new snapshot appears in the list of snapshots with its current status\. The example following shows that `snapshot-example` is in the process of being created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-create-30.png)

## Changing the Manual Snapshot Retention Period<a name="snapshot-manual-retention-period"></a>

You can change the retention period for a manual snapshot by modifying the snapshot settings\.<a name="snapshot-manual-retention-period-task"></a>

**To change the manual snapshot retention period**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-list-10.png)

1. To filter the list in order to find the snapshot that you want to copy, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Type** box, choose **manual**\.
   + In the **Cluster** box, choose a cluster name to list snapshots for a single cluster, or choose **All Clusters** to list snapshots from all clusters\.
   + In the **Sort by** field, choose how you want the list ordered\.

1. In the snapshot list, select the snapshot that you want to modify\.

1. Choose **Manual Snapshot Settings**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/manual-settings.png)

1. For **Snapshot retention period**, enter the number of days to retain the snapshot\. To retain the snapshot indefinitely, enter **\-1**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-manual-retention-period.png)

1. Choose **Save**\.

## Deleting Manual Snapshots<a name="snapshot-delete"></a>

You can delete manual snapshots by selecting one or more snapshots in the snapshot list\.<a name="snapshot-delete-task"></a>

**To delete a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. To filter the list, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Type** box, choose **manual**\.
   + In the **Cluster** box, choose a cluster name to list snapshots for a single cluster, or choose **All Clusters** to list snapshots from all clusters\.
   + In the **Sort by** field, choose how you want the list ordered\.

1. In the snapshot list, select the rows that contains the snapshots that you want to delete\.

1. Choose **Delete Manual Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-delete-10.png)

1. In the **Delete Manual Snapshot** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-delete-20.png)

## Copying an Automated Snapshot<a name="snapshot-copy"></a>

Automated snapshots are automatically deleted when their retention period expires, when you disable automated snapshots, or when you delete a cluster\. If you want to keep an automated snapshot, you can copy it to a manual snapshot\. <a name="snapshot-copy-task"></a>

**To copy an automated snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. To filter the list, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Type** box, choose **automated**\.
   + In the **Cluster** box, choose a cluster name to list snapshots for a single cluster, or choose **All Clusters** to list snapshots from all clusters\.
   + In the **Sort by** field, choose how you want the list ordered\.

1. In the snapshot list, select the snapshot that you want to copy\.

1. Choose **Copy Automated Snapshot**\.

1. In the **Snapshot Identifier** box of the **Copy Automated Snapshot** dialog box, enter a name for the snapshot copy\.

1. For **Snapshot retention period**, enter the number of days to retain the snapshot\. To retain the snapshot indefinitely, enter **\-1**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-copy-20.png)

1. Choose **Continue**\.

## Restoring a Cluster from a Snapshot<a name="snapshot-restore"></a>

When you restore a cluster from a snapshot, Amazon Redshift creates a new cluster with all the snapshot data on the new cluster\.

**Note**  
You can use these steps to change a cluster platform from EC2\-Classic to EC2\-VPC and vice versa\.<a name="snapshot-restore-task"></a>

**To restore a cluster from a snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. To filter the list, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Type** box, choose **manual** or **automated**\.
   + In the **Cluster** box, choose a cluster name to list snapshots for a single cluster, or choose **All Clusters** to list snapshots from all clusters\.
   + In the **Sort by** field, choose how you want the list ordered\.

1. In the snapshot list, choose the row that contains the snapshot that you want to use\.

1. Choose **Restore From Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/restore-from-snapshot-list.png)

1. In the **Restore Cluster from Snapshot** dialog box, do the following:

   1. In the **Cluster Identifier** box, type a cluster identifier for the restored cluster\. 

      Cluster identifiers must meet the following conditions:
      + They must contain from 1 to 255 alphanumeric characters or hyphens\.
      + Alphabetic characters must be lowercase\.
      + The first character must be a letter\.
      + They cannot end with a hyphen or contain two consecutive hyphens\.
      + They must be unique for all clusters within an AWS account\.

   1. In the **Port** box, accept the port from the snapshot or change the value as appropriate\.

   1. Select **Allow Version Upgrade** as appropriate\.

   1.  In **Cluster Subnet Group**, select the subnet group into which you want to restore the cluster\. 

      This option only appears if you restore the cluster into the EC2\-VPC platform\.

   1. In **Publicly Accessible**, select **Yes** if you want the cluster to have a public IP address that can be accessed over a public connection to the Internet, and select **No** if you want the cluster to have a private IP address that can only be accessed from within the VPC\. If your AWS account allows you to create EC2\-Classic clusters, the default is **No**\. Otherwise, the default is **Yes**\. 

      This option only appears if you restore the cluster into the EC2\-VPC platform\.

   1. In **Choose a Public IP Address**, select **Yes** if you want to select an elastic IP \(EIP\) address that you already have configured\. Otherwise, select **No** to have Amazon Redshift create an EIP for your instance\. 

      This option only appears if you restore the cluster into the EC2\-VPC platform\.

   1. In **Elastic IP**, select an EIP to use to connect to the cluster from outside of the VPC\. 

      This option only appears if you restore the cluster into the EC2\-VPC platform and you select **Yes** in **Choose a Public IP Address**\.

   1. In the **Availability Zone** box, accept the Availability Zone from the snapshot or change the value as appropriate\.

   1. In **Cluster Parameter Group**, select a parameter group to associate with the cluster\. 

   1. In **Cluster Security Groups** or **VPC Security Groups**, select a security group to associate with the cluster\. The types of security group that appear here depend on whether you're restoring the cluster into the EC2\-VPC or EC2\-Classic platform\. 

      The option to select a cluster security group or a VPC security group depends on whether you restore the cluster into the EC2\-VPC platform or the EC2\-Classic platform\. 

   1. In **Maintenance track**, the value of the maintenance track is displayed\. In **Change maintenance track to**, optionally choose to restore the cluster using one of the maintenance tracks listed\. 

   The following is an example of restoring a snapshot into a cluster that uses the EC2\-VPC platform\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-restore-from-snapshot-vpc.png)

   The following is an example of restoring a snapshot into a cluster that uses the EC2\-Classic platform\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-restore-from-snapshot-classic.png)

1. Choose **Restore**\.

## Sharing a Cluster Snapshot<a name="snapshot-share"></a>

You can authorize other users to access a manual snapshot you own, and you can later revoke that access when it is no longer required\.<a name="snapshot-share-task"></a>

**To share a cluster snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. If you need to filter the list in order to find the snapshot that you want to share, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Cluster** box, choose the cluster whose snapshot you want to share\.

1. In the snapshot list, choose the row that contains the snapshot that you want to use\.

1. Choose **Manage Access**\.

1. In the **Manage Snapshot Access** dialog box, you can either authorize a user to access the snapshot or revoke a previously authorized access\.
   + To authorize a user to access the snapshot, type that user's 12\-digit AWS account ID in the box \(omit the dashes\), and then choose **Add Account**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-share-10.png)
   + To revoke the authorization for a user, choose **X** beside that user's AWS account ID\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-share-20.png)

1. Choose **Save** to save your changes, or **Cancel** to roll back the changes\.

## Configuring Cross\-Region Snapshot Copy for a Non\-Encrypted Cluster<a name="snapshot-crossregioncopy-configure"></a>

You can configure Amazon Redshift to copy snapshots for a cluster to another region\. To configure cross\-region snapshot copy, you need to enable this copy feature for each cluster and configure where to copy snapshots and how long to keep copied automated snapshots in the destination region\. When cross\-region copy is enabled for a cluster, all new manual and automatic snapshots are copied to the specified region\. Copied snapshot names are prefixed with **copy:**\.<a name="snapshot-crossregioncopy-configure-task"></a>

**To configure cross\-region snapshot copy for a non\-encrypted cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Backup**, and then choose **Configure Cross\-Region Snapshots**\.

1. In the **Configure Cross\-Region Snapshots** dialog box, for **Copy Snapshots** choose **Yes**\.

1. In **Destination Region**, choose the region to which to copy snapshots\.

1. In **Retention Period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination region before they are deleted\.

1. Choose **Save**\.

## Configure Cross\-Region Snapshot Copy for an AWS KMS\-Encrypted Cluster<a name="xregioncopy-kms-encrypted-snapshot"></a>

 When you launch an Amazon Redshift cluster, you can choose to encrypt it with a master key from the AWS Key Management Service \(AWS KMS\)\. AWS KMS keys are specific to a region\. If you want to enable cross\-region snapshot copy for an AWS KMS\-encrypted cluster, you must configure a *snapshot copy grant* for a master key in the destination region so that Amazon Redshift can perform encryption operations in the destination region\. The following procedure describes the process of enabling cross\-region snapshot copy for an AWS KMS\-encrypted cluster\. For more information about encryption in Amazon Redshift and snapshot copy grants, see [Copying AWS KMS\-Encrypted Snapshots to Another AWS Region](working-with-db-encryption.md#configure-snapshot-copy-grant)\. <a name="xregioncopy-kms-encrypted-snapshot-task"></a>

**To configure cross\-region snapshot copy for an AWS KMS\-encrypted cluster**

1. Open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the cluster list, choose a cluster name to open the **Configuration** view for the cluster\.

1. Choose **Backup**, and then choose **Configure Cross\-Region Snapshots**\.

1. In the **Configure Cross\-Region Snapshots** dialog box, for **Copy Snapshots** choose **Yes**\.

1. In **Destination Region**, choose the region to which to copy snapshots\.

1. In **Retention Period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination region before they are deleted\.

1. For **Existing Snapshot Copy Grant**, do one of the following:

   1. Choose **No** to create a new snapshot copy grant\. For **KMS Key**, choose the AWS KMS key for which to create the grant, and then type a name in **Snapshot Copy Grant Name**\.

   1. Choose **Yes** to choose an existing snapshot copy grant from the destination region\. Then choose a grant from **Snapshot Copy Grant**\.

1. Choose **Save**\.

## Modifying the Retention Period for Cross\-Region Snapshot Copy<a name="snapshot-crossregioncopy-modify"></a>

After you configure cross\-region snapshot copy, you might want to change the settings\. You can easily change the retention period by selecting a new number of days and saving the changes\. 

**Warning**  
You cannot modify the destination region after cross\-region snapshot copy is configured\. If you want to copy snapshots to a different region, you must first disable cross\-region snapshot copy, and then re\-enable it with a new destination region and retention period\. Because any copied automated snapshots are deleted after you disable cross\-region snapshot copy, you should determine if there are any that you want to keep and copy them to manual snapshots before disabling cross\-region snapshot copy\.<a name="snapshot-crossregioncopy-modify-task"></a>

**To modify the retention period for snapshots copied to a destination cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Backup**, and then choose **Configure Cross Region Snapshots**\.

1. In the **Retention Period** box, select the new number of days that you want automated snapshots to be retained in the destination region\.

   If you select a smaller number of days to retain snapshots in the destination region, any automated snapshots that were taken before the new retention period will be deleted\. If you select a larger number of days to retain snapshots in the destination region, the retention period for existing automated snapshots will be extended by the difference between the old value and the new value\.

1. Choose **Save Configuration**\.

## Disabling Cross\-Region Snapshot Copy<a name="snapshot-crossregioncopy-disable"></a>

You can disable cross\-region snapshot copy for a cluster when you no longer want Amazon Redshift to copy snapshots to a destination region\.<a name="snapshot-crossregioncopy-disable-task"></a>

**To disable cross\-region snapshot copy for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Backup**, and then choose **Configure Cross Region Snapshots** to open the **Configure Cross Region Snapshots** dialog box\.

1. In the **Enable Cross Region Snapshots** box, choose **No**\.

1. Choose **Save Configuration**\.