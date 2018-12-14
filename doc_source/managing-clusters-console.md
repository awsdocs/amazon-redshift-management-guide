# Managing Clusters Using the Console<a name="managing-clusters-console"></a>

To create, modify, resize, delete, reboot, and back up clusters, use the **Clusters** section in the Amazon Redshift console\. 

When you don't have any clusters in an AWS Region and you open the **Clusters** page, you have the option to launch a cluster\. In the following screenshot, the AWS Region is the US East \(N\. Virginia\) Region and there are no clusters for this account\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-no-cluster.png)

When you have at least one cluster in an AWS Region, the **Clusters** section displays a subset of information about all the clusters for the account in that AWS Region\. In the following screenshot, there is one cluster created for this account in the selected AWS Region\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

You can expand the cluster to view more information about the cluster, such as the endpoint details, cluster and database properties, tags, and so on\. In the following screenshot, *examplecluster* is expanded to show a summary of information about the cluster\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-expanded.png)

**Topics**
+ [Creating a Cluster](#create-cluster)
+ [Modifying a Cluster](#modify-cluster)
+ [Deleting a Cluster](#delete-cluster)
+ [Rebooting a Cluster](#reboot-cluster)
+ [Resizing a Cluster](#resizing-cluster)
+ [Getting Information About Cluster Configuration](#describe-cluster)
+ [Getting an Overview of Cluster Status](#status-cluster)
+ [Taking a Snapshot of a Cluster](#snapshot-cluster)
+ [Editing the Default Disk Space Alarm](#rs-mgmt-edit-default-disk-space-alarm)
+ [Working with Cluster Performance Data](#performance-cluster)

## Creating a Cluster<a name="create-cluster"></a>

Before you create a cluster, make sure to read the clusters topics [Overview](working-with-clusters.md#working-with-clusters-overview) and [Clusters and Nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.

You can create a cluster in the AWS Management Console in two ways: 
+ If you're new to Amazon Redshift or just need a basic cluster, use **Quick launch cluster**\. With this approach, you specify only the node type, number of nodes, user name, password, and AWS Identity and Access Management \(IAM\) role to use for access\. For more information, see [Creating a Cluster by Using Quick Launch Cluster](#quick-create-cluster)\.
+ If you're an existing user or want to customize your cluster, use **Launch cluster**\. For example, use **Launch cluster** to use a specific virtual private cloud \(VPC\) or encrypt data in your cluster\. For more information, see [Creating a Cluster by Using Launch Cluster](#create-cluster-task)\.

### Creating a Cluster by Using Quick Launch Cluster<a name="quick-create-cluster"></a>

If you're new to Amazon Redshift or just need a basic cluster, use this streamlined approach\. If you're an existing user or want to customize your cluster, see [Creating a Cluster by Using Launch Cluster](#create-cluster-task)\.

**To create a cluster by using Quick launch cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.
**Important**  
If you use IAM user credentials, make sure that the user has the necessary permissions to perform the cluster operations\. For more information, see [Controlling Access to IAM Users](https://docs.aws.amazon.com/redshift/latest/mgmt/iam-redshift-user-mgmt.html) in the *Amazon Redshift Cluster Management Guide*\.

1. Choose the AWS Region where you want to create the cluster, for example **US West \(Oregon\)**\.

1. On the Amazon Redshift Dashboard, choose **Quick launch cluster**\.

1. On the Cluster specifications page, enter the following values and then choose **Launch cluster**:
   + **Node type**: Choose **dc2\.large**\.
   + **Number of compute nodes**: Keep the default value of **2**\.
   + **Master user name**: Keep the default value of **awsuser**\.
   + **Master user password** and **Confirm password**: Enter a password for the master user account\.
   + **Database port**: Accept the default value of **5439**\.
   + **Available IAM roles**: Choose **myRedshiftRole**\. 

   A confirmation page appears\. The cluster takes a few minutes to be created\. Choose **Close** to return to the list of clusters\.

1. On the **Clusters** page, choose the cluster that you just launched and review the **Cluster Status** information\. Make sure that **Cluster Status** is **available** and **Database Health** is **healthy** before you try to connect to the database\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-gsg-clusters-config-cluster-status.png)

### Creating a Cluster by Using Launch Cluster<a name="create-cluster-task"></a>

If you're an existing Amazon Redshift user or want to customize your cluster, use the following procedure to launch your cluster\. If you're new to Amazon Redshift or just need a basic cluster, see [Creating a Cluster by Using Quick Launch Cluster](#quick-create-cluster)\.

**To create a cluster by using Launch cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose **Launch Cluster**\.

1.  On the **Cluster Details** page, specify values for the following options, and then choose **Continue**\.   
**Cluster Identifier**  
 Type a unique name for your cluster\.   
Cluster identifiers must meet the following conditions:  
   + They must contain 1–63 alphanumeric characters or hyphens\.
   + Alphabetic characters must be lowercase\.
   + The first character must be a letter\.
   + They can't end with a hyphen or contain two consecutive hyphens\.
   + They must be unique for all clusters within an AWS account\.  
**Database Name**  
Type a name if you want to create a database with a custom name \(for example, **mydb**\)\. This field is optional\. A default database named *dev* is created for the cluster whether or not you specify a custom database name\.   
Database names must meet the following conditions:  
   + They must contain 1–64 alphanumeric characters\.
   + They must contain only lowercase letters\.
   + A database name can't be a reserved word\. For more information, see [Reserved Words](http://docs.aws.amazon.com/redshift/latest/dg/r_pg_keywords.html) in the *Amazon Redshift Database Developer Guide*\.  
**Database Port**  
 Type a port number through which you plan to connect from client applications to the database\. The port number must be included in the connection string when opening JDBC or ODBC connections to the databases in the cluster\.   
The port number must meet the following conditions:  
   + It must contain only numeric characters\.
   + It must fall in the range 1150–65535\. The default port is 5439\.
   + It must specify an open port that accepts inbound connections, if you are behind a firewall\.  
**Master User Name**  
 Type an account name for the master user of the database\.   
Master user names must meet the following conditions:  
   + They must contain 1–128 alphanumeric characters\.
   + The first character must be a letter\. 
   + A master user name can't be a reserved word\. For more information, see [Reserved Words](http://docs.aws.amazon.com/redshift/latest/dg/r_pg_keywords.html) in the *Amazon Redshift Database Developer Guide*\.  
**Master User Password** and **Confirm Password**  
 Type a password for the master user account, and then retype it to confirm the password\.   
The password must meet the following conditions:  
   + It must be 8–64 characters in length\.
   + It must contain at least one uppercase letter\.
   + It must contain at least one lowercase letter\. 
   + It must contain at least one number\.
   + It can be any printable ASCII character \(ASCII code 33–126\) except single quotation mark, double quotation mark, \\, /, @, or space\.

   In the following screenshot, `examplecluster` is the cluster identifier, no custom database name is specified, 5439 is the port, and `masteruser` is the master user name\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-cluster-details.png)

1.  On the **Node Configuration** page, specify values for the following options, and then choose **Continue**\.   
**Node Type**  
Choose a node type\. When you choose a node type, the page displays information that corresponds to the selected node type, such as **CPU**, **Memory**, **Storage**, and **I/O Performance**\.   
**Cluster Type**  
Choose a cluster type\. When you do, the maximum number of compute nodes for the selected node and cluster type appears for **Maximum**, and the minimum number appears for **Minimum**\.   
 If you choose **Single Node**, you have one node that shares leader and compute functionality\.   
 If you choose **Multi Node**, specify the number of compute nodes that you want for the cluster in **Number of Compute Nodes**\. 

    In the following screenshot, the **dc1\.large** node type is selected for a **Multi Node** cluster with two compute nodes\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-node-configuration.png)

1.  On the **Additional Configuration** page, specify values for the following options, and then choose **Continue**\. 

   1.  For **Provide the optional additional configuration details below**, configure the following options:   
**Cluster Parameter Group**  
 Choose a cluster parameter group to associate with the cluster\. If you don't choose one, the cluster uses the default parameter group\.   
**Encrypt Database**  
Choose whether you want to encrypt all data within the cluster and its snapshots\. If you leave the default setting, **None**, encryption is not enabled\.  If you want to enable encryption, choose whether you want to use AWS Key Management Service \(AWS KMS\) or a hardware security module \(HSM\), and then configure the related settings\. For more information about encryption in Amazon Redshift, see [Amazon Redshift Database Encryption](working-with-db-encryption.md)\.  
      + **KMS**

        Choose **KMS** if you want to enable encryption and use AWS KMS to manage your encryption key\. In **Master Key**, choose **\(default\) aws/redshift** to use a default customer master key \(CMK\) or choose another key from your AWS account\.
**Note**  
If you want to use a key from another AWS account, choose **Enter a key ARN** from **Master Key**\. Then type the Amazon Resource Name \(ARN\) for the key to use\. You must have permission to use the key\. For more information about access to keys in AWS KMS, see [Controlling Access to Your Keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

        For more information about using AWS KMS encryption keys in Amazon Redshift, see [Database Encryption for Amazon Redshift Using AWS KMS](working-with-db-encryption.md#working-with-aws-kms)\.
      + **HSM**

        Choose **HSM** if you want to enable encryption and use a hardware security module \(HSM\) to manage your encryption key\.

        If you choose **HSM**, choose values from **HSM Connection** and **HSM Client Certificate**\. These values are required for Amazon Redshift and the HSM to form a trusted connection over which the cluster key can be passed\. The HSM connection and client certificate must be set up in Amazon Redshift before you launch a cluster\. For more information about setting up HSM connections and client certificates, see [Encryption for Amazon Redshift Using Hardware Security Modules](working-with-db-encryption.md#working-with-HSM)\.

   1. For **Configure Networking Options**, you configure whether to launch your cluster in a virtual private cloud \(VPC\) or outside a VPC\. The option you choose affects the additional options available in this section\. Amazon Redshift uses the EC2\-Classic and EC2\-VPC platforms to launch clusters\. Your AWS account determines which platform or platforms are available to you for your cluster\. For more information, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\.   
**Choose a VPC**  
To launch your cluster in a virtual private cloud \(VPC\), choose the VPC you want to use\. You must have at least one Amazon Redshift subnet group set up to use VPCs\. For more information, see [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)\.   

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-networking-vpc.png)
To launch your cluster outside a VPC, choose **Not in VPC**\. This option is available only to AWS accounts that support the EC2\-Classic platform\. Otherwise, you must launch your cluster in a VPC\.   

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-networking-no-vpc.png)  
**Cluster Subnet Group**  
 Select the Amazon Redshift subnet group in which to launch the cluster\.   
This option is available only for clusters in a VPC\.  
**Publicly Accessible**  
 Choose **Yes** to enable connections to the cluster from outside of the VPC in which you launch the cluster\. Choose **No** if you want to limit connections to the cluster from only within the VPC\.   
This option is available only for clusters in a VPC\.  
**Choose a Public IP Address**  
If you set **Publicly Accessible** to **Yes**, choose **No** here to have Amazon Redshift to provide an Elastic IP \(EIP\) for the cluster\. Alternatively, choose **Yes** if you want to use an EIP that you have created and manage\. If you have Amazon Redshift create the EIP, it is managed by Amazon Redshift\.   
This option is available only for clusters in a VPC where **Publicly Accessible** is **Yes**\.  
**Elastic IP**  
 Select the EIP that you want to use to connect to the cluster from outside of the VPC\.   
This option is available only for clusters in a VPC where **Publicly Accessible** and **Choose a Public IP Address** are **Yes**\.  
**Availability Zone**  
 Choose **No Preference** to have Amazon Redshift choose the Availability Zone that the cluster is created in\. Otherwise, choose a specific Availability Zone\.   
**Enhanced VPC Routing**  
Choose **Yes** to enable enhanced VPC routing\. Enhanced VPC routing might require some additional configuration\. For more information, see [Amazon Redshift Enhanced VPC Routing](enhanced-vpc-routing.md)\. 

   1. For **Optionally, associate your cluster with one or more security groups**, specify values for the following options:  
**Cluster Security Groups**  
 Choose an Amazon Redshift security group or groups for the cluster\. By default, the chosen security group is the default security group\. For more information about cluster security groups, see [Amazon Redshift Cluster Security Groups](working-with-security-groups.md)\.   
 This option is only available if you launch your cluster in the EC2\-Classic platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-cluster-security-group.png)  
**VPC Security Groups**  
 Choose a VPC security group or groups for the cluster\. By default, the chosen security group is the default VPC security group\. For more information about VPC security groups, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.   
 This option is only available if you launch your cluster in the EC2\-VPC platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-vpc-security-group.png)

   1.  For **Optionally create a basic alarm for this cluster**, configure the following options, and then choose **Continue**:   
**Create CloudWatch Alarm**  
 Choose **Yes** if you want to create an alarm that monitors the disk usage of your cluster, and then specify values for the corresponding options\. Choose **No **if you don't want to create an alarm\.   
**Disk Usage Threshold**  
Choose a percentage of average disk usage that has been reached or exceeded at which the alarm should trigger\.  
**Use Existing Topic**  
Choose **No** if you want to create a new Amazon Simple Notification Service \(Amazon SNS\) topic for this alarm\. In the **Topic** box, edit the default name if necessary\. For **Recipients**, type the email addresses for any recipients who should receive the notification when the alarm triggers\.   

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-create-cw-alarm-new-topic.png)
 Choose **Yes** if you want to choose an existing Amazon SNS topic for this alarm, and then in the **Topic** list, choose the topic that you want to use\.   

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-create-cw-alarm-existing-topic.png)

   1. For **Optionally, select your maintenance track for this cluster**, choose **Current** or **Trailing**\.

      If you choose **Current**, your cluster is updated with the latest approved release during your maintenance window\. If you choose **Trailing**, your cluster is updated with the release that was approved previously\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-createcluster-mainttrack.png)

1. On the **Review** page, review the details of the cluster\. If everything is satisfactory, choose **Launch Cluster** to start the creation process\. Otherwise, choose **Back** to make any necessary changes, and then choose **Continue** to return to the **Review** page\.
**Note**  
 Some cluster properties, such as the values for **Database Port** and **Master User Name**, cannot be modified later\. If you need to change them, choose **Back** to change them now\. 

    The following screenshot shows a summary of various options chosen during the cluster launch process\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-review.png)

1. After you initiate the creation process, choose **Close**\. The cluster might take several minutes to be ready to use\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-create-70.png)

   You can monitor the status of the operation in the performance dashboard\.

## Modifying a Cluster<a name="modify-cluster"></a>

When you modify a cluster, changes to the following options are applied immediately:
+ **VPC Security Groups** 
+ **Publicly Accessible** 
+ **Master User Password** 
+ **Automated Snapshot Retention Period** 
+ **HSM Connection** 
+ **HSM Client Certificate** 
+ **Maintenance Window Start** 
+ **Maintenance Window End** 

 Changes to the following options take effect only after the cluster is restarted:
+ **Cluster Identifier**

  Amazon Redshift restarts the cluster automatically when you change **Cluster Identifier**\.
+ **Enhanced VPC Routing**

  Amazon Redshift restarts the cluster automatically when you change **Enhanced VPC Routing**\.
+ **Cluster Parameter Group** 

If you decrease the automated snapshot retention period, existing automated snapshots whose settings fall outside of the new retention period are deleted\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\. <a name="modify-cluster-task"></a>

**To modify a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Modify**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1. In the **Modify Cluster** window, change your cluster, and then choose **Modify**\.

   The following screenshot shows the **Modify Cluster** options for a cluster in a VPC\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-modify.png)

### Setting the Maintenance Track for a Cluster<a name="rs-mgmt-set-maintenance-track"></a>

You can set the maintenance track for a cluster with the console\. For more information , see [Choosing Cluster Maintenance Tracks](working-with-clusters.md#rs-mgmt-maintenance-tracks)\.

**To set a maintenance track for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\. 

1. Choose the **Maintenance settings** tab\.

1. Choose either **Current** or **Trailing**\.

1. Choose **Modify**\. 

### Deferring Maintenance<a name="defer-maintenance-window"></a>

If you need to reschedule your cluster’s maintenance window, you have the option to defer maintenance by up to 14 days\.<a name="defer-maintenance-task"></a>

**To defer the maintenance window**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\. 

1. Choose the **Maintenance settings** tab\.

1. Choose **Defer maintenance** and set the date and time to defer maintenance\.

1. Choose **Modify**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-defermaint.png)

## Deleting a Cluster<a name="delete-cluster"></a>

If you no longer need your cluster, you can delete it\. If you plan to provision a new cluster with the same data and configuration as the one you are deleting, you need a manual snapshot\. By using a manual snapshot, you can restore the snapshot later and resume using the cluster\. If you delete your cluster but you don't create a final manual snapshot, the cluster data is deleted\. In either case, automated snapshots are deleted after the cluster is deleted, but any manual snapshots are retained until you delete them\. You might be charged Amazon Simple Storage Service storage rates for manual snapshots, depending on the amount of storage you have available for Amazon Redshift snapshots for your clusters\. For more information, see [Shutting Down and Deleting Clusters](working-with-clusters.md#rs-mgmt-shutdown-delete-cluster)\. <a name="delete-cluster-task"></a>

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**, and then choose the cluster that you want to delete\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Delete**\.

1. In the **Delete Cluster** dialog box, do one of the following: 
   +  In **Create snapshot**, choose **Yes** to delete the cluster and take a final snapshot\. In **Snapshot name**, type a name for the final snapshot, and then choose **Delete**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-delete-final-snapshot.png)
   +  In **Create snapshot**, choose **No** to delete the cluster without taking a final snapshot, and then choose **Delete**\. 

   After you initiate the delete of the cluster, it can take several minutes for the cluster to be deleted\. You can monitor the status in the cluster list as shown in the following screenshots\. If you requested a final snapshot, **Cluster Status** shows `final-snapshot` before `deleting`\.

    The following screenshot shows the cluster with a status of `final-snapshot` before it is deleted\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-final-snapshot-status.png)

    The following screenshot shows the cluster with a status of `deleting`\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-delete-status.png)

   When the process has finished, you can verify that the cluster has been deleted because it no longer appears in the list of clusters on the **Clusters** page\.

## Rebooting a Cluster<a name="reboot-cluster"></a>

When you reboot a cluster, the cluster status is set to `rebooting` and a cluster event is created when the reboot is completed\. Any pending cluster modifications are applied at this reboot\.<a name="reboot-cluster-task"></a>

**To reboot a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to reboot\. 

1. On the **Configuration** tab of the cluster details page, choose **Cluster** and then choose **Reboot**\.

1. In the **Reboot Clusters** window, confirm that you want to reboot this cluster, and then choose **Reboot**\.

   It can take several minutes for the cluster to be available\. You can monitor the status of the reboot in the cluster list as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-reboot-status.png)

