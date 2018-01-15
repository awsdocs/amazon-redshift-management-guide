# Amazon Redshift Clusters<a name="working-with-clusters"></a>

## Overview<a name="working-with-clusters-overview"></a>

An Amazon Redshift data warehouse is a collection of computing resources called *nodes*, which are organized into a group called a *cluster*\. Each cluster runs an Amazon Redshift engine and contains one or more databases\. 

**Note**  
At this time, Amazon Redshift version 1\.0 engine is available\. However, as the engine is updated, multiple Amazon Redshift engine versions might be available for selection\. 

You can determine the Amazon Redshift engine and database versions for your cluster in the **Cluster Version** field in the console\. The first two sections of the number are the cluster version, and the last section is the specific revision number of the database in the cluster\. In the following example, the cluster version is 1\.0 and the database revision number is 884\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-config-cluster-version.png)

**Note**  
 Although the console displays this information in one field, it is two parameters in the Amazon Redshift API: `ClusterVersion` and `ClusterRevisionNumber`\. For more information, go to [Cluster](http://docs.aws.amazon.com/redshift/latest/APIReference/API_Cluster.html) in the *Amazon Redshift API Reference*\. 

Amazon Redshift provides a setting, **Allow Version Upgrade**, to specify whether to automatically upgrade the Amazon Redshift engine in your cluster if a new version of the engine becomes available\. This setting does not affect the database version upgrades, which are applied during the maintenance window that you specify for your cluster\. Amazon Redshift engine upgrades are *major version upgrades*, and Amazon Redshift database upgrades are *minor version upgrades*\. You can disable automatic version upgrades for major versions only\. For more information about maintenance windows for minor version upgrades, see [Maintenance Windows](#rs-maintenance-windows)\. 

## Clusters and Nodes in Amazon Redshift<a name="rs-about-clusters-and-nodes"></a>

An Amazon Redshift cluster consists of nodes\. Each cluster has a leader node and one or more compute nodes\. The *leader node* receives queries from client applications, parses the queries, and develops query execution plans\. The leader node then coordinates the parallel execution of these plans with the compute nodes and aggregates the intermediate results from these nodes\. It then finally returns the results back to the client applications\. 

*Compute nodes* execute the query execution plans and transmit data among themselves to serve these queries\. The intermediate results are sent to the leader node for aggregation before being sent back to the client applications\. For more information about leader nodes and compute nodes, see [Data Warehouse System Architecture](http://docs.aws.amazon.com/redshift/latest/dg/c_high_level_system_architecture.html) in the *Amazon Redshift Database Developer Guide*\. 

When you launch a cluster, one option you specify is the node type\. The node type determines the CPU, RAM, storage capacity, and storage drive type for each node\. The *dense storage* \(DS\) node types are storage optimized\. The *dense compute* \(DC\) node types are compute optimized\. 

DS2 node types are optimized for large data workloads and use hard disk drive \(HDD\) storage\. 

DC1 and DC2 nodes are optimized for performance\-intensive workloads\. Because they use solid state drive \(SSD\) storage, DC1 and DC2 node types deliver much faster I/O compared to DS node types, but provide less storage space\. 

You launch clusters that use the DC2 node types in a virtual private cloud \(VPC\)\. You can't launch DC2 clusters in EC2 classic mode\. For more information, see [Creating a Cluster in a VPC](getting-started-cluster-in-vpc.md)\.

The node type that you choose depends heavily on three things:

+ The amount of data you import into Amazon Redshift

+ The complexity of the queries and operations that you run in the database

+ The needs of downstream systems that depend on the results from those queries and operations

Node types are available in different sizes\. DS2 nodes are available in xlarge and 8xlarge sizes\. DC1 nodes are available in large and 8xlarge sizes\. Node size and the number of nodes determine the total storage for a cluster\. 

Some node types allow one node \(single\-node\) or two or more nodes \(multi\-node\)\. The minimum for 8xlarge clusters is two nodes\. On a single\-node cluster, the node is shared for leader and compute functionality\. On a multi\-node cluster, the leader node is separate from the compute nodes\. 

 Amazon Redshift applies quotas to resources for each AWS account in each region\. A *quota* restricts the number of resources that your account can create for a given resource type, such as nodes or snapshots, within a region\. For more information about the default quotas that apply to Amazon Redshift resources, go to [Amazon Redshift Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift) in the *Amazon Web Services General Reference*\. To request an increase, submit an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

The cost of your cluster depends on the region, node type, number of nodes, and whether the nodes are reserved in advance\. For more information about the cost of nodes, go to the [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/) page\. 

### Migrating From DC1 Node Types to DC2 Node Types<a name="rs-migrating-from-dc1-to-dc2"></a>

To take advantage of performance improvements, you can migrate your DC1 cluster to the newer DC2 node types\. 

Clusters that use the DC2 node types must be launched in a virtual private cloud \(EC2\-VPC\)\. If your cluster is not in a VPC \(EC2\-CLASSIC\), first create a snapshot of your cluster, and then choose one of the following options: 

+ From a dc1\.large cluster, restore directly to a dc2\.large cluster in a VPC\.

+  From a dc1\.8xlarge cluster in EC2\-CLASSIC, first restore to a dc1\.8xlarge cluster in a VPC, and then resize your dc1\.8xlarge cluster to a dc2\.8xlarge cluster\. You can't restore directly to a dc2\.8xl cluster because the dc2\.8xlarge node type has a different number of slices than the dc1\.8xlarge node type\. 

If your cluster is in a VPC, choose one of the following options:

+ From a dc1\.large cluster, restore directly to a dc2\.large cluster in a VPC\.

+ From a dc1\.8xlarge cluster, resize your dc1\.8xl cluster to a dc2\.8xlarge cluster\. You can't restore directly to a dc2\.8xlarge cluster because the dc2\.8xlarge node type has a different number of slices than the dc1\.8xlarge node type\. 

For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md) and [Resizing Clusters](rs-resize-tutorial.md)\.

### Node Type Details<a name="rs-node-type-info"></a>

 The following tables summarize the node specifications for each node type and size\. In the two tables following, these headings have the given meaning: 

+ *vCPU* is the number of virtual CPUs for each node\.

+ *ECU* is the number of Amazon EC2 compute units for each node\.

+ *RAM* is the amount of memory in gibibytes \(GiB\) for each node\.

+ *Slices per Node* is the number of slices into which a compute node is partitioned\.

+ *Storage* is the capacity and type of storage for each node\.

+  *Node Range* is the minimum and maximum number of nodes that Amazon Redshift supports for the node type and size\. 
**Note**  
You might be restricted to fewer nodes depending on the quota that is applied to your AWS account in the selected region, as discussed preceding\.

+ *Total Capacity* is the total storage capacity for the cluster if you deploy the maximum number of nodes that is specified in the node range\.

**Important**  
The DS1 node types are deprecated\.  The new DS2 node types provide higher performance than DS1 at no extra cost\. If you have purchased DS1 reserved nodes, contact *redshift\-pm@amazon\.com* for assistance transitioning to DS2 node types\.


**Dense Storage Node Types**  

| Node Size | vCPU | ECU | RAM \(GiB\) | Slices Per Node | Storage Per Node | Node Range | Total Capacity | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ds2\.xlarge | 4 | 13 | 31 | 2 | 2 TB HDD | 1–32 | 64 TB | 
| ds2\.8xlarge | 36 | 119 | 244 | 16 | 16 TB HDD | 2–128 | 2 PB | 


**Dense Compute Node Types**  

| Node Size | vCPU | ECU | RAM \(GiB\) | Slices Per Node | Storage Per Node | Node Range | Total Capacity | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| dc1\.large | 2 | 7 | 15 | 2 | 160 GB SSD | 1–32 | 5\.12 TB | 
| dc1\.8xlarge | 32 | 104 | 244 | 32 | 2\.56 TB SSD | 2–128 | 326 TB | 
| dc2\.large | 2 | 7 | 15\.25 | 2 | 160 GB NVMe\-SSD | 1–32 | 5\.12 TB | 
| dc2\.8xlarge | 32 | 99 | 244 | 16 | 2\.56 TB NVMe\-SSD | 2–128 | 326 TB | 

**Previous Node Type Names**  
In previous releases of Amazon Redshift, the node types had different names\. You can use the old names in the Amazon Redshift API and AWS Command Line Interface \(AWS CLI\)\. However, we recommend that you update any scripts that reference those names to use the current names instead\. The current and previous names are as follows\. 


**Previous Node Type Names**  

| Current Name | Previous Name\(s\) | 
| --- | --- | 
| ds2\.xlarge | ds1\.xlarge, dw\.hs1\.xlarge, dw1\.xlarge | 
| ds2\.8xlarge | ds1\.8xlarge, dw\.hs1\.8xlarge, dw1\.8xlarge | 
| dc1\.large | dw2\.large | 
| dc1\.8xlarge | dw2\.8xlarge | 

### Determining the Number of Nodes<a name="how-many-nodes"></a>

The number of nodes that you choose depends on the size of your dataset and your desired query performance\. Using the dense storage node types as an example, if you have 32 TB of data, you can choose either 16 ds2\.xlarge nodes or 2 ds2\.8xlarge nodes\. If your data grows in small increments, choosing the ds1\.xlarge node size will allow you to scale in increments of 2 TB\. If you typically see data growth in larger increments, a ds2\.8xlarge node size might be a better choice\. 

Because Amazon Redshift distributes and executes queries in parallel across all of a cluster’s compute nodes, you can increase query performance by adding nodes to your cluster\. Amazon Redshift also distributes your data across all compute nodes in a cluster\. When you run a cluster with at least two compute nodes, data on each node will always be mirrored on disks on another node and you reduce the risk of incurring data loss\. 

Regardless of the choice you make, you can monitor query performance in the Amazon Redshift console and with Amazon CloudWatch metrics\. You can also add or remove nodes as needed to achieve the balance between storage and performance that works best for you\. When you request an additional node, Amazon Redshift takes care of all the details of deployment, load balancing, and data maintenance\. For more information about cluster performance, see [Monitoring Amazon Redshift Cluster Performance](metrics.md)\. 

If you intend to keep your cluster running continuously for a prolonged period, say, one year or more, you can pay considerably less by reserving the compute nodes for a one\-year or three\-year period\. To reserve compute nodes, you purchase what are called reserved node offerings\. You purchase one offering for each compute node that you want to reserve\. When you reserve a compute node, you pay a fixed up\-front charge and then an hourly recurring charge, whether your cluster is running or not\. The hourly charges, however, are significantly lower than those for on\-demand usage\. For more information, see [Purchasing Amazon Redshift Reserved Nodes](purchase-reserved-node-instance.md)\. 

## Resizing a Cluster<a name="cluster-resize-intro"></a>

 If your storage and performance needs change after you initially provision your cluster, you can resize your cluster\. You can scale the cluster in or out by adding or removing nodes\. Additionally, you can scale the cluster up or down by specifying a different node type\. 

 For example, you can add more nodes, change node types, change a single\-node cluster to a multinode cluster, or change a multinode cluster to a single\-node cluster\. However, you must ensure that the resulting cluster is large enough to hold the data that you currently have or else the resize will fail\. When using the API, you have to specify the node type, node size, and the number of nodes even if you only change one of the two\. 

 The following describes the resize process: 

1. When you initiate the resize process, Amazon Redshift sends an event notification that acknowledges the resize request and starts to provision the new \(target\) cluster\.

1. When the new \(target\) cluster is provisioned, Amazon Redshift sends an event notification that the resize has started, then restarts your existing \(source\) cluster in read\-only mode\. The restart terminates all existing connections to the cluster\. All uncommitted transactions \(including COPY\) are rolled back\. While the cluster is in read\-only mode, you can run read queries but not write queries\.

1. Amazon Redshift starts to copy data from the source cluster to the target cluster\.

1. When the resize process nears completion, Amazon Redshift updates the endpoint of the target cluster and all connections to the source cluster are terminated\.

1. After the resize completes, Amazon Redshift sends an event notification that the resize has completed\. You can connect to the target cluster and resume running read and write queries\.

When you resize your cluster, it will remain in read\-only mode until the resize completes\. You can view the resize progress on the cluster's **Status** tab in the Amazon Redshift console\. The time it takes to resize a cluster depends on the amount of data in each node\. Typically, the resize process varies from a couple of hours to a day, although clusters with larger amounts of data might take even longer\. This is because the data is copied in parallel from each node on the source cluster to the nodes in the target cluster\. For more information about resizing clusters, see [Tutorial: Resizing Clusters in Amazon Redshift](rs-resize-tutorial.md) and [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\. 

Amazon Redshift does not sort tables during a resize operation\. When you resize a cluster, Amazon Redshift distributes the database tables to the new compute nodes based on their distribution styles and runs an ANALYZE to update statistics\. Rows that are marked for deletion are not transferred, so you will only need to run a VACUUM if your tables need to be resorted\. For more information, see [Vacuuming tables](http://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the *Amazon Redshift Database Developer Guide*\. 

If your cluster is public and is in a VPC, it keeps the same elastic IP address \(EIP\) for the leader node after resizing\. If your cluster is private and is in a VPC, it keeps the same private IP address for the leader node after resizing\. If your cluster is not in a VPC, a new public IP address is assigned for the leader node as part of the resize operation\.

To get the leader node IP address for a cluster, use the dig utility, as shown following:

```
dig mycluster.abcd1234.us-west-2.redshift.amazonaws.com
```

The leader node IP address is at the end of the ANSWER SECTION in the results, as shown following:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/dig_IP_address.png)

## Supported Platforms to Launch Your Cluster<a name="cluster-platforms"></a>

 Amazon Redshift clusters run in Amazon Elastic Compute Cloud \(Amazon EC2\) instances that are configured for the Amazon Redshift node type and size that you select\. You can launch an Amazon Redshift cluster in one of two platforms: EC2\-Classic or EC2\-VPC, which are the supported platforms for Amazon EC2 instances\. For more information about these platforms, go to [Supported Platforms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. The platform or platforms available to you depend on your AWS account settings\. 

**Note**  
 To prevent connection issues between SQL client tools and the Amazon Redshift database, we recommend that you either configure an inbound rule that enables the hosts to negotiate packet size or disable TCP/IP jumbo frames by setting the maximum transmission unit \(MTU\) to 1500 on the network interface \(NIC\) of your Amazon EC2 instances\. For more information about these approaches, see [Queries Appear to Hang and Sometimes Fail to Reach the Cluster](connecting-drop-issues.md)\. 

### EC2\-Classic Platform<a name="cluster-platforms-ec2-classic"></a>

 In the EC2\-Classic platform, your cluster runs in a single, flat network that you share with other AWS customers\. If you provision your cluster in the EC2\-Classic platform, you control access to your cluster by associating one or more Amazon Redshift cluster security groups with the cluster\. For more information, see [Amazon Redshift Cluster Security Groups](working-with-security-groups.md)\. 

### EC2\-VPC Platform<a name="cluster-platforms-ec2-vpc"></a>

 In the EC2\-VPC platform, your cluster runs in a virtual private cloud \(VPC\) that is logically isolated to your AWS account\. If you provision your cluster in the EC2\-VPC platform, you control access to your cluster by associating one or more VPC security groups with the cluster\. For more information, go to [Security Groups for Your VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. 

 To create a cluster in a VPC, you must first create an Amazon Redshift cluster subnet group by providing subnet information of your VPC, and then provide the subnet group when launching the cluster\. For more information, see [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)\. 

 For more information about Amazon Virtual Private Cloud \(Amazon VPC\), go to the [Amazon VPC product detail page](https://aws.amazon.com/vpc/)\. 

### Choose a Platform<a name="choose-rs-cluster-platform"></a>

 Your AWS account is capable of launching instances either into both platforms, or only into EC2\-VPC, on a region\-by\-region basis\. To determine which platform your account supports, and then launch a cluster, do the following: 

1.  Decide on the AWS region in which you want to deploy a cluster\. For a list of AWS regions in which Amazon Redshift is available, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\.

1.  Find out which Amazon EC2 platforms your account supports in the chosen AWS region\. You can find this information in the Amazon EC2 console\. For step\-by\-step instructions, go to [Supported Platforms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

1.  If your account supports both of the platforms, choose the one on which you want to deploy your Amazon Redshift cluster\. If your account supports only EC2\-VPC, you must deploy your cluster in VPC\. 

1.  Deploy your Amazon Redshift cluster\. You can deploy a cluster by using the Amazon Redshift console, or programmatically by using the Amazon Redshift API, CLI, or SDK libraries\. For more information about these options and links to the related documentation, see [What Is Amazon Redshift?](welcome.md)\. 

## Regions and Availability Zone Considerations<a name="az-considerations"></a>

 Amazon Redshift is available in several AWS regions\. By default, Amazon Redshift provisions your cluster in a randomly selected Availability Zone \(AZ\) within the AWS region that you select\. All the cluster nodes are provisioned in the same AZ\. 

 You can optionally request a specific AZ if Amazon Redshift is available in that AZ\. For example, if you already have an Amazon EC2 instance running in one AZ, you might want to create your Amazon Redshift cluster in the same AZ to reduce latency\. On the other hand, you might want to choose another AZ for higher availability\. Amazon Redshift might not be available in all AZs within a region\.

 For a list of supported AWS regions where you can provision an Amazon Redshift cluster, go to [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#redshift_region) in the *Amazon Web Services General Reference*\.

## Maintenance Windows<a name="rs-maintenance-windows"></a>

 Amazon Redshift periodically performs maintenance to apply upgrades to your cluster\. During these updates, your Amazon Redshift cluster is not available for normal operations\. 

 Amazon Redshift assigns a 30\-minute maintenance window at random from an 8\-hour block of time per region, occurring on a random day of the week \(Monday through Sunday, inclusive\)\. The following list shows the time blocks for each region from which the default maintenance windows are assigned: 

+ US East \(N\. Virginia\) region: 03:00–11:00 UTC

+ US East \(Ohio\) region: 03:00–11:00 UTC

+ US West \(N\. California\) region: 06:00–14:00 UTC

+ US West \(Oregon\) region: 06:00–14:00 UTC

+ Canada \(Central\) region: 03:00–11:00 UTC

+ Asia Pacific \(Mumbai\) region: 16:30–00:30 UTC

+ Asia Pacific \(Seoul\) region: 13:00–21:00 UTC

+ Asia Pacific \(Singapore\) region: 14:00–22:00 UTC

+ Asia Pacific \(Sydney\) region: 12:00–20:00 UTC

+ Asia Pacific \(Tokyo\) region: 13:00–21:00 UTC

+ EU \(Frankfurt\) region: 06:00–14:00 UTC

+ China \(Beijing\) region: 13:00–21:00 UTC

+ EU \(Ireland\) region: 22:00–06:00 UTC

+ EU \(London\) region: 22:00–06:00 UTC

+ South America \(São Paulo\) region: 19:00–03:00 UTC

If a maintenance event is scheduled for a given week, it will start during the assigned 30 minute maintenance window\. While Amazon Redshift is performing maintenance, it terminates any queries or other operations that are in progress\. Most maintenance completes during the 30 minute maintenance window, but some maintenance tasks might continue running after the window closes\. If there are no maintenance tasks to perform during the scheduled maintenance window, your cluster continues to operate normally until the next scheduled maintenance window\. 

You can change the scheduled maintenance window by modifying the cluster, either programmatically or by using the Amazon Redshift console\. The window must be at least 30 minutes and not longer than 24 hours\. For more information, see [Managing Clusters Using the Console](managing-clusters-console.md)\.

## Default Disk Space Alarm<a name="rs-clusters-default-disk-usage-alarm"></a>

When you create an Amazon Redshift cluster, you can optionally configure an Amazon CloudWatch alarm to monitor the average percentage of disk space that is used across all of the nodes in your cluster\. We’ll refer to this alarm as the *default disk space alarm*\. 

The purpose of default disk space alarm is to help you monitor the storage capacity of your cluster\. You can configure this alarm based on the needs of your data warehouse\. For example, you can use the warning as an indicator that you might need to resize your cluster, either to a different node type or to add nodes, or perhaps to purchase reserved nodes for future expansion\. 

The default disk space alarm triggers when disk usage reaches or exceeds a specified percentage for a certain number of times and at a specified duration\. By default, this alarm triggers when the percentage that you specify is reached, and stays at or above that percentage for five minutes or longer\. You can edit the default values after you launch the cluster\. 

When the CloudWatch alarm triggers, Amazon Simple Notification Service \(Amazon SNS\) sends a notification to specified recipients to warn them that the percentage threshold is reached\. Amazon SNS uses a topic to specify the recipients and message that are sent in a notification\. You can use an existing Amazon SNS topic; otherwise, a topic is created based on the settings that you specify when you launch the cluster\. You can edit the topic for this alarm after you launch the cluster\. For more information about creating Amazon SNS topics, see [Getting Started with Amazon Simple Notification Service](http://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\. 

After you launch the cluster, you can view and edit the alarm from the cluster’s **Status** window under **CloudWatch Alarms**\. The name is **percentage\-disk\-space\-used\-default\-<*string*>**\. You can open the alarm to view the Amazon SNS topic that it is associated with and edit alarm settings\. If you did not select an existing Amazon SNS topic to use, the one created for you is named **<*clustername*>\-default\-alarms \(<*recipient*>\)**; for example, **examplecluster\-default\-alarms \(notify@example\.com\)**\. 

 For more information about configuring and editing the default disk space alarm, see [Creating a Cluster](managing-clusters-console.md#create-cluster) and [Editing the Default Disk Space Alarm](managing-clusters-console.md#rs-mgmt-edit-default-disk-space-alarm)\. 

**Note**  
 If you delete your cluster, the alarm associated with the cluster will not be deleted but it will not trigger\. You can delete the alarm from the CloudWatch console if you no longer need it\. 

## Renaming Clusters<a name="rs-mgmt-rename-cluster"></a>

You can rename a cluster if you want the cluster to use a different name\. Because the endpoint to your cluster includes the cluster name \(also referred to as the *cluster identifier*\), the endpoint will change to use the new name after the rename finishes\. For example, if you have a cluster named `examplecluster` and rename it to `newcluster`, the endpoint will change to use the `newcluster` identifier\. Any applications that connect to the cluster must be updated with the new endpoint\. 

You might rename a cluster if you want to change the cluster to which your applications connect without having to change the endpoint in those applications\. In this case, you must first rename the original cluster and then change the second cluster to reuse the name of the original cluster prior to the rename\. Doing this is necessary because the cluster identifier must be unique within your account and region, so the original cluster and second cluster cannot have the same name \. You might do this if you restore a cluster from a snapshot and don’t want to change the connection properties of any dependent applications\. 

**Note**  
 If you delete the original cluster, you are responsible for deleting any unwanted cluster snapshots\. 

When you rename a cluster, the cluster status changes to `renaming` until the process finishes\. The old DNS name that was used by the cluster is immediately deleted, although it could remain cached for a few minutes\. The new DNS name for the renamed cluster becomes effective within about 10 minutes\. The renamed cluster is not available until the new name becomes effective\. The cluster will be rebooted and any existing connections to the cluster will be dropped\. After this completes, the endpoint will change to use the new name\. For this reason, you should stop queries from running before you start the rename and restart them after the rename finishes\. 

 Cluster snapshots are retained, and all snapshots associated with a cluster remain associated with that cluster after it is renamed\. For example, suppose you have a cluster that serves your production database and the cluster has several snapshots\. If you rename the cluster and then replace it in the production environment with a snapshot, the cluster that you renamed will still have those existing snapshots associated with it\. 

 Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) event notifications are associated with the name of the cluster\. If you rename the cluster, you need to update these accordingly\. You can update the CloudWatch alarms in the CloudWatch console, and you can update the Amazon SNS event notifications in the Amazon Redshift console on the **Events** pane\. The load and query data for the cluster continues to display data from before the rename and after the rename\. However, performance data is reset after the rename process finishes\. 

For more information, see [Modifying a Cluster](managing-clusters-console.md#modify-cluster)\.

## Shutting Down and Deleting Clusters<a name="rs-mgmt-shutdown-delete-cluster"></a>

You can shut down your cluster if you want to stop it from running and incurring charges\. When you shut it down, you can optionally create a final snapshot\. If you create a final snapshot, Amazon Redshift will create a manual snapshot of your cluster before shutting it down\. You can later restore that snapshot if you want to resume running the cluster and querying data\. 

If you no longer need your cluster and its data, you can shut it down without creating a final snapshot\. In this case, the cluster and data are deleted permanently\. For more information about shutting down and deleting clusters, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. 

Regardless of whether you shut down your cluster with a final manual snapshot, all automated snapshots associated with the cluster will be deleted after the cluster is shut down\. Any manual snapshots associated with the cluster are retained\. Any manual snapshots that are retained, including the optional final snapshot, are charged at the Amazon Simple Storage Service storage rate if you have no other clusters running when you shut down the cluster, or if you exceed the available free storage that is provided for your running Amazon Redshift clusters\. For more information about snapshot storage charges, go to the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 

## Cluster Status<a name="rs-mgmt-cluster-status"></a>

The cluster status displays the current state of the cluster\. The following table provides a description for each cluster status\. 


****  

| Status | Description | 
| --- | --- | 
| available | The cluster is running and available\. | 
| creating | Amazon Redshift is creating the cluster\. For more information, see [Creating a Cluster](managing-clusters-console.md#create-cluster)\. | 
| deleting | Amazon Redshift is deleting the cluster\. For more information, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. | 
| final\-snapshot | Amazon Redshift is taking a final snapshot of the cluster before deleting it\. For more information, see [Deleting a Cluster](managing-clusters-console.md#delete-cluster)\. | 
| hardware\-failure |  The cluster suffered a hardware failure\. If you have a single\-node cluster, the node cannot be replaced\. To recover your cluster, restore a snapshot\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.  | 
| incompatible\-hsm |  Amazon Redshift cannot connect to the hardware security module \(HSM\)\. Check the HSM configuration between the cluster and HSM\. For more information, see [About Encryption for Amazon Redshift Using Hardware Security Modules](working-with-db-encryption.md#working-with-HSM)\.  | 
| incompatible\-network |  There is an issue with the underlying network configuration\. Make sure that the VPC in which you launched the cluster exists and its settings are correct\. For more information, see [Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)](managing-clusters-vpc.md)\.  | 
| incompatible\-parameters | There is an issue with one or more parameter values in the associated parameter group, and the parameter value or values cannot be applied\. Modify the parameter group and update any invalid values\. For more information, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\.  | 
| incompatible\-restore |  There was an issue restoring the cluster from the snapshot\. Try restoring the cluster again with a different snapshot\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.  | 
| modifying |  Amazon Redshift is applying changes to the cluster\. For more information, see [Modifying a Cluster](managing-clusters-console.md#modify-cluster)\.  | 
| rebooting |  Amazon Redshift is rebooting the cluster\. For more information, see [Rebooting a Cluster](managing-clusters-console.md#reboot-cluster)\.  | 
| renaming |  Amazon Redshift is applying a new name to the cluster\. For more information, see [Renaming Clusters](#rs-mgmt-rename-cluster)\.  | 
| resizing |  Amazon Redshift is resizing the cluster\. For more information, see [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| rotating\-keys |  Amazon Redshift is rotating encryption keys for the cluster\. For more information, see [About Rotating Encryption Keys in Amazon Redshift](working-with-db-encryption.md#working-with-key-rotation)\.  | 
| storage\-full |  The cluster has reached its storage capacity\. Resize the cluster to add nodes or to choose a different node size\. For more information, see [Resizing a Cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| updating\-hsm |  Amazon Redshift is updating the HSM configuration\. For more information, see [About Encryption for Amazon Redshift Using Hardware Security Modules](working-with-db-encryption.md#working-with-HSM)\.  | 