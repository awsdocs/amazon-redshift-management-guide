# Amazon Redshift Clusters<a name="working-with-clusters"></a>

In the following sections, you can learn the basics of creating a data warehouse by launching a set of compute nodes, called an Amazon Redshift* cluster\.*

**Topics**
+ [Overview](#working-with-clusters-overview)
+ [Resizing a Cluster](#cluster-resize-intro)
+ [Supported Platforms to Launch Your Cluster](#cluster-platforms)
+ [Regions and Availability Zone Considerations](#az-considerations)
+ [Cluster Maintenance](#rs-cluster-maintenance)
+ [Default Disk Space Alarm](#rs-clusters-default-disk-usage-alarm)
+ [Renaming Clusters](#rs-mgmt-rename-cluster)
+ [Shutting Down and Deleting Clusters](#rs-mgmt-shutdown-delete-cluster)
+ [Cluster Status](#rs-mgmt-cluster-status)
+ [Managing Clusters Using the Console](managing-clusters-console.md)
+ [Managing Clusters Using the AWS SDK for Java](managing-clusters-java.md)
+ [Manage Clusters Using the Amazon Redshift CLI and API](manage-clusters-api-cli.md)
+ [Managing Clusters in a VPC](managing-clusters-vpc.md)
+ [Cluster Version History](rs-mgmt-cluster-version-notes.md)

## Overview<a name="working-with-clusters-overview"></a>

An Amazon Redshift data warehouse is a collection of computing resources called *nodes*, which are organized into a group called a *cluster*\. Each cluster runs an Amazon Redshift engine and contains one or more databases\. 

**Note**  
At this time, Amazon Redshift version 1\.0 engine is available\. However, as the engine is updated, multiple Amazon Redshift engine versions might be available for selection\. 

### Clusters and Nodes in Amazon Redshift<a name="rs-about-clusters-and-nodes"></a>

An Amazon Redshift cluster consists of nodes\. Each cluster has a leader node and one or more compute nodes\. The *leader node* receives queries from client applications, parses the queries, and develops query execution plans\. The leader node then coordinates the parallel execution of these plans with the compute nodes and aggregates the intermediate results from these nodes\. It then finally returns the results back to the client applications\. 

*Compute nodes* execute the query execution plans and transmit data among themselves to serve these queries\. The intermediate results are sent to the leader node for aggregation before being sent back to the client applications\. For more information about leader nodes and compute nodes, see [Data Warehouse System Architecture](https://docs.aws.amazon.com/redshift/latest/dg/c_high_level_system_architecture.html) in the *Amazon Redshift Database Developer Guide*\. 


|  | 
| --- |
|  When you create a cluster on the Amazon Redshift console \([https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\), you can get a recommendation of your cluster configuration based on the size of your data and number of nodes\. To use this sizing calculator, look for **Calculate the best configuration for your needs** or **Find the best cluster configuration for your needs**\.   | 

When you launch a cluster, one option you specify is the node type\. The node type determines the CPU, RAM, storage capacity, and storage drive type for each node\.  

RA3 node types enable you to scale storage and compute independently to fit your needs\. You pay separately for the amount of compute and Amazon Redshift–managed storage that you use\. You launch clusters that use the RA3 node types in a virtual private cloud \(VPC\)\. You can't launch RA3 clusters in EC2\-Classic\. For more information, see [Creating a Cluster in a VPC](getting-started-cluster-in-vpc.md)\. 

DS2 node types are optimized for large data workloads and use hard disk drive \(HDD\) storage\. 

DC2 node types, and the earlier generation DC1 node types, are optimized for performance\-intensive workloads\. Because they use solid state drive \(SSD\) storage, the DC node types deliver much faster I/O compared to DS node types, but provide less storage space\. You launch clusters that use the DC2 node types in a virtual private cloud \(VPC\)\. You can't launch DC2 clusters in EC2\-classic mode\. For more information, see [Creating a Cluster in a VPC](getting-started-cluster-in-vpc.md)\. 

The node type that you choose depends heavily on three things:
+ The amount of data you import into Amazon Redshift
+ The complexity of the queries and operations that you run in the database
+ The needs of downstream systems that depend on the results from those queries and operations

Node types are available in different sizes\. Node size and the number of nodes determine the total storage for a cluster\. For more information, see [Node Type Details](#rs-node-type-info)

Some node types allow one node \(single\-node\) or two or more nodes \(multi\-node\)\. The minimum for 8xlarge and 16xlarge clusters is two nodes\. On a single\-node cluster, the node is shared for leader and compute functionality\. On a multi\-node cluster, the leader node is separate from the compute nodes\. 

 Amazon Redshift applies quotas to resources for each AWS account in each region\. A *quota* restricts the number of resources that your account can create for a given resource type, such as nodes or snapshots, within a region\. For more information about the default quotas that apply to Amazon Redshift resources, see [Amazon Redshift Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift) in the *Amazon Web Services General Reference*\. To request an increase, submit an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

The cost of your cluster depends on the region, node type, number of nodes, and whether the nodes are reserved in advance\. For more information about the cost of nodes, see the [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/) page\. 

#### Overview of RA3 Node Types<a name="rs-ra3-node-type"></a>

**Important**  
You can use RA3 node types only with cluster version 1\.0\.11420 or later\. You can view the version of an existing cluster with the Amazon Redshift console\. For more information, see [Determining the Cluster Maintenance Version](#cluster-maintenance-version)\.   
To use RA3 node types, your AWS Region must support RA3\. For more information, see [RA3 Node Type Availability in AWS Regions](#ra3-regions)\.   
Make sure that you use the new Amazon Redshift console when working with RA3 node types\. The new console provides a sizing calculator that supports RA3 configurations\. The original console doesn't support all RA3 operations\.   
In addition, to use RA3 node types with Amazon Redshift operations that use the maintenance track, the maintenance track value must be set to a cluster version that supports RA3\. For more information on maintenance tracks, see [Choosing Cluster Maintenance Tracks](#rs-mgmt-maintenance-tracks)\. 

Consider choosing RA3 node types in these cases: 
+ If you need the flexibility to scale compute separate from storage\.
+ If you use Amazon Redshift for your operational analytics uses, where you store all your data and frequently query only a portion of the complete dataset\. 
+ If you want to keep your data volume capped to constrain costs\. Previously, if you wanted to keep your data volume capped to constrain costs, you had to unload data to Amazon S3\. 
+ If you need to have transactional access to your complete data with familiar tooling\.

#### Working with Amazon Redshift Managed Storage<a name="rs-managed-storage"></a>

With Amazon Redshift managed storage, you can store and process all your data in Amazon Redshift while getting more flexibility to scale compute and storage capacity separately\. You continue to ingest data with the COPY or INSERT command\.   To optimize performance and manage automatic data placement across tiers of storage, Amazon Redshift takes advantage of optimizations such as data block temperature, data block age, and workload patterns\. When needed, Amazon Redshift scales storage automatically to Amazon S3 without requiring any manual action\.  

For information about storage costs, see [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/)\. 

#### Managing RA3 Node Types<a name="rs-managing-ra3"></a>

To take advantage of separating compute from storage, you can migrate your cluster to the RA3 node type\. To use the RA3 node types, create your clusters in a virtual private cloud \(EC2\-VPC\)\. 

To create or update an RA3 cluster, do one of the following:
+ Create a cluster and choose RA3 as the node type\.  
+ Create a cluster from an existing cluster\. You can restore directly to an RA3 cluster with the AWS CLI command `restore-from-cluster-snapshot`\. You can also restore directly to an RA3 cluster by using the Amazon Redshift console\. To migrate a cluster to RA3, we recommend that you restore your cluster instead of resizing it\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\. 
+ Resize an existing cluster to RA3 directly\. To do this, use the classic resize operation and change the cluster to an RA3 cluster\. To migrate a cluster to RA3, we recommend that you restore your cluster instead of resizing it\. For more information, see [Resizing Clusters](rs-resize-tutorial.md)\. 

To change an RA3 cluster size, do one of the following:
+ To add more compute nodes to an RA3 cluster, you can use the elastic resize operation\. For more information, see [Resizing Clusters](rs-resize-tutorial.md)\.If elastic resize isn't available, use classic resize\. 
+ To remove compute nodes from an RA3 cluster, use the classic resize operation\. For more information, see [Resizing Clusters](rs-resize-tutorial.md)\. 

#### Migrating to RA3 Node Types<a name="rs-migrating-to-ra3"></a>

You can take a snapshot of your DS2 or DC2 cluster and restore it to RA3 nodes to create a new RA3 cluster\. You also can use the resize cluster operation to migrate to RA3, however the restore operation is recommended as it is faster\. You can use the Amazon Redshift console sizing calculator to get a recommended configuration when you restore or resize\. You can also get recommendations from the `DescribeNodeConfigurationOptions` API operation\.

#### RA3 Node Type Availability in AWS Regions<a name="ra3-regions"></a>

The RA3 node types are available only in the following AWS Regions: 
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ US East \(Ohio\) Region \(us\-east\-2\)
+ US West \(N\. California\) Region \(us\-west\-1\)
+ US West \(Oregon\) Region \(us\-west\-2\) 
+ Asia Pacific \(Seoul\) Region \(ap\-northeast\-2\)
+ Asia Pacific \(Singapore\) Region \(ap\-southeast\-1\)
+ Asia Pacific \(Sydney\) Region \(ap\-southeast\-2\)
+ Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)
+ EU \(Frankfurt\) Region \(eu\-central\-1\)
+ EU \(Ireland\) Region \(eu\-west\-1\)
+ EU \(London\) Region \(eu\-west\-2\)

#### Migrating from DC1 Node Types to DC2 Node Types<a name="rs-migrating-from-dc1-to-dc2"></a>

To take advantage of performance improvements, you can migrate your DC1 cluster to the DC2 node types\. 

Clusters that use the DC2 node types must be launched in a virtual private cloud \(EC2\-VPC\)\. If your cluster isn't in a VPC \(that is, it is using EC2\-Classic\), first create a snapshot of your cluster, and then choose one of the following options: 
+ From a dc1\.large cluster, restore directly to a dc2\.large cluster in a VPC\.
+  From a dc1\.8xlarge cluster in EC2\-Classic, first restore to a dc1\.8xlarge cluster in a VPC, and then resize your dc1\.8xlarge cluster to a dc2\.8xlarge cluster\. You can't restore directly to a dc2\.8xlarge cluster because the dc2\.8xlarge node type has a different number of slices than the dc1\.8xlarge node type\. 

If your cluster is in a VPC, choose one of the following options:
+ From a dc1\.large cluster, restore directly to a dc2\.large cluster in a VPC\.
+ From a dc1\.8xlarge cluster, resize your dc1\.8xlarge cluster to a dc2\.8xlarge cluster\. You can't restore directly to a dc2\.8xlarge cluster because the dc2\.8xlarge node type has a different number of slices than the dc1\.8xlarge node type\. 

Consider the following when migrating from DC1 to DC2 nodes\.
+ DC1 clusters which are 100% full might not migrate to an equivalent number of DC2 nodes\. If more disk space is needed, you can:
  + Resize to a configuration with more available disk space\. 
  + Cleanup unneeded data by truncating tables\. 
  + Delete rows and vacuum tables\. 
+ DC2 clusters do not support EC2\-Classic networking\. If your DC1 cluster is not running in a VPC, create one for your DC2 migration\. For more information, see [Managing Clusters in a VPC ](managing-clusters-vpc.md)\.
+ The cluster might be unavailable during the migration\. If you resize the cluster, it might be put into read\-only mode for the duration of the operation\. You can cancel a resize operation before it completes by choosing **cancel resize** from the cluster list in the Amazon Redshift console\. For more information, see [Resizing Clusters in Amazon Redshift](rs-resize-tutorial.md)\.

For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md) \.

#### Node Type Details<a name="rs-node-type-info"></a>

 The following tables summarize the node specifications for each node type and size\. The headings in the tables have these meanings: 
+ *vCPU* is the number of virtual CPUs for each node\.
+ *RAM* is the amount of memory in gibibytes \(GiB\) for each node\.
+ *Slices per Node* is the number of slices into which a compute node is partitioned\.

  The number of slices per node might change if the cluster is resized using elastic resize\.
+ *Storage* is the capacity and type of storage for each node\.
+  *Node Range* is the minimum and maximum number of nodes that Amazon Redshift supports for the node type and size\. 
**Note**  
You might be restricted to fewer nodes depending on the quota that is applied to your AWS account in the selected AWS Region\. 
+ *Total Capacity* is the total storage capacity for the cluster if you deploy the maximum number of nodes that is specified in the node range\.

For 16 TB or larger amounts of data, we recommend that you consider RA3 nodes as the default choice\. For workloads that need the fastest performance, consider using DC2 nodes\.


**RA3 Node Types**  

| Node Size | vCPU | RAM \(GiB\) | Slices Per Node | Managed Storage Per Node | Node Range | Total Capacity | 
| --- | --- | --- | --- | --- | --- | --- | 
| ra3\.16xlarge | 48 | 384 | 16 | 64 TB\* | 2–128 | 8\.192 PB | 

\* Indicates the amount of data stored in Amazon Redshift managed storage\. 


**Dense Storage Node Types**  

| Node Size | vCPU | RAM \(GiB\) | Slices Per Node | Storage Per Node | Node Range | Total Capacity | 
| --- | --- | --- | --- | --- | --- | --- | 
| ds2\.xlarge | 4 | 13 | 2 | 2 TB HDD | 1–32 | 64 TB | 
| ds2\.8xlarge | 36 | 244 | 16 | 16 TB HDD | 2–128 | 2 PB | 


**Dense Compute Node Types**  

| Node Size | vCPU | RAM \(GiB\) | Slices Per Node | Storage Per Node | Node Range | Total Capacity | 
| --- | --- | --- | --- | --- | --- | --- | 
| dc1\.large | 2 | 15 | 2 | 160 GB SSD | 1–32 | 5\.12 TB | 
| dc1\.8xlarge | 32 | 244 | 32 | 2\.56 TB SSD | 2–128 | 326 TB | 
| dc2\.large | 2 | 15\.25 | 2 | 160 GB NVMe\-SSD | 1–32 | 5\.12 TB | 
| dc2\.8xlarge | 32 | 244 | 16 | 2\.56 TB NVMe\-SSD | 2–128 | 326 TB | 
<a name="rs-old-node-names"></a>
**Previous Node Type Names**  
In previous releases of Amazon Redshift, certain node types had different names\. You can use the old names in the Amazon Redshift API and AWS CLI\. However, we recommend that you update any scripts that reference those names to use the current names instead\. The current and previous names are as follows\. 


**Previous Node Type Names**  

| Current Name | Previous Name\(s\) | 
| --- | --- | 
| ds2\.xlarge | ds1\.xlarge, dw\.hs1\.xlarge, dw1\.xlarge | 
| ds2\.8xlarge | ds1\.8xlarge, dw\.hs1\.8xlarge, dw1\.8xlarge | 
| dc1\.large | dw2\.large | 
| dc1\.8xlarge | dw2\.8xlarge | 

#### Determining the Number of Nodes<a name="how-many-nodes"></a>

The number of nodes that you choose depends on the size of your dataset and your desired query performance\. Using the dense storage node types as an example, if you have 32 TB of data, you can choose either 16 ds2\.xlarge nodes or 2 ds2\.8xlarge nodes\. If your data grows in small increments, choosing the ds2\.xlarge node size allows you to scale in increments of 2 TB\. If you typically see data growth in larger increments, a ds2\.8xlarge node size might be a better choice\. 

Because Amazon Redshift distributes and executes queries in parallel across all of a cluster’s compute nodes, you can increase query performance by adding nodes to your cluster\. Amazon Redshift also distributes your data across all compute nodes in a cluster\. When you run a cluster with at least two compute nodes, data on each node is mirrored on disks of another node to reduce the risk of incurring data loss\. 

Regardless of the choice you make, you can monitor query performance in the Amazon Redshift console and with Amazon CloudWatch metrics\. You can also add or remove nodes as needed to achieve the balance between storage and performance that works best for you\. When you request an additional node, Amazon Redshift takes care of all the details of deployment, load balancing, and data maintenance\. For more information about cluster performance, see [Monitoring Amazon Redshift Cluster Performance](metrics.md)\. 

If you intend to keep your cluster running continuously for a prolonged period, say, one year or more, you can pay considerably less by reserving the compute nodes for a one\-year or three\-year period\. To reserve compute nodes, you purchase what are called reserved node offerings\. You purchase one offering for each compute node that you want to reserve\. When you reserve a compute node, you pay a fixed up\-front charge and then an hourly recurring charge, whether your cluster is running or not\. The hourly charges, however, are significantly lower than those for on\-demand usage\. For more information, see [Purchasing Amazon Redshift Reserved Nodes](purchase-reserved-node-instance.md)\. 

### Determining the Cluster Maintenance Version<a name="cluster-maintenance-version"></a>

You can determine the Amazon Redshift engine and database version with the Amazon Redshift console\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

#### New Console<a name="cluster-engine-version-console"></a>

**To find the version of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, including **Query monitoring**, **Cluster performance**, **Maintenance and monitoring**, **Backup**, and **Properties** tabs\.

1. Choose the **Maintenance and monitoring** tab for more details\. 

1. In the **Maintenance** section, find **Current cluster version**\.

**Note**  
Although the console displays this information in one field, it's two parameters in the Amazon Redshift API, `ClusterVersion` and `ClusterRevisionNumber`\. For more information, see [Cluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Cluster.html) in the *Amazon Redshift API Reference*\. 

#### Original Console<a name="cluster-engine-version-console-originalconsole"></a>

You can determine the Amazon Redshift engine and database versions for your cluster in the **Cluster Version** field in the console\. The first two sections of the number are the cluster version, and the last section is the specific revision number of the database in the cluster\. In the following example, the cluster version is 1\.0 and the database revision number is 884\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-config-cluster-version.png)

**Note**  
 Although the console displays this information in one field, it’s two parameters in the Amazon Redshift API, `ClusterVersion` and `ClusterRevisionNumber`\. For more information, see [Cluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Cluster.html) in the *Amazon Redshift API Reference*\. 

To specify whether to automatically upgrade the Amazon Redshift engine in your cluster if a new version of the engine becomes available, use the setting **Allow version upgrade**\. This setting doesn't affect the database version upgrades, which are applied during the maintenance window that you specify for your cluster\. Amazon Redshift engine upgrades are *major version upgrades*, and Amazon Redshift database upgrades are *minor version upgrades*\. You can disable automatic version upgrades for major versions only\. For more information about maintenance windows for minor version upgrades, see [Maintenance Windows](#rs-maintenance-windows)\. 

## Resizing a Cluster<a name="cluster-resize-intro"></a>

 If your storage and performance needs change after you initially provision your cluster, you can resize your cluster\. You can scale the cluster in or out by adding or removing nodes\. Additionally, you can scale the cluster up or down by specifying a different node type\. 

 For example, you can add more nodes, change node types, change a single\-node cluster to a multi\-node cluster, or change a multi\-node cluster to a single\-node cluster\. However, you must ensure that the resulting cluster is large enough to hold the data that you currently have or else the resize will fail\. When using the API, you have to specify the node type, node size, and the number of nodes even if you only change one of the properties\. 

 The following describes the resize process: 

1. When you initiate the resize process, Amazon Redshift sends an event notification that acknowledges the resize request and starts to provision the new \(target\) cluster\.

1. When the new \(target\) cluster is provisioned, Amazon Redshift sends an event notification that the resize has started, then restarts your existing \(source\) cluster in read\-only mode\. The restart terminates all existing connections to the cluster\. All uncommitted transactions \(including COPY\) are rolled back\. While the cluster is in read\-only mode, you can run read queries but not write queries\.

1. Amazon Redshift starts to copy data from the source cluster to the target cluster\.

1. When the resize process nears completion, Amazon Redshift updates the endpoint of the target cluster, and all connections to the source cluster are terminated\.

1. After the resize completes, Amazon Redshift sends an event notification that the resize has completed\. You can connect to the target cluster and resume running read and write queries\.

When you resize your cluster, it will remain in read\-only mode until the resize completes\. You can view the resize progress on the cluster's **Status** tab in the Amazon Redshift console\. The time it takes to resize a cluster depends on the amount of data in each node\. Typically, the resize process varies from a couple of hours to a day, although clusters with larger amounts of data might take even longer\. This is because the data is copied in parallel from each node on the source cluster to the nodes in the target cluster\. For more information about resizing clusters, see [Resizing Clusters in Amazon Redshift](rs-resize-tutorial.md) and [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\. 

Amazon Redshift doesn't sort tables during a resize operation\. When you resize a cluster, Amazon Redshift distributes the database tables to the new compute nodes based on their distribution styles and runs an ANALYZE to update statistics\. Rows that are marked for deletion aren't transferred, so you need to run a VACUUM only if your tables need to be resorted\. For more information, see [Vacuuming Tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the *Amazon Redshift Database Developer Guide*\. 

If your cluster is public and is in a VPC, it keeps the same Elastic IP address \(EIP\) for the leader node after resizing\. If your cluster is private and is in a VPC, it keeps the same private IP address for the leader node after resizing\. If your cluster isn't in a VPC, a new public IP address is assigned for the leader node as part of the resize operation\.

To get the leader node IP address for a cluster, use the dig utility, as shown following:

```
dig mycluster.abcd1234.us-west-2.redshift.amazonaws.com
```

The leader node IP address is at the end of the ANSWER SECTION in the results, as shown following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/dig_IP_address.png)

## Supported Platforms to Launch Your Cluster<a name="cluster-platforms"></a>

 Amazon Redshift clusters run in Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are configured for the Amazon Redshift node type and size that you select\. You can launch an Amazon Redshift cluster in one of two platforms: EC2\-VPC or EC2\-Classic, which are the supported platforms for Amazon EC2 instances\. We recommend that you launch your cluster in an EC2\-VPC platform instead of an EC2\-Classic platform\. For more information about these platforms, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. The platform or platforms available to you depend on your AWS account settings\. 

**Note**  
To prevent connection issues between SQL client tools and the Amazon Redshift database, we recommend doing one of two things\. You can configure an inbound rule that enables the hosts to negotiate packet size\. Alternatively, you can disable TCP/IP jumbo frames by setting the maximum transmission unit \(MTU\) to 1500 on the network interface \(NIC\) of your Amazon EC2 instances\. For more information about these approaches, see [Queries Appear to Hang and Sometimes Fail to Reach the Cluster](connecting-drop-issues.md)\. 

### EC2\-VPC Platform<a name="cluster-platforms-ec2-vpc"></a>

 In the EC2\-VPC platform, your cluster runs in a virtual private cloud \(VPC\) that is logically isolated to your AWS account\. If you provision your cluster in the EC2\-VPC platform, you control access to your cluster by associating one or more VPC security groups with the cluster\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. 

 To create a cluster in a VPC, you must first create an Amazon Redshift cluster subnet group by providing subnet information of your VPC, and then provide the subnet group when launching the cluster\. For more information, see [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)\. 

 For more information about Amazon Virtual Private Cloud \(Amazon VPC\), see the [Amazon VPC product detail page](https://aws.amazon.com/vpc/)\. 

### EC2\-Classic Platform<a name="cluster-platforms-ec2-classic"></a>

 In the EC2\-Classic platform, your cluster runs in a single, flat network that you share with other AWS customers\. If you provision your cluster in the EC2\-Classic platform, you control access to your cluster by associating one or more Amazon Redshift cluster security groups with the cluster\. For more information, see [Amazon Redshift Cluster Security Groups](working-with-security-groups.md)\. 

### Choose a Platform<a name="choose-rs-cluster-platform"></a>

 Your AWS account is capable of launching instances either into both platforms, or only into EC2\-VPC, on a region\-by\-region basis\. To determine which platform your account supports, and then launch a cluster, do the following: 

1.  Decide on the AWS region in which you want to deploy a cluster\. For a list of AWS regions in which Amazon Redshift is available, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

1.  Find out which Amazon EC2 platforms your account supports in the chosen AWS region\. You can find this information in the Amazon EC2 console\. For step\-by\-step instructions, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

1.  If your account supports both of the platforms, choose the one on which you want to deploy your Amazon Redshift cluster\. If your account supports only EC2\-VPC, you must deploy your cluster in VPC\. 

1.  Deploy your Amazon Redshift cluster\. You can deploy a cluster by using the Amazon Redshift console, or programmatically by using the Amazon Redshift API, CLI, or SDK libraries\. For more information about these options and links to the related documentation, see [What Is Amazon Redshift?](welcome.md)\. 

## Regions and Availability Zone Considerations<a name="az-considerations"></a>

 Amazon Redshift is available in several AWS regions\. By default, Amazon Redshift provisions your cluster in a randomly selected Availability Zone \(AZ\) within the AWS region that you select\. All the cluster nodes are provisioned in the same AZ\. 

 You can optionally request a specific AZ if Amazon Redshift is available in that AZ\. For example, if you already have an Amazon EC2 instance running in one AZ, you might want to create your Amazon Redshift cluster in the same AZ to reduce latency\. On the other hand, you might want to choose another AZ for higher availability\. Amazon Redshift might not be available in all AZs within an AWS Region\.

 For a list of supported AWS regions where you can provision an Amazon Redshift cluster, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#redshift_region) in the *Amazon Web Services General Reference*\.

## Cluster Maintenance<a name="rs-cluster-maintenance"></a>

Amazon Redshift periodically performs maintenance to apply upgrades to your cluster\. During these updates, your Amazon Redshift cluster isn't available for normal operations\. You have several ways to control how we maintain your cluster\. For example, you can control when we deploy updates to your clusters\. You can also choose whether your cluster will always run the most recently released version, or the version released previously to the most recently released version\. Finally, you have the option to defer non\-mandatory maintenance updates for a period of time\.

**Topics**
+ [Maintenance Windows](#rs-maintenance-windows)
+ [Deferring Maintenance](#rs-mgmt-defer-maintenance)
+ [Choosing Cluster Maintenance Tracks](#rs-mgmt-maintenance-tracks)
+ [Managing Cluster Versions](#rs-mgmt-cluster-version)
+ [Rolling Back the Cluster Version](#rs-mgmt-rollback-version)

### Maintenance Windows<a name="rs-maintenance-windows"></a>

 Amazon Redshift assigns a 30\-minute maintenance window at random from an 8\-hour block of time per region, occurring on a random day of the week \(Monday through Sunday, inclusive\)\. 

#### Default Maintenance Windows<a name="rs-default-maintenance-windows"></a>

The following list shows the time blocks for each region from which the default maintenance windows are assigned: 
+ US East \(N\. Virginia\) region: 03:00–11:00 UTC
+ US East \(Ohio\) region: 03:00–11:00 UTC
+ US West \(N\. California\) region: 06:00–14:00 UTC
+ US West \(Oregon\) region: 06:00–14:00 UTC
+ Asia Pacific \(Hong Kong\) region: 13:00–21:00 UTC
+ Asia Pacific \(Mumbai\) region: 16:30–00:30 UTC
+ Asia Pacific \(Osaka\-Local\) region: 13:00–21:00 UTC
+ Asia Pacific \(Seoul\) region: 13:00–21:00 UTC
+ Asia Pacific \(Singapore\) region: 14:00–22:00 UTC
+ Asia Pacific \(Sydney\) region: 12:00–20:00 UTC
+ Asia Pacific \(Tokyo\) region: 13:00–21:00 UTC
+ Canada \(Central\) region: 03:00–11:00 UTC
+ China \(Beijing\) region: 13:00–21:00 UTC
+ China \(Ningxia\) region: 13:00–21:00 UTC
+ EU \(Frankfurt\) region: 06:00–14:00 UTC
+ EU \(Ireland\) region: 22:00–06:00 UTC
+ EU \(London\) region: 22:00–06:00 UTC
+ EU \(Paris\) region: 23:00–07:00 UTC
+ EU \(Stockholm\) region: 23:00–07:00 UTC
+ Middle East \(Bahrain\) region: 13:00–21:00 UTC
+ South America \(São Paulo\) region: 19:00–03:00 UTC

If a maintenance event is scheduled for a given week, it will start during the assigned 30 minute maintenance window\. While Amazon Redshift is performing maintenance, it terminates any queries or other operations that are in progress\. Most maintenance completes during the 30 minute maintenance window, but some maintenance tasks might continue running after the window closes\. If there are no maintenance tasks to perform during the scheduled maintenance window, your cluster continues to operate normally until the next scheduled maintenance window\. 

You can change the scheduled maintenance window by modifying the cluster, either programmatically or by using the Amazon Redshift console\. The window must be at least 30 minutes and not longer than 24 hours\. For more information, see [Managing Clusters Using the Console](managing-clusters-console.md)\.

### Deferring Maintenance<a name="rs-mgmt-defer-maintenance"></a>

If you need to reschedule your cluster’s maintenance window, you have the option to defer maintenance by up to 45 days\. For example, if your cluster’s maintenance window is set to Wednesday 8:30 – 9:00 UTC and you need to have access to your cluster at that time, you can defer the maintenance to a later time period\. We will not perform any maintenance on your cluster when you have specified a deferment, unless we need to update hardware\.

If we need to update hardware or make other mandatory updates during your period of deferment, we notify you and make the required changes\. Your cluster isn't available during these updates\.

If you defer your cluster's maintenance, the maintenance window following your period of deferment is mandatory\. It can't be deferred\.

**Note**  
You can't defer maintenance after it has started\.

For more information, see [Deferring Maintenance](managing-clusters-console.md#defer-maintenance-window)\.

### Choosing Cluster Maintenance Tracks<a name="rs-mgmt-maintenance-tracks"></a><a name="rs-maintenance-tracks"></a>

When Amazon Redshift releases a new cluster version, your cluster is updated during its maintenance window\. You can control whether your cluster is updated to the most recent approved release or to the previous release\. 

The maintenance track controls which cluster version is applied during a maintenance window\. When Amazon Redshift releases a new cluster version, that version is assigned to the *current* track, and the previous version is assigned to the *trailing* track\. To set the maintenance track for the cluster, specify one of the following values:
+ **Current** – Use the most current approved cluster version\.
+ **Trailing** – Use the cluster version before the current version\.
+ **Preview** – Use the cluster version that contains new features available for preview\. 

For example, suppose that your cluster is currently running version 1\.0\.2762 and the Amazon Redshift current version is 1\.0\.3072\. If you set the maintenance track value to **Current**, your cluster is updated to version 1\.0\.3072 \(the next approved release\) during the next maintenance window\. If you set the maintenance track value to **Trailing**, your cluster isn't updated until there is a new release after 1\.0\.3072\. 

**Preview tracks**

A **Preview** track might not always be available to choose\. When you choose a **Preview** track, a track name must also be selected\. Preview tracks and its related resources are temporary, have functional limitations, and might not contain all current Amazon Redshift features available in other tracks\. When working with preview tracks: 
+ You can't switch a cluster from one preview track to another\. 
+ You can't switch a cluster to a preview track from a current or trailing track\. 
+ You can't restore from a snapshot created from a different preview track\.
+ You can only use the preview track when creating a new cluster, or when restoring from a snapshot\. 
+ You can't restore from a snapshot created from a different preview track, or with a cluster maintenance version later than the preview track cluster version\. For example, when you restore a cluster to a preview track, you can only use a snapshot created from an earlier cluster maintenance version than that of the preview track\. 

**Switching between Maintenance Tracks**

Changing tracks for a cluster is generally a one\-time decision\. You should exercise caution in changing tracks\. If you change the maintenance track from **Trailing** to **Current**, we will update the cluster to the **Current** track release version during the next maintenance window\. However, if you change the cluster's maintenance track to **Trailing** we won't update your cluster until there is a new release after the **Current** track release version\. 

**Maintenance Tracks and Restore**

A snapshot inherits the source cluster's maintenance track\. If you change the source cluster's maintenance track after you take a snapshot, the snapshot and the source cluster are on different tracks\. When you restore from the snapshot, the new cluster will be on the maintenance track that was inherited from the source cluster\. You can change the maintenance track after the restore operation completes\. Resizing a cluster doesn't affect the cluster's maintenance track\. 

For more information see, [Setting the Maintenance Track for a Cluster](managing-clusters-console.md#rs-mgmt-set-maintenance-track)\.

### Managing Cluster Versions<a name="rs-mgmt-cluster-version"></a>

A maintenance track is a series of releases\. You can decide if your cluster is on the **Current** track or the **Trailing** track\. If you put your cluster on the **Current** track, it will always be upgraded to the most recent cluster release version during its maintenance window\. If you put your cluster on the **Trailing** track, it will always run the cluster release version that was released immediately before the most recently released version\. 

The **Release status** column in the Amazon Redshift console list of clusters indicates whether one of your clusters is available for upgrade\. 

### Rolling Back the Cluster Version<a name="rs-mgmt-rollback-version"></a>

If your cluster is up to date with the latest cluster version, you can choose to roll it back to the previous version\.

For detailed information about features and improvements included with each cluster version, see [Cluster Version History](rs-mgmt-cluster-version-notes.md)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New Console** or the **Original Console** instructions based on the console that you are using\. The **New Console** instructions are open by default\.

#### New Console<a name="cluster-rollback"></a>

**To roll back to a previous cluster version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**\. 

1. Choose the cluster to roll back\. 

1. For **Actions**, choose **Roll back cluster version**\. The **Roll back cluster version** page appears\.

1. If there is a version available for roll back, follow the instructions on the page\. 

1. Choose **Roll back now**\. 

#### Original Console<a name="cluster-rollback-originalconsole"></a>

**To roll back to a previous cluster version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. Choose the cluster that you want to roll back and choose the **Status** tab\.

   If there is a version available to roll back to, it appears on the status tab of the details page\.   
![\[Roll-back version details screen\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/roll-back.png)

1. Choose **Rollback to release \(release number\)**\.

## Default Disk Space Alarm<a name="rs-clusters-default-disk-usage-alarm"></a>

When you create an Amazon Redshift cluster, you can optionally configure an Amazon CloudWatch alarm to monitor the average percentage of disk space that is used across all of the nodes in your cluster\. We’ll refer to this alarm as the *default disk space alarm*\. 

The purpose of default disk space alarm is to help you monitor the storage capacity of your cluster\. You can configure this alarm based on the needs of your data warehouse\. For example, you can use the warning as an indicator that you might need to resize your cluster\. You might resize either to a different node type or to add nodes, or perhaps to purchase reserved nodes for future expansion\. 

The default disk space alarm triggers when disk usage reaches or exceeds a specified percentage for a certain number of times and at a specified duration\. By default, this alarm triggers when the percentage that you specify is reached, and stays at or above that percentage for five minutes or longer\. You can edit the default values after you launch the cluster\. 

When the CloudWatch alarm triggers, Amazon Simple Notification Service \(Amazon SNS\) sends a notification to specified recipients to warn them that the percentage threshold is reached\. Amazon SNS uses a topic to specify the recipients and message that are sent in a notification\. You can use an existing Amazon SNS topic; otherwise, a topic is created based on the settings that you specify when you launch the cluster\. You can edit the topic for this alarm after you launch the cluster\. For more information about creating Amazon SNS topics, see [Getting Started with Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\. 

After you launch the cluster, you can view and edit the alarm from the cluster’s **Status** window under **CloudWatch Alarms**\. The name is **percentage\-disk\-space\-used\-default\-<*string*>**\. You can open the alarm to view the Amazon SNS topic that it is associated with and edit alarm settings\. If you did not select an existing Amazon SNS topic to use, the one created for you is named **<*clustername*>\-default\-alarms \(<*recipient*>\)**; for example, **examplecluster\-default\-alarms \(notify@example\.com\)**\. 

 For more information about configuring and editing the default disk space alarm, see [Creating a Cluster](managing-clusters-console.md#create-cluster) and [Creating or Editing a Disk Space Alarm](managing-clusters-console.md#rs-mgmt-edit-default-disk-space-alarm)\. 

**Note**  
 If you delete your cluster, the alarm associated with the cluster will not be deleted but it will not trigger\. You can delete the alarm from the CloudWatch console if you no longer need it\. 

## Renaming Clusters<a name="rs-mgmt-rename-cluster"></a>

You can rename a cluster if you want the cluster to use a different name\. Because the endpoint to your cluster includes the cluster name \(also referred to as the *cluster identifier*\), the endpoint will change to use the new name after the rename finishes\. For example, if you have a cluster named `examplecluster` and rename it to `newcluster`, the endpoint will change to use the `newcluster` identifier\. Any applications that connect to the cluster must be updated with the new endpoint\. 

You might rename a cluster if you want to change the cluster to which your applications connect without having to change the endpoint in those applications\. In this case, you must first rename the original cluster and then change the second cluster to reuse the name of the original cluster before the rename\. Doing this is necessary because the cluster identifier must be unique within your account and region, so the original cluster and second cluster cannot have the same name\. You might do this if you restore a cluster from a snapshot and don’t want to change the connection properties of any dependent applications\. 

**Note**  
 If you delete the original cluster, you are responsible for deleting any unwanted cluster snapshots\. 

When you rename a cluster, the cluster status changes to `renaming` until the process finishes\. The old DNS name that was used by the cluster is immediately deleted, although it could remain cached for a few minutes\. The new DNS name for the renamed cluster becomes effective within about 10 minutes\. The renamed cluster is not available until the new name becomes effective\. The cluster will be rebooted and any existing connections to the cluster will be dropped\. After this completes, the endpoint will change to use the new name\. For this reason, you should stop queries from running before you start the rename and restart them after the rename finishes\. 

 Cluster snapshots are retained, and all snapshots associated with a cluster remain associated with that cluster after it is renamed\. For example, suppose you have a cluster that serves your production database and the cluster has several snapshots\. If you rename the cluster and then replace it in the production environment with a snapshot, the cluster that you renamed will still have those existing snapshots associated with it\. 

 Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) event notifications are associated with the name of the cluster\. If you rename the cluster, you need to update these accordingly\. You can update the CloudWatch alarms in the CloudWatch console, and you can update the Amazon SNS event notifications in the Amazon Redshift console on the **Events** pane\. The load and query data for the cluster continues to display data from before the rename and after the rename\. However, performance data is reset after the rename process finishes\. 

For more information, see [Modifying a Cluster](managing-clusters-console.md#modify-cluster)\.

## Shutting Down and Deleting Clusters<a name="rs-mgmt-shutdown-delete-cluster"></a>

You can shut down your cluster if you want to stop it from running and incurring charges\. When you shut it down, you can optionally create a final snapshot\. If you create a final snapshot, Amazon Redshift will create a manual snapshot of your cluster before shutting it down\. You can later restore that snapshot if you want to resume running the cluster and querying data\. 

If you no longer need your cluster and its data, you can shut it down without creating a final snapshot\. In this case, the cluster and data are deleted permanently\. For more information about shutting down and deleting clusters, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. 

Regardless of whether you shut down your cluster with a final manual snapshot, all automated snapshots associated with the cluster will be deleted after the cluster is shut down\. Any manual snapshots associated with the cluster are retained\. Any manual snapshots that are retained, including the optional final snapshot, are charged at the Amazon Simple Storage Service storage rate if you have no other clusters running when you shut down the cluster, or if you exceed the available free storage that is provided for your running Amazon Redshift clusters\. For more information about snapshot storage charges, see the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 

## Cluster Status<a name="rs-mgmt-cluster-status"></a>

The cluster status displays the current state of the cluster\. The following table provides a description for each cluster status\. 


****  

| Status | Description | 
| --- | --- | 
| available | The cluster is running and available\. | 
| available, prep\-for\-resize | The cluster is being prepared for elastic resize\. The cluster is running and available for read and write queries, but cluster operations, such as creating a snapshot, are not available\. | 
| available, resize\-cleanup  | An elastic resize operation is completing data transfer to the new cluster nodes\. The cluster is running and available for read and write queries, but cluster operations, such as creating a snapshot, are not available\. | 
| creating | Amazon Redshift is creating the cluster\. For more information, see [Creating a Cluster](managing-clusters-console.md#create-cluster)\. | 
| deleting | Amazon Redshift is deleting the cluster\. For more information, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. | 
| final\-snapshot | Amazon Redshift is taking a final snapshot of the cluster before deleting it\. For more information, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. | 
| hardware\-failure |  The cluster suffered a hardware failure\. If you have a single\-node cluster, the node cannot be replaced\. To recover your cluster, restore a snapshot\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.  | 
| incompatible\-hsm |  Amazon Redshift cannot connect to the hardware security module \(HSM\)\. Check the HSM configuration between the cluster and HSM\. For more information, see [Encryption for Amazon Redshift Using Hardware Security Modules](working-with-db-encryption.md#working-with-HSM)\.  | 
| incompatible\-network |  There is an issue with the underlying network configuration\. Make sure that the VPC in which you launched the cluster exists and its settings are correct\. For more information, see [Managing Clusters in a VPC ](managing-clusters-vpc.md)\.  | 
| incompatible\-parameters | There is an issue with one or more parameter values in the associated parameter group, and the parameter value or values cannot be applied\. Modify the parameter group and update any invalid values\. For more information, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\.  | 
| incompatible\-restore |  There was an issue restoring the cluster from the snapshot\. Try restoring the cluster again with a different snapshot\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.  | 
| modifying |  Amazon Redshift is applying changes to the cluster\. For more information, see [Modifying a Cluster](managing-clusters-console.md#modify-cluster)\.  | 
| rebooting |  Amazon Redshift is rebooting the cluster\. For more information, see [Rebooting a Cluster](managing-clusters-console.md#reboot-cluster)\.  | 
| renaming |  Amazon Redshift is applying a new name to the cluster\. For more information, see [Renaming Clusters](#rs-mgmt-rename-cluster)\.  | 
| resizing |  Amazon Redshift is resizing the cluster\. For more information, see [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| rotating\-keys |  Amazon Redshift is rotating encryption keys for the cluster\. For more information, see [Encryption Key Rotation in Amazon Redshift](working-with-db-encryption.md#working-with-key-rotation)\.  | 
| storage\-full |  The cluster has reached its storage capacity\. Resize the cluster to add nodes or to choose a different node size\. For more information, see [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| updating\-hsm |  Amazon Redshift is updating the HSM configuration\. \.  | 