## Resizing a Cluster<a name="resizing-cluster"></a>

 When you resize a cluster, you specify a number of nodes or node type that is different from the current configuration of the cluster\. While the cluster is in the process of resizing, you cannot run any write or read/write queries on the cluster; you can run only read queries\. 

 For more information about resizing clusters, including walking through the process of resizing clusters using different approaches, see [Resizing Clusters in Amazon Redshift](rs-resize-tutorial.md)\. <a name="resize-cluster-task"></a>

**To resize a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to resize\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Resize**\.

1. In the **Resize Clusters** window, configure the resize parameters including the **Node Type**, **Cluster Type**, and **Number of Nodes**, and then choose **Resize**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-dialog.png)

   You can monitor the progress of the resize on the **Status** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status.png)

   You can cancel a resize before it's complete by choosing **cancel resize** on the cluster list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cancel-resize.png)

## Getting Information About Cluster Configuration<a name="describe-cluster"></a><a name="describe-cluster-task"></a>

**To get cluster configuration details**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to view configuration information\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, review the configuration information\. You can view information about the cluster properties, status, database, capacity, backup, audit logging, maintenance, and SSH ingestion settings\.

## Getting an Overview of Cluster Status<a name="status-cluster"></a>

The cluster **Status** tab provides a high\-level overview of the status of a cluster and a summary of events related to the cluster\. It also provides a list of Amazon CloudWatch alarms associated with the cluster\.<a name="status-cluster-task"></a>

