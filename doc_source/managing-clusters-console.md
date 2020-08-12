# Managing clusters using the console<a name="managing-clusters-console"></a>

To create, modify, resize, delete, reboot, and back up clusters, use the **Clusters** section in the Amazon Redshift console\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="managing-clusters-console-overview"></a>

**To view clusters**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\. If you don't have any clusters, choose **Create cluster** to create one\.

1. Choose the cluster name in the list to view more details about a cluster\.

## Original console<a name="managing-clusters-originalconsole"></a>

When you don't have any clusters in an AWS Region and you open the **Clusters** page, you have the option to launch a cluster\. In the following screenshot, the AWS Region is the US East \(N\. Virginia\) Region and there are no clusters for this account\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-no-cluster.png)

When you have at least one cluster in an AWS Region, the **Clusters** section displays a subset of information about all the clusters for the account in that AWS Region\. In the following screenshot, there is one cluster created for this account in the selected AWS Region\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

You can expand the cluster to view more information about the cluster, such as the endpoint details, cluster and database properties, tags, and so on\. In the following screenshot, *examplecluster* is expanded to show a summary of information about the cluster\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-expanded.png)

**Topics**
+ [Creating a cluster](#create-cluster)
+ [Modifying a cluster](#modify-cluster)
+ [Deleting a cluster](#delete-cluster)
+ [Rebooting a cluster](#reboot-cluster)
+ [Resizing a cluster](#resizing-cluster)
+ [Upgrading the release version of a cluster](#upgrade-release-version-cluster)
+ [Getting information about cluster configuration](#describe-cluster)
+ [Getting an overview of cluster status](#status-cluster)
+ [Creating a snapshot of a cluster](#snapshot-cluster)
+ [Creating or editing a disk space alarm](#rs-mgmt-edit-default-disk-space-alarm)
+ [Working with cluster performance data](#performance-cluster)

## Creating a cluster<a name="create-cluster"></a>

Before you create a cluster, read [Overview of Amazon Redshift clusters](working-with-clusters.md#working-with-clusters-overview) and [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-create-console"></a>

One way to learn about creating a cluster is to create a cluster using the console\. 

**To create a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\. 

1. Choose **Create cluster** to create a cluster\. 

1. Follow the instructions on the console page to enter the properties for **Cluster configuration**\. 

   Choose one of the following methods to size your cluster:
**Note**  
The following step describes an Amazon Redshift console that is running in an AWS Region that supports RA3 node types\. For a list of AWS Regions that support RA3 node types, see [Overview of RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-ra3-node-types) in the *Amazon Redshift Cluster Management Guide*\. 
   + If your AWS Region supports RA3 node types, choose either **Production** or **Free trial** to answer the question **What are you planning to use this cluster for?** 

     If your organization is eligible, you might be able to create a cluster under the Amazon Redshift free trial program\. To do this, choose **Free trial** to create a configuration with the dc2\.large node type\. For more information about choosing a free trial, see [Amazon Redshift free trial](http://aws.amazon.com/redshift/free-trial/)\. 
   + If you don't know how large to size your cluster, choose **Help me choose**\. Doing this starts a sizing calculator that asks you questions about the size and query characteristics of the data that you plan to store in your data warehouse\. 

     If you know the required size of your cluster \(that is, the node type and number of nodes\), choose **I'll choose**\. Then choose the **Node type** and number of **Nodes** to size your cluster for the proof of concept\.

1. Follow the instructions on the console page to enter the properties for **Cluster details**\. 
**Note**  
If you are behind a firewall, the database port must be an open port that accepts inbound connections\. 

1. \(Optional\) Follow the instructions on the console page to enter properties for **Cluster permissions**\. Provide cluster permissions if your cluster needs to access other AWS services for you, for example to load data from Amazon S3\. 

1. Choose **Create cluster** to create the cluster\. The cluster might take several minutes to be ready to use\.

#### Additional configurations<a name="cluster-create-console-configuration"></a>

When you create a cluster, you can specify additional properties to customize it\. You can find more details about some of these properties in the following list\. 

**Virtual private cloud \(VPC\)**  
Choose a VPC that has a subnet group\. After the cluster is created, the subnet group can't be changed\. 

**Parameter groups**  
Choose a cluster parameter group to associate with the cluster\. If you don't choose one, the cluster uses the default parameter group\. 

**Encryption**  
Choose whether you want to encrypt all data within the cluster and its snapshots\. If you leave the default setting, **None**, encryption is not enabled\. If you want to enable encryption, choose whether you want to use AWS Key Management Service \(AWS KMS\) or a hardware security module \(HSM\), and then configure the related settings\. For more information about encryption in Amazon Redshift, see [Amazon Redshift database encryption](working-with-db-encryption.md)\.  
+ **KMS**

  Choose **KMS** if you want to enable encryption and use AWS KMS to manage your encryption key\. In **Master Key**, choose **\(default\) aws/redshift** to use a default customer master key \(CMK\) or choose another key from your AWS account\.
**Note**  
If you want to use a key from another AWS account, choose **Enter a key ARN** from **Master Key**\. Then type the Amazon Resource Name \(ARN\) for the key to use\. You must have permission to use the key\. For more information about access to keys in AWS KMS, see [Controlling access to your keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

  For more information about using AWS KMS encryption keys in Amazon Redshift, see [Database encryption for Amazon Redshift using AWS KMS](working-with-db-encryption.md#working-with-aws-kms)\.
+ **HSM**

  Choose **HSM** if you want to enable encryption and use a hardware security module \(HSM\) to manage your encryption key\.

  If you choose **HSM**, choose values from **HSM Connection** and **HSM Client Certificate**\. These values are required for Amazon Redshift and the HSM to form a trusted connection over which the cluster key can be passed\. The HSM connection and client certificate must be set up in Amazon Redshift before you launch a cluster\. For more information about setting up HSM connections and client certificates, see [Encryption for Amazon Redshift using hardware security modules](working-with-db-encryption.md#working-with-HSM)\.

**Maintenance track**  
You can choose whether the cluster version used is the **Current**, **Trailing**, or sometimes **Preview** track\. 

**Monitoring**  
You can choose whether to create CloudWatch alarms\. 

**Configure cross\-region snapshot**  
You can choose whether to enable cross\-Region snapshots\. 

### Original console<a name="create-cluster-originalconsole"></a>

You can create a cluster in the AWS Management Console in two ways: 
+ If you're new to Amazon Redshift or just need a basic cluster, use **Quick launch cluster**\. With this approach, you specify only the node type, number of nodes, user name, password, and AWS Identity and Access Management \(IAM\) role to use for access\. For more information, see [Creating a cluster by using quick launch cluster](#quick-create-cluster)\.
+ If you're an existing user or want to customize your cluster, use **Launch cluster**\. For example, use **Launch cluster** to use a specific virtual private cloud \(VPC\) or encrypt data in your cluster\. For more information, see [Creating a cluster by using a launch cluster](#create-cluster-task)\.

#### Creating a cluster by using quick launch cluster<a name="quick-create-cluster"></a>

If you're new to Amazon Redshift or just need a basic cluster, use this streamlined approach\. If you're an existing user or want to customize your cluster, see [Creating a cluster by using a launch cluster](#create-cluster-task)\.

**To create a cluster by using a quick launch cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.
**Important**  
If you use IAM user credentials, make sure that the user has the necessary permissions to perform the cluster operations\. For more information, see [Controlling access to IAM users](https://docs.aws.amazon.com/redshift/latest/mgmt/iam-redshift-user-mgmt.html) in the *Amazon Redshift Cluster Management Guide*\.

1. Choose the AWS Region where you want to create the cluster, for example **US West \(Oregon\)**\.

1. On the Amazon Redshift dashboard, choose **Quick launch cluster**\.

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

#### Creating a cluster by using a launch cluster<a name="create-cluster-task"></a>

If you're an existing Amazon Redshift user or want to customize your cluster, use the following procedure to launch your cluster\. If you're new to Amazon Redshift or just need a basic cluster, see [Creating a cluster by using quick launch cluster](#quick-create-cluster)\.

**To create a cluster by using a launch cluster**

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
It can use any ASCII characters with ASCII codes 33–126, except ' \(single quote\), " \(double quote\), \\, /, or @\. 

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
Choose whether you want to encrypt all data within the cluster and its snapshots\. If you leave the default setting, **None**, encryption is not enabled\.  If you want to enable encryption, choose whether you want to use AWS Key Management Service \(AWS KMS\) or a hardware security module \(HSM\), and then configure the related settings\. For more information about encryption in Amazon Redshift, see [Amazon Redshift database encryption](working-with-db-encryption.md)\.  
      + **KMS**

        Choose **KMS** if you want to enable encryption and use AWS KMS to manage your encryption key\. In **Master Key**, choose **\(default\) aws/redshift** to use a default customer master key \(CMK\) or choose another key from your AWS account\.
**Note**  
If you want to use a key from another AWS account, choose **Enter a key ARN** from **Master Key**\. Then type the Amazon Resource Name \(ARN\) for the key to use\. You must have permission to use the key\. For more information about access to keys in AWS KMS, see [Controlling access to your keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

        For more information about using AWS KMS encryption keys in Amazon Redshift, see [Database encryption for Amazon Redshift using AWS KMS](working-with-db-encryption.md#working-with-aws-kms)\.
      + **HSM**

        Choose **HSM** if you want to enable encryption and use a hardware security module \(HSM\) to manage your encryption key\.

        If you choose **HSM**, choose values from **HSM Connection** and **HSM Client Certificate**\. These values are required for Amazon Redshift and the HSM to form a trusted connection over which the cluster key can be passed\. The HSM connection and client certificate must be set up in Amazon Redshift before you launch a cluster\. For more information about setting up HSM connections and client certificates, see [Encryption for Amazon Redshift using hardware security modules](working-with-db-encryption.md#working-with-HSM)\.

   1. For **Configure Networking Options**, you configure whether to launch your cluster in a virtual private cloud \(VPC\) or outside a VPC\. The option you choose affects the additional options available in this section\. Amazon Redshift uses the EC2\-VPC and EC2\-Classic platforms to launch clusters\. Your AWS account determines which platform or platforms are available to you for your cluster\. For more information, see [Supported platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\.   
**Choose a VPC**  
      + To launch your cluster in a virtual private cloud \(VPC\), choose the VPC you want to use\. You must have at least one Amazon Redshift subnet group set up to use VPCs\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-networking-vpc.png)
      + To launch your cluster outside a VPC, choose **Not in VPC**\. This option is available only to AWS accounts that support the EC2\-Classic platform\. Otherwise, you must launch your cluster in a VPC\.   
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
Choose **Yes** to enable enhanced VPC routing\. Enhanced VPC routing might require some additional configuration\. For more information, see [Amazon Redshift enhanced VPC routing](enhanced-vpc-routing.md)\. 

   1. For **Optionally, associate your cluster with one or more security groups**, specify values for the following options:  
**VPC Security Groups**  
 Choose a VPC security group or groups for the cluster\. By default, the chosen security group is the default VPC security group\. For more information about VPC security groups, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.   
 This option is only available if you launch your cluster in the EC2\-VPC platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-vpc-security-group.png)  
**Cluster Security Groups**  
 Choose an Amazon Redshift security group or groups for the cluster\. By default, the chosen security group is the default security group\. For more information about cluster security groups, see [Amazon Redshift cluster security groups](working-with-security-groups.md)\.   
 This option is only available if you launch your cluster in the EC2\-Classic platform\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-launch-addl-config-cluster-security-group.png)

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

## Modifying a cluster<a name="modify-cluster"></a>

When you modify a cluster, changes to the following options are applied immediately:
+ **VPC Security Groups** 
+ **Publicly Accessible** 
+ **Master User Password** 
+ **HSM Connection** 
+ **HSM Client Certificate** 
+ **Maintenance settings** 
+ **Snapshot preferences** 

 Changes to the following options take effect only after the cluster is restarted:
+ **Cluster Identifier**

  Amazon Redshift restarts the cluster automatically when you change **Cluster Identifier**\.
+ **Enhanced VPC Routing**

  Amazon Redshift restarts the cluster automatically when you change **Enhanced VPC Routing**\.
+ **Cluster Parameter Group** 

If you decrease the automated snapshot retention period, existing automated snapshots whose settings fall outside of the new retention period are deleted\. For more information, see [Amazon Redshift snapshots](working-with-snapshots.md)\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-modify"></a>

**To modify a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to modify\. 

1. For **Actions**, choose **Modify**\. The **Modify cluster** page appears\.

1. Update the cluster properties\. Some of the properties you can modify are: 
   + Cluster identifier
   + VPC security groups
   + Enhanced VPC routing
   + Parameter groups
   + Encryption
   + Maintenance window
   + Maintenance track
   + Defer maintenance window
   + Snapshot retention

1. Choose **Modify cluster**\.

### Original console<a name="modify-cluster-task-originalconsole"></a><a name="modify-cluster-task"></a>

**To modify a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Modify**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1. In the **Modify Cluster** window, change your cluster, and then choose **Modify**\. The resulting window shows the options available to modify a cluster\. Including tabs with options to update **Cluster settings**, **Maintenance settings**, and **Snapshot settings**\.

#### Setting the maintenance track for a cluster<a name="rs-mgmt-set-maintenance-track"></a>

You can set the maintenance track for a cluster with the console\. For more information, see [Choosing cluster maintenance tracks](working-with-clusters.md#rs-mgmt-maintenance-tracks)\.

**To set a maintenance track for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\. 

1. Choose the **Maintenance settings** tab\.

1. Choose either **Current** or **Trailing**\.

1. Choose **Modify**\. 

#### Deferring maintenance<a name="defer-maintenance-window"></a>

If you need to reschedule your cluster's maintenance window, you have the option to defer maintenance by up to 45 days\.<a name="defer-maintenance-task"></a>

**To defer the maintenance window**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to modify\. 

1. Choose the **Maintenance settings** tab\.

1. Choose **Defer maintenance** and set the date and time to defer maintenance\.

1. Choose **Modify**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-defermaint.png)

## Deleting a cluster<a name="delete-cluster"></a>

If you no longer need your cluster, you can delete it\. If you plan to provision a new cluster with the same data and configuration as the one you are deleting, you need a manual snapshot\. By using a manual snapshot, you can restore the snapshot later and resume using the cluster\. If you delete your cluster but you don't create a final manual snapshot, the cluster data is deleted\. In either case, automated snapshots are deleted after the cluster is deleted, but any manual snapshots are retained until you delete them\. You might be charged Amazon Simple Storage Service storage rates for manual snapshots, depending on the amount of storage you have available for Amazon Redshift snapshots for your clusters\. For more information, see [Shutting down and deleting clusters](managing-cluster-operations.md#rs-mgmt-shutdown-delete-cluster)\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-delete"></a>

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to delete\. 

1. For **Actions**, choose **Delete**\. The **Delete cluster** page appears\. 

1. Choose **Delete cluster**\. 

### Original console<a name="cluster-delete-originalconsole"></a><a name="delete-cluster-task"></a>

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**, and then choose the cluster that you want to delete\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Delete**\.

1. In the **Delete Cluster** dialog box, do one of the following: 
   +  In **Create snapshot**, choose **Yes** to delete the cluster and take a final snapshot\. In **Snapshot name**, type a name for the final snapshot, and then choose **Delete**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-delete-final-snapshot.png)
   +  In **Create snapshot**, choose **No** to delete the cluster without creating a final snapshot, and then choose **Delete**\. 

   After you initiate the delete of the cluster, it can take several minutes for the cluster to be deleted\. You can monitor the status in the cluster list as shown in the following screenshots\. If you requested a final snapshot, **Cluster Status** shows `final-snapshot` before `deleting`\.

    The following screenshot shows the cluster with a status of `final-snapshot` before it is deleted\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-final-snapshot-status.png)

    The following screenshot shows the cluster with a status of `deleting`\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-delete-status.png)

   When the process has finished, you can verify that the cluster has been deleted because it no longer appears in the list of clusters on the **Clusters** page\.

## Rebooting a cluster<a name="reboot-cluster"></a>

When you reboot a cluster, the cluster status is set to `rebooting` and a cluster event is created when the reboot is completed\. Any pending cluster modifications are applied at this reboot\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-reboot"></a>

**To reboot a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to reboot\. 

1. For **Actions**, choose **Reboot cluster**\. The **Reboot cluster** page appears\.

1. Choose **Reboot cluster**\. 

### Original console<a name="cluster-reboot-originalconsole"></a><a name="reboot-cluster-task"></a>

**To reboot a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to reboot\. 

1. On the **Configuration** tab of the cluster details page, choose **Cluster** and then choose **Reboot**\.

1. In the **Reboot Clusters** window, confirm that you want to reboot this cluster, and then choose **Reboot**\.

   It can take several minutes for the cluster to be available\. You can monitor the status of the reboot in the cluster list as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-reboot-status.png)

## Resizing a cluster<a name="resizing-cluster"></a>

 When you resize a cluster, you specify a number of nodes or node type that is different from the current configuration of the cluster\. While the cluster is in the process of resizing, you cannot run any write or read/write queries on the cluster; you can run only read queries\. 

 For more information about resizing clusters, including walking through the process of resizing clusters using different approaches, see [Resizing clusters in Amazon Redshift](managing-cluster-operations.md#rs-resize-tutorial)\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-reresize"></a>

**To resize a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to resize\. 

1. For **Actions**, choose **Resize**\. The **Resize cluster** page appears\.

1. Follow the instructions on the page\. You can resize the cluster now, once at a specific time, or increase and decrease the size of your cluster on a schedule\.

1. Depending on your choices, choose **Resize now** or **Schedule resize**\. 

### Original console<a name="cluster-resize-originalconsole"></a><a name="resize-cluster-task"></a>

**To resize a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to resize\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Cluster**, and then choose **Resize**\.

1. In the **Resize Clusters** window, configure the resize parameters including the **Node Type**, **Cluster Type**, and **Number of Nodes**, and then choose **Resize**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-dialog.png)

   You can monitor the progress of the resize on the **Status** tab\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status.png)

   You can cancel a classic resize before it's complete by choosing **cancel resize** on the cluster list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cancel-resize.png)

## Upgrading the release version of a cluster<a name="upgrade-release-version-cluster"></a>

You can upgrade the release maintenance version of a cluster that has a **Release Status** value of **New release available**\. When you upgrade the maintenance version, you can choose to upgrade immediately or upgrade in the next maintenance window\.

**Important**  
If you upgrade immediately, your cluster is offline until the upgrade completes\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-upgrade"></a>

**To upgrade a cluster to a new release version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to upgrade\. 

1. For **Actions**, choose **Upgrade cluster version**\. The **Upgrade cluster version** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Upgrade cluster version**\. 

### Original console<a name="cluster-upgrade-originalconsole"></a><a name="upgrade-release-version-cluster-task"></a>

**To upgrade your cluster to a new release version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster that you want to upgrade\. 

1. On the **Clusters** tab, choose **Cluster**, and then choose **Upgrade release version**\.

1. In the **Upgrade** window, you can view version numbers of the **Current release** and **New release**, a link to details about the [Cluster version history](rs-mgmt-cluster-version-notes.md), and the **Maintenance window schedule**\. If you choose **Yes, Upgrade now**, you must acknowledge that the cluster is offline during the upgrade\. Otherwise, you can **Never mind, upgrade in my maintenance window**\.

   When the upgrade completes, you can see the new status in the **Release status** column\.

## Getting information about cluster configuration<a name="describe-cluster"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-describe"></a>

**To display information about a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **Maintenance and monitoring**, **Backup**, **Properties**, and **Schedule** tabs\.

1. Choose each tab to view more details\. 

### Original console<a name="cluster-describe-originalconsole"></a><a name="describe-cluster-task"></a>

**To get cluster configuration details**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to view configuration information\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, review the configuration information\. You can view information about the cluster properties, status, database, capacity, backup, audit logging, maintenance, and SSH ingestion settings\.

## Getting an overview of cluster status<a name="status-cluster"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-status"></a>

**To view the status of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. View the status of the cluster in the **Status** column\. 

### Original console<a name="cluster-status-originalconsole"></a>

The cluster **Status** tab provides a high\-level overview of the status of a cluster and a summary of events related to the cluster\. It also provides a list of Amazon CloudWatch alarms associated with the cluster\.<a name="status-cluster-task"></a>

**To get an overview of cluster status**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to view status information\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. Choose the **Status** tab\. 

   The status summary page is displayed as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-status.png)

## Creating a snapshot of a cluster<a name="snapshot-cluster"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-snapshot"></a>

**To create a snapshot of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster for which to create a snapshot\. 

1. For **Actions**, choose **Create snapshot**\. The **Create snapshot** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Create snapshot**\.

### Original console<a name="cluster-snapshot-originalconsole"></a>

You can create a snapshot of your cluster from the **Configuration** tab of your cluster as shown following\. You can also create a snapshot of your cluster from the snapshots part of the Amazon Redshift console\. For more information, see [Managing snapshots using the console](managing-snapshots-console.md)\.<a name="snapshot-cluster-task"></a>

**To create a snapshot of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Clusters**, and then choose the cluster for which you want to take a snapshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1. On the **Configuration** tab of the cluster details page, choose **Backup**, and then choose **Take Snapshot**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-backup-menu.png)

1. In the **Create Snapshot** dialog box, do the following: 

   1. In the **Cluster Identifier** box, choose the cluster that you want to take a snapshot of\.

   1. In the **Snapshot Identifier** box, enter a name for the snapshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-take-snapshot.png)

1. Choose **Create**\.

   To view details about the snapshot taken and all other snapshots for your AWS account, see the snapshots section of the Amazon Redshift console\. For more information, see [Managing snapshots using the console](managing-snapshots-console.md)\.

## Creating or editing a disk space alarm<a name="rs-mgmt-edit-default-disk-space-alarm"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-default-disk-space-alarm"></a>

**To create a disk space usage alarm for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **ALARMS**\. 

1. For **Actions**, choose **Create alarm**\. The **Create alarm** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Create alarm**\.

### Original console<a name="cluster-default-disk-space-alarm-originalconsole"></a>

If you created a default disk space alarm when you created your Amazon Redshift cluster, you can edit the alarm\. For example, you might want to change the percentage at which the alarm triggers\. Or you might want to change the duration settings\. <a name="how-to-edit-default-disk-space-alarm"></a>

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

## Working with cluster performance data<a name="performance-cluster"></a>

In the new console, you can work with cluster performance on the **Cluster performance** tab of the cluster details page\. 

In the original console, you can work with cluster performance data using the **Performance**, **Queries**, and **Loads** tabs\. For more information about working with cluster performance, see [Working with performance data in the Amazon Redshift console](performance-metrics-console.md)\.