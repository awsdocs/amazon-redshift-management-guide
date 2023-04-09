# Managing clusters using the console<a name="managing-clusters-console"></a>

To create, modify, resize, delete, reboot, and back up clusters, use the **Clusters** section in the Amazon Redshift console\. 

**To view clusters**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\. If you don't have any clusters, choose **Create cluster** to create one\.

1. Choose the cluster name in the list to view more details about a cluster\.

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

**To create a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\. 

1. Choose **Create cluster** to create a cluster\. 

1. Follow the instructions on the console page to enter the properties for **Cluster configuration**\. 

   The following step describes an Amazon Redshift console that is running in an AWS Region that supports RA3 node types\. For a list of AWS Regions that support RA3 node types, see [Overview of RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-ra3-node-types) in the *Amazon Redshift Management Guide*\. 

   If you don't know how large to size your cluster, choose **Help me choose**\. Doing this starts a sizing calculator that asks you questions about the size and query characteristics of the data that you plan to store in your data warehouse\. If you know the required size of your cluster \(that is, the node type and number of nodes\), choose **I'll choose**\. Then choose the **Node type** and number of **Nodes** to size your cluster for the proof of concept\.
**Note**  
If your organization is eligible and your cluster is being created in an AWS Region where Amazon Redshift Serverless is unavailable, you might be able to create a cluster under the Amazon Redshift free trial program\. Choose either **Production** or **Free trial** to answer the question **What are you planning to use this cluster for?** When you choose **Free trial**, you create a configuration with the dc2\.large node type\. For more information about choosing a free trial, see [Amazon Redshift free trial](http://aws.amazon.com/redshift/free-trial/)\.   For a list of AWS Regions where Amazon Redshift Serverless is available, see the endpoints listed for the [Redshift Serverless API ](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\.  

1. Follow the instructions on the console page to enter the properties for **Cluster details**\. 
**Note**  
If you are behind a firewall, the database port must be an open port that accepts inbound connections\. 

1. \(Optional\) Follow the instructions on the console page to enter properties for **Cluster permissions**\. Provide cluster permissions if your cluster needs to access other AWS services for you, for example to load data from Amazon S3\. 

1. Choose **Create cluster** to create the cluster\. The cluster might take several minutes to be ready to use\.

### Additional configurations<a name="cluster-create-console-configuration"></a>

When you create a cluster, you can specify additional properties to customize it\. You can find more details about some of these properties in the following list\. 

**Virtual private cloud \(VPC\)**  
Choose a VPC that has a subnet group\. After the cluster is created, the subnet group can't be changed\. 

**Parameter groups**  
Choose a cluster parameter group to associate with the cluster\. If you don't choose one, the cluster uses the default parameter group\. 

**Encryption**  
Choose whether you want to encrypt all data within the cluster and its snapshots\. If you leave the default setting, **None**, encryption is not enabled\. If you want to enable encryption, choose whether you want to use AWS Key Management Service \(AWS KMS\) or a hardware security module \(HSM\), and then configure the related settings\. For more information about encryption in Amazon Redshift, see [Amazon Redshift database encryption](working-with-db-encryption.md)\.  
+ **KMS**

  Choose **Use AWS Key Management Service \(AWS KMS\)** if you want to enable encryption and use AWS KMS to manage your encryption key\. Also, choose the key to use\. You can choose a default key, a key from the current account, or a key from a different account\. 
**Note**  
If you want to use a key from another AWS account,  then enter the Amazon Resource Name \(ARN\) for the key to use\. You must have permission to use the key\. For more information about access to keys in AWS KMS, see [Controlling access to your keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

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

**Automated snapshot retention period**  
You can choose the number of days to retain these snapshots within 35 days\. If the node type is DC2 or DS2, you can choose zero \(0\) days to not create automated snapshots\.

**Manual snapshot retention period**  
You can choose the number of days or `Indefinitely` to retain these snapshots\. 

## Modifying a cluster<a name="modify-cluster"></a>

When you modify a cluster, changes to the following options are applied immediately:
+ **VPC security groups** 
+ **Publicly accessible** 
+ **Admin user password** 
+ **HSM Connection** 
+ **HSM Client Certificate** 
+ **Maintenance detail** 
+ **Snapshot preferences** 

 Changes to the following options take effect only after the cluster is restarted:
+ **Cluster identifier**

  Amazon Redshift restarts the cluster automatically when you change **Cluster identifier**\.
+ **Enhanced VPC routing**

  Amazon Redshift restarts the cluster automatically when you change **Enhanced VPC routing**\.
+ **Cluster parameter group** 

If you decrease the automated snapshot retention period, existing automated snapshots whose settings fall outside of the new retention period are deleted\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\. 

For more information about cluster properties, see [Additional configurations](#cluster-create-console-configuration)\. 

**To modify a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster to modify\. 

1. Choose **Edit**\. The **Edit cluster** page appears\.

1. Update the cluster properties\. Some of the properties you can modify are: 
   + Cluster identifier
   + Snapshot retention
   + Cluster relocation

   To edit settings for **Network and security**, **Maintenance**, and **Database configurations**, the console provides links to the the appropriate cluster details tab\.

1. Choose **Save changes**\.

## Deleting a cluster<a name="delete-cluster"></a>

If you no longer need your cluster, you can delete it\. If you plan to provision a new cluster with the same data and configuration as the one you are deleting, you need a manual snapshot\. By using a manual snapshot, you can restore the snapshot later and resume using the cluster\. If you delete your cluster but you don't create a final manual snapshot, the cluster data is deleted\. In either case, automated snapshots are deleted after the cluster is deleted, but any manual snapshots are retained until you delete them\. You might be charged Amazon Simple Storage Service storage rates for manual snapshots, depending on the amount of storage you have available for Amazon Redshift snapshots for your clusters\. For more information, see [Shutting down and deleting clusters](managing-cluster-operations.md#rs-mgmt-shutdown-delete-cluster)\. 

**To delete a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\.

1. Choose the cluster to delete\. 

1. For **Actions**, choose **Delete**\. The **Delete cluster** page appears\. 

1. Choose **Delete cluster**\. 

## Rebooting a cluster<a name="reboot-cluster"></a>

When you reboot a cluster, the cluster status is set to `rebooting` and a cluster event is created when the reboot is completed\. Any pending cluster modifications are applied at this reboot\.

**To reboot a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster to reboot\. 

1. For **Actions**, choose **Reboot cluster**\. The **Reboot cluster** page appears\.

1. Choose **Reboot cluster**\. 

## Resizing a cluster<a name="resizing-cluster"></a>

 When you resize a cluster, you specify a number of nodes or node type that is different from the current configuration of the cluster\. While the cluster is in the process of resizing, you cannot run any write or read/write queries on the cluster; you can run only read queries\. 

 For more information about resizing clusters, including walking through the process of resizing clusters using different approaches, see [Resizing clusters in Amazon Redshift](managing-cluster-operations.md#rs-resize-tutorial)\. 

**To resize a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster to resize\. 

1. For **Actions**, choose **Resize**\. The **Resize cluster** page appears\.

1. Follow the instructions on the page\. You can resize the cluster now, once at a specific time, or increase and decrease the size of your cluster on a schedule\.

1. Depending on your choices, choose **Resize now** or **Schedule resize**\. 

If you have reserved nodes, for example DS2 reserved nodes, you can upgrade to RA3 reserved nodes\. You can do this when you use the console to restore from a snapshot or to perform an elastic resize\. You can use the console to guide you through this process\. For more information about upgrading to RA3 nodes, see [Upgrading to RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-upgrading-to-ra3)\. 

## Upgrading the release version of a cluster<a name="upgrade-release-version-cluster"></a>

You can upgrade the release maintenance version of a cluster that has a **Release Status** value of **New release available**\. When you upgrade the maintenance version, you can choose to upgrade immediately or upgrade in the next maintenance window\.

**Important**  
If you upgrade immediately, your cluster is offline until the upgrade completes\.

**To upgrade a cluster to a new release version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster to upgrade\. 

1. For **Actions**, choose **Upgrade cluster version**\. The **Upgrade cluster version** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Upgrade cluster version**\. 

## Getting information about cluster configuration<a name="describe-cluster"></a>

**To display information about a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, which can include **Cluster performance**, **Query monitoring**, **Databases**, **Datashares**, **Schedules**, **Maintenance**, and **Properties** tabs\.

1. Choose each tab to view more details\. 

## Getting an overview of cluster status<a name="status-cluster"></a>

**To view the status of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. View the status of the cluster in the **Status** column\. 

## Creating a snapshot of a cluster<a name="snapshot-cluster"></a>

**To create a snapshot of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster for which to create a snapshot\. 

1. For **Actions**, choose **Create snapshot**\. The **Create snapshot** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Create snapshot**\.

## Creating or editing a disk space alarm<a name="rs-mgmt-edit-default-disk-space-alarm"></a>

**To create a disk space usage alarm for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Alarms**\. 

1. For **Actions**, choose **Create alarm**\. The **Create alarm** page appears\.

1. Follow the instructions on the page\. 

1. Choose **Create alarm**\.

## Working with cluster performance data<a name="performance-cluster"></a>

In the console, you can work with cluster performance on the **Cluster performance** tab of the cluster details page\. 