**To get an overview of cluster status**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to view status information\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. Choose the **Status** tab\. 

   The status summary page is displayed as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-status.png)

## Taking a Snapshot of a Cluster<a name="snapshot-cluster"></a>

You can take a snapshot of your cluster from the **Configuration** tab of your cluster as shown following\. You can also take a snapshot of your cluster from the snapshots part of the Amazon Redshift console\. For more information, see [Managing Snapshots Using the Console](managing-snapshots-console.md)\.<a name="snapshot-cluster-task"></a>

**To take a snapshot of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to take a snapshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Backup**, and then choose **Take Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-backup-menu.png)

1. In the **Create Snapshot** dialog box, do the following: 

   1. In the **Cluster Identifier** box, choose the cluster that you want to take a snapshot of\.

   1. In the **Snapshot Identifier** box, type a name for the snapshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-take-snapshot.png)

1. Choose **Create**\.

   To view details about the snapshot taken and all other snapshots for your AWS account, see the snapshots section of the Amazon Redshift console\. For more information, see [Managing Snapshots Using the Console](managing-snapshots-console.md)\.

## Editing the Default Disk Space Alarm<a name="rs-mgmt-edit-default-disk-space-alarm"></a>

If you opted to create a default disk space alarm when you created your Amazon Redshift cluster, you can edit the alarm\. For example, you might want to change the percentage at which the alarm triggers, or you might want to change the duration settings\. <a name="how-to-edit-default-disk-space-alarm"></a>

**To edit the default disk space alarm**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**, and then choose the cluster associated with the alarm that you want to edit\.

1. Choose the **Status** tab\. 

1. In the **CloudWatch Alarms** section, choose the alarm that you want to edit\. 

   The default disk space alarm that was created when you launched your cluster is named **percentage\-disk\-space\-used\-default\-<*string*>\.** The *string* is randomly generated by Amazon Redshift\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-status-cw-alarm-disk-percent-used.png)

1.  In the **Edit Alarm** window, edit any values that you want to change, such as the percentage or minutes\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-status-cw-alarm-edit.png)

1.  To change the Amazon SNS topic that the alarm is associated with, do one of the following: 
   +  If you want to choose another existing topic, choose a topic from the **Send a notification to** list\. 
   +  If you want to create a new topic, choose **create topic** and specify a new topic name and the email addresses for recipients\. 

1.  Choose **Save**\. 

## Working with Cluster Performance Data<a name="performance-cluster"></a>

You can work with cluster performance data using the **Performance**, **Queries**, and **Loads** tabs\. For more information about working with cluster performance, see [Working with Performance Data in the Amazon Redshift Console](performance-metrics-console.md)\.