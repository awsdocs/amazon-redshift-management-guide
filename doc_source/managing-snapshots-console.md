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
+ [Sharing a cluster snapshot](#snapshot-share)
+ [Configuring cross\-Region snapshot copy for a nonencrypted cluster](#snapshot-crossregioncopy-configure)
+ [Configure cross\-Region snapshot copy for an AWS KMS–encrypted cluster](#xregioncopy-kms-encrypted-snapshot)
+ [Modifying the retention period for cross\-Region snapshot copy](#snapshot-crossregioncopy-modify)

## Creating a snapshot schedule<a name="snapshot-schedule-create"></a>

To precisely control when snapshots are taken, you can create a snapshot schedule and attach it to one or more clusters\. You can attach a schedule when you create a cluster or by modifying the cluster\. For more information, see [Automated snapshot schedules](working-with-snapshots.md#automated-snapshot-schedules)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-schedule-automatic"></a>

**To create a snapshot schedule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the **Snapshot schedules** tab\. The snapshot schedules are displayed\. 

1. Choose **Add schedule** to display the page to add a schedule\. 

1. Enter the properties of the schedule definition, then choose **Add schedule**\. 

1. On the page that appears, you can attach clusters to your new snapshot schedule, then choose **OK**\. 

### Original console<a name="snapshot-schedule-automatic-originalconsole"></a><a name="snapshot-schedule-create-task"></a>

**To create a snapshot schedule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. Choose **Snapshot Schedules**\.

1. Choose **Add Schedule**\.

1. Under **When do you want to take automated snapshots?** choose **Configure custom automated snapshot rules** and then add one or more rules\. Or choose **Take a snapshot every 8 hours** and specify the number of hours\.

1. Choose **Select a snapshot rule to add ** and choose a rule template from the list\. You can add multiple rules\.

1. Modify the template fields to customize your schedule\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-sample-schedule.png)

1. To view the schedule, choose **Preview schedule for all rules**\.

1. Choose **Add Schedule**\.

## Creating a manual snapshot<a name="snapshot-create"></a>

You can create a manual snapshot of a cluster from the snapshots list as follows\. Or, you can take a snapshot of a cluster in the cluster configuration pane\. For more information, see [Creating a snapshot of a cluster](managing-clusters-console.md#snapshot-cluster)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-create-manual"></a>

**To create a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose **Create snapshot**\. The snapshot page to create a manual snapshot is displayed\. 

1. Enter the properties of the snapshot definition, then choose **Create snapshot**\. It might take some time for the snapshot to be available\. 

### Original console<a name="snapshot-create-manual-originalconsole"></a><a name="snapshot-create-task"></a>

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

## Changing the manual snapshot retention period<a name="snapshot-manual-retention-period"></a>

You can change the retention period for a manual snapshot by modifying the snapshot settings\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-manual-retention"></a>

**To change the manual snapshot retention period**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the manual snapshot to change\. 

1. For **Actions**, choose **Manual snapshot settings** to display the properties of the manual snapshot\. 

1. Enter the revised properties of the snapshot definition, then choose **Save**\. 

### Original console<a name="snapshot-manual-retention-originalconsole"></a><a name="snapshot-manual-retention-period-task"></a>

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

## Deleting manual snapshots<a name="snapshot-delete"></a>

You can delete manual snapshots by selecting one or more snapshots in the snapshot list\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-delete"></a>

**To delete a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the snapshot to delete\. 

1. For **Actions**, choose **Delete snapshot** to delete the snapshot\. 

1. Confirm the deletion of the listed snapshots, then choose **Delete**\. 

### Original console<a name="snapshot-delete-originalconsole"></a><a name="snapshot-delete-task"></a>

**To delete a manual snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Snapshots**\.

1. To filter the list, do any or all of the following:
   + In the **Time Range** box, choose a time range that will narrow your search appropriately\.
   + In the **Type** box, choose **manual**\.
   + In the **Cluster** box, choose a cluster name to list snapshots for a single cluster, or choose **All Clusters** to list snapshots from all clusters\.
   + In the **Sort by** field, choose how you want the list ordered\.

1. In the snapshot list, select the rows that contain the snapshots that you want to delete\.

1. Choose **Delete Manual Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-delete-10.png)

1. In the **Delete Manual Snapshot** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/snapshot-delete-20.png)

## Copying an automated snapshot<a name="snapshot-copy"></a>

Automated snapshots are automatically deleted when their retention period expires, when you disable automated snapshots, or when you delete a cluster\. If you want to keep an automated snapshot, you can copy it to a manual snapshot\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-automated-copy"></a>

**To copy an automated snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the snapshot to copy\. 

1. For **Actions**, choose **Copy automated snapshot** to copy the snapshot\. 

1. Update the properties of the new snapshot, then choose **Copy**\. 

### Original console<a name="snapshot-automatic-copy-originalconsole"></a><a name="snapshot-copy-task"></a>

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

## Restoring a cluster from a snapshot<a name="snapshot-restore"></a>

When you restore a cluster from a snapshot, Amazon Redshift creates a new cluster with all the snapshot data on the new cluster\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-cluster-restore"></a>

**To restore a cluster from a snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the snapshot to restore\. 

1. Choose **Restore from snapshot** to view the **Cluster configuration** and **Cluster details** values of the new cluster to be created using the snapshot information\. 

1. Update the properties of the new cluster, then choose **Restore cluster from snapshot**\. 

### Original console<a name="snapshot-cluster-restore-originalconsole"></a>

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

   1. For **Publicly Accessible**, select **Yes** if you want the cluster to have a public IP address that can be accessed over a public connection to the Internet\. Select **No** if you want the cluster to have a private IP address that can only be accessed from within the VPC\. If your AWS account allows you to create EC2\-Classic clusters, the default is **No**\. Otherwise, the default is **Yes**\. 

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

## Sharing a cluster snapshot<a name="snapshot-share"></a>

You can authorize other users to access a manual snapshot you own, and you can later revoke that access when it is no longer required\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-share-access"></a>

**To share a snapshot with another account**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, **Snapshots**, then choose the manual snapshot to share\. 

1. For **Actions**, choose **Manual snapshot settings** to display the properties of the manual snapshot\. 

1. Enter the account or accounts to share with in the **Manage access** section, then choose **Save**\. 

### Original console<a name="snapshot-share-access-originalconsole"></a><a name="snapshot-share-task"></a>

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

## Configuring cross\-Region snapshot copy for a nonencrypted cluster<a name="snapshot-crossregioncopy-configure"></a>

You can configure Amazon Redshift to copy snapshots for a cluster to another AWS Region\. To configure cross\-Region snapshot copy, you need to enable this copy feature for each cluster and configure where to copy snapshots and how long to keep copied automated snapshots in the destination AWS Region\. When cross\-Region copy is enabled for a cluster, all new manual and automated snapshots are copied to the specified AWS Region\. Copied snapshot names are prefixed with **copy:**\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-crossregion-configure"></a>

**To configure a cross\-Region snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot** to display the configuration page\. 

1. Enter the properties of the new configuration, then **Save**\. 

### Original console<a name="snapshot-crossregion-configure-originalconsole"></a><a name="snapshot-crossregioncopy-configure-task"></a>

**To configure cross\-Region snapshot copy for a nonencrypted cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Backup**, and then choose **Configure Cross\-Region Snapshots**\.

1. In the **Configure Cross\-Region Snapshots** dialog box, for **Copy Snapshots** choose **Yes**\.

1. In **Destination Region**, choose the AWS Region to which to copy snapshots\.

1. In **Retention Period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination AWS Region before they are deleted\.

1. Choose **Save**\.

## Configure cross\-Region snapshot copy for an AWS KMS–encrypted cluster<a name="xregioncopy-kms-encrypted-snapshot"></a>

 When you launch an Amazon Redshift cluster, you can choose to encrypt it with a master key from the AWS Key Management Service \(AWS KMS\)\. AWS KMS keys are specific to an AWS Region\. If you want to enable cross\-Region snapshot copy for an AWS KMS–encrypted cluster, you must configure a *snapshot copy grant* for a master key in the destination AWS Region\. By doing this, you enable Amazon Redshift to perform encryption operations in the destination AWS Region\.

The following procedure describes the process of enabling cross\-Region snapshot copy for an AWS KMS\-encrypted cluster\. For more information about encryption in Amazon Redshift and snapshot copy grants, see [Copying AWS KMS–encrypted snapshots to another AWS Region](working-with-db-encryption.md#configure-snapshot-copy-grant)\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-crossregion-encrypt-configure"></a>

**To configure a cross\-Region snapshot for an AWS KMS–encrypted cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot** to display the configuration page\. 

1. Enter the properties of the new configuration, then **Save**\. 

### Original console<a name="snapshot-crossregion-encrypt-configure-originalconsole"></a><a name="xregioncopy-kms-encrypted-snapshot-task"></a>

**To configure cross\-Region snapshot copy for an AWS KMS\-encrypted cluster**

1. Open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the cluster list, choose a cluster name to open the **Configuration** view for the cluster\.

1. Choose **Backup**, and then choose **Configure Cross\-Region Snapshots**\.

1. In the **Configure Cross\-Region Snapshots** dialog box, for **Copy Snapshots** choose **Yes**\.

1. In **Destination Region**, choose the AWS Region to which to copy snapshots\.

1. In **Retention Period \(days\)**, choose the number of days for which you want automated snapshots to be retained in the destination AWS Region before they are deleted\.

1. For **Existing Snapshot Copy Grant**, do one of the following:

   1. Choose **No** to create a new snapshot copy grant\. For **KMS Key**, choose the AWS KMS key for which to create the grant, and then type a name in **Snapshot Copy Grant Name**\.

   1. Choose **Yes** to choose an existing snapshot copy grant from the destination AWS Region\. Then choose a grant from **Snapshot Copy Grant**\.

1. Choose **Save**\.

## Modifying the retention period for cross\-Region snapshot copy<a name="snapshot-crossregioncopy-modify"></a>

After you configure cross\-Region snapshot copy, you might want to change the settings\. You can easily change the retention period by selecting a new number of days and saving the changes\. 

**Warning**  
You can't modify the destination AWS Region after cross\-Region snapshot copy is configured\.   
If you want to copy snapshots to a different AWS Region, first disable cross\-Region snapshot copy\. Then re\-enable it with a new destination AWS Region and retention period\. Any copied automated snapshots are deleted after you disable cross\-Region snapshot copy\. Thus, you should determine if there are any that you want to keep and copy them to manual snapshots before disabling cross\-Region snapshot copy\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="snapshot-crossregion-modify"></a>

**To modify a cross\-Region snapshot**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to modify snapshots for\.

1. For **Actions**, choose **Configure cross\-region snapshot** to display the properties of the snapshot\. 

1. Enter the revised properties of the snapshot definition, then choose **Save**\. 

### Original console<a name="snapshot-crossregion-modify-originalconsole"></a><a name="snapshot-crossregioncopy-modify-task"></a>

**To modify the retention period for snapshots copied to a destination cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose **Backup**, and then choose **Configure Cross Region Snapshots**\.

1. In the **Retention Period** box, select the new number of days that you want automated snapshots to be retained in the destination AWS Region\.

   If you select a smaller number of days to retain snapshots in the destination AWS Region, any automated snapshots that were taken before the new retention period are deleted\. If you select a larger number of days to retain snapshots in the destination AWS Region, the retention period for existing automated snapshots is extended\. It lengthens by the difference between the old value and the new value\.

1. Choose **Save Configuration**\.