# Amazon Redshift clusters<a name="working-with-clusters"></a>

In the following sections, you can learn the basics of creating a data warehouse by launching a set of compute nodes, called an Amazon Redshift* cluster\.*

**Topics**
+ [Overview of Amazon Redshift clusters](#working-with-clusters-overview)
+ [Use EC2\-VPC when you create your cluster](#cluster-platforms)
+ [Overview of RA3 node types](#rs-ra3-node-types)
+ [Upgrading to RA3 node types](#rs-upgrading-to-ra3)
+ [Upgrading from DC1 node types to DC2 node types](#rs-migrating-from-dc1-to-dc2)
+ [Upgrading a DS2 cluster on EC2\-Classic to EC2\-VPC](#rs-upgrading-ds2-cluster-ec2-classic-to-ec2-vpc)
+ [Region and Availability Zone considerations](#az-considerations)
+ [Cluster maintenance](#rs-cluster-maintenance)
+ [Default disk space alarm](#rs-clusters-default-disk-usage-alarm)
+ [Cluster status](#rs-mgmt-cluster-status)
+ [Overview of managing clusters in Amazon Redshift](managing-cluster-operations.md)
+ [Managing usage limits in Amazon Redshift](managing-cluster-usage-limits.md)
+ [Managing cluster relocation in Amazon Redshift](managing-cluster-recovery.md)
+ [Configuring Multi\-AZ deployment \(preview\)](managing-cluster-multi-az.md)
+ [Working with Redshift\-managed VPC endpoints in Amazon Redshift](managing-cluster-cross-vpc.md)
+ [Managing clusters using the console](managing-clusters-console.md)
+ [Managing clusters using the AWS CLI and Amazon Redshift API](manage-clusters-api-cli.md)
+ [Managing clusters using the AWS SDK for Java](managing-clusters-java.md)
+ [Managing clusters in a VPC](managing-clusters-vpc.md)
+ [Cluster version history](rs-mgmt-cluster-version-notes.md)

## Overview of Amazon Redshift clusters<a name="working-with-clusters-overview"></a>

An Amazon Redshift data warehouse is a collection of computing resources called *nodes*, which are organized into a group called a *cluster*\. Each cluster runs an Amazon Redshift engine and contains one or more databases\. 

**Note**  
At this time, Amazon Redshift version 1\.0 engine is available\. However, as the engine is updated, multiple Amazon Redshift engine versions might be available for selection\.

### Preview features when using Amazon Redshift clusters<a name="cluster-preview"></a>

You can create an Amazon Redshift cluster in **Preview** to test new features of Amazon Redshift\. You can't use those features in production or move your **Preview** cluster to a production cluster or a cluster on another track\. For preview terms and conditions, see *Beta and Previews* in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.

**To create a cluster in **Preview****

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Provisioned clusters dashboard**, and choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. A banner displays on the **Clusters** list page that introduces preview\. Choose the button **Create preview cluster** to open the create cluster page\.

1. Enter properties for your cluster\. Choose the **Preview track** that contains the features you want to test\. We recommend entering a name for the cluster that indicates that it is on a preview track\. Choose options for your cluster, including options labeled as **\-preview**, for the features you want to test\. For general information about creating clusters, see [Creating a cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster) in the *Amazon Redshift Management Guide*\.

1. Choose **Create preview cluster** to create a cluster in preview\.

1. When your preview cluster is available, use your SQL client to load and query data\.

The following features are currently available in preview clusters:
+ SQL MERGE command – see [MERGE](https://docs.aws.amazon.com/redshift/latest/dg/r_MERGE.html) in the *Amazon Redshift Database Developer Guide*\.
+ SQL SELECT GROUP BY aggregation extensions – see [Aggregation extensions](https://docs.aws.amazon.com/redshift/latest/dg/r_GROUP_BY_aggregation-extensions.html) in the *Amazon Redshift Database Developer Guide*\.
+ SUPER data type larger than 1 MB – see [Limitations](https://docs.aws.amazon.com/redshift/latest/dg/limitations-super.html) in the *Amazon Redshift Database Developer Guide*\.
+ Dynamic data masking – see [Dynamic data masking](https://docs.aws.amazon.com/redshift/latest/dg/t_ddm.html) in the *Amazon Redshift Database Developer Guide*\.
+ Auto\-copy – see [Continuous file ingestion from Amazon S3](https://docs.aws.amazon.com/redshift/latest/dg/loading-data-copy-job.html) in the *Amazon Redshift Database Developer Guide*\.
+ Lake Formation for data sharing – see [AWS Lake Formation\-managed Redshift datashares](https://docs.aws.amazon.com/redshift/latest/dg/lake-formation-datashare.html) in the *Amazon Redshift Database Developer Guide*\.
+ Multi\-AZ – see [Managing Multi\-AZ using the console](https://docs.aws.amazon.com/redshift/latest/mgmt/multi-az-console.html)\.
+ Query Data Catalog – see [Querying the AWS Glue Data Catalog \(preview\)](query-editor-v2-glue.md)\.

For information about preview in serverless workgroups, see [Preview when using Amazon Redshift Serverless](serverless-known-issues.md#serverless-preview)\.

### Clusters and nodes in Amazon Redshift<a name="rs-about-clusters-and-nodes"></a>

An Amazon Redshift cluster consists of nodes\. Each cluster has a leader node and one or more compute nodes\. The *leader node* receives queries from client applications, parses the queries, and develops query execution plans\. The leader node then coordinates the parallel execution of these plans with the compute nodes and aggregates the intermediate results from these nodes\. It then finally returns the results back to the client applications\. 

*Compute nodes* run the query execution plans and transmit data among themselves to serve these queries\. The intermediate results are sent to the leader node for aggregation before being sent back to the client applications\. For more information about leader nodes and compute nodes, see [Data warehouse system architecture](https://docs.aws.amazon.com/redshift/latest/dg/c_high_level_system_architecture.html) in the *Amazon Redshift Database Developer Guide*\. 

**Note**  
When you create a cluster on the Amazon Redshift console \([https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\), you can get a recommendation of your cluster configuration based on the size of your data and query characteristics\. To use this sizing calculator, look for **Help me choose** on the console in AWS Regions that support RA3 node types\. For more information, see [Creating a cluster](managing-clusters-console.md#create-cluster)\. 

When you launch a cluster, one option that you specify is the node type\. The node type determines the CPU, RAM, storage capacity, and storage drive type for each node\.  

Amazon Redshift offers different node types to accommodate your workloads, and we recommend choosing RA3 or DC2 depending on the required performance, data size, and expected data growth\. 

RA3 nodes with managed storage enable you to optimize your data warehouse by scaling and paying for compute and managed storage independently\. With RA3, you choose the number of nodes based on your performance requirements and only pay for the managed storage that you use\. Size your RA3 cluster based on the amount of data you process daily\. You launch clusters that use the RA3 node types in a virtual private cloud \(VPC\)\. You can't launch RA3 clusters in EC2\-Classic\. For more information, see [Creating a cluster in a VPC](getting-started-cluster-in-vpc.md)\.  

Amazon Redshift managed storage uses large, high\-performance SSDs in each RA3 node for fast local storage and Amazon S3 for longer\-term durable storage\. If the data in a node grows beyond the size of the large local SSDs, Amazon Redshift managed storage automatically offloads that data to Amazon S3\. You pay the same low rate for Amazon Redshift managed storage regardless of whether the data sits in high\-performance SSDs or Amazon S3\. For workloads that require ever\-growing storage, managed storage lets you automatically scale your data warehouse storage capacity without adding and paying for additional nodes\.

DC2 nodes enable you to have compute\-intensive data warehouses with local SSD storage included\. You choose the number of nodes you need based on data size and performance requirements\. DC2 nodes store your data locally for high performance, and as the data size grows, you can add more compute nodes to increase the storage capacity of the cluster\. For datasets under 1 TB \(compressed\), we recommend DC2 node types for the best performance at the lowest price\. If you expect your data to grow, we recommend using RA3 nodes so you can size compute and storage independently to achieve improved price and performance\. You launch clusters that use the DC2 node types in a virtual private cloud \(VPC\)\. You can't launch DC2 clusters in EC2\-Classic\. For more information, see [Creating a cluster in a VPC](getting-started-cluster-in-vpc.md)\.

DS2 nodes enable you to create large data warehouses using hard disk drives \(HDDs\), and we recommend using RA3 nodes instead\. If you are using DS2 nodes, see [Upgrading to RA3 node types](#rs-upgrading-to-ra3) for upgrade guidelines\. If you are using eight or more nodes of ds2\.xlarge, or any number of ds2\.8xlarge nodes, you can now upgrade to RA3 to get 2x more storage and improved performance for the same on\-demand cost\.

Node types are available in different sizes\. Node size and the number of nodes determine the total storage for a cluster\. For more information, see [Node type details](#rs-node-type-info)\. 

Some node types allow one node \(single\-node\) or two or more nodes \(multi\-node\)\. The minimum number of nodes for clusters of some node types is two nodes\. On a single\-node cluster, the node is shared for leader and compute functionality\. Single\-node clusters are not recommended for running production workloads\. On a multi\-node cluster, the leader node is separate from the compute nodes\. The leader node is the same node type as the compute nodes\. You only pay for compute nodes\. 

 Amazon Redshift applies quotas to resources for each AWS account in each AWS Region\. A *quota* restricts the number of resources that your account can create for a given resource type, such as nodes or snapshots, within an AWS Region\. For more information about the default quotas that apply to Amazon Redshift resources, see [Amazon Redshift Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift) in the *Amazon Web Services General Reference*\. To request an increase, submit an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 

The cost of your cluster depends on the AWS Region, node type, number of nodes, and  whether the nodes are reserved in advance\. For more information about the cost of nodes, see the [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/) page\. 

#### Node type details<a name="rs-node-type-info"></a>

 The following tables summarize the node specifications for each node type and size\. The headings in the tables have these meanings: 
+ *vCPU* is the number of virtual CPUs for each node\.
+ *RAM* is the amount of memory in gibibytes \(GiB\) for each node\.
+ *Default slices per node* is the number of slices into which a compute node is partitioned when a cluster is created or resized with classic resize\. 

  The number of slices per node might change if the cluster is resized using elastic resize\. However the total number of slices on all the compute nodes in the cluster remains the same after elastic resize\. 

  When you create a cluster with the restore from snapshot operation, the number of slices of the resulting cluster might change from the original cluster if you change the node type\. 
+ *Storage* is the capacity and type of storage for each node\.
+ *Node range* is the minimum and maximum number of nodes that Amazon Redshift supports for the node type and size\. 
**Note**  
You might be restricted to fewer nodes depending on the quota that is applied to your AWS account in the selected AWS Region\. To request an increase, submit an [Amazon Redshift Limit Increase Form](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-redshift)\. 
+ *Total capacity* is the total storage capacity for the cluster if you deploy the maximum number of nodes that is specified in the node range\.


**RA3 node types**  

| Node type | vCPU | RAM \(GiB\) | Default slices per node | Managed storage limit per node 1 | Node range with create cluster  | Total managed storage capacity 2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| ra3\.xlplus \(single\-node\) | 4 | 32 | 2 | 4 TB | 1 | 4 TB3 | 
| ra3\.xlplus \(multi\-node\) | 4 | 32 | 2 | 32 TB | 2–164 | 1024 TB4 | 
| ra3\.4xlarge | 12 | 96 | 4 | 128 TB | 2–325 | 8192 TB5 | 
| ra3\.16xlarge | 48 | 384 | 16 | 128 TB | 2–128 | 16,384 TB | 

1 The storage limit for Amazon Redshift managed storage\. This is a hard limit\.

2 Total managed storage limit is the maximum number of nodes times the managed storage limit per node\.

3 To resize a single\-node cluster to multi\-node, only classic resize is supported\.

4 You can create a cluster with the ra3\.xlplus \(multi\-node\) node type that has up to 16 nodes\. For multiple\-node clusters, you can resize with elastic resize to a maximum of 32 nodes\. 

5 You can create a cluster with the ra3\.4xlarge  node type with up to 32 nodes\. You can resize it with elastic resize to a maximum of 64 nodes\. 


**Dense storage node types**  

| Node type | vCPU | RAM \(GiB\) | Default slices per node | Storage per node | Node range | Total capacity | 
| --- | --- | --- | --- | --- | --- | --- | 
| ds2\.xlarge | 4 | 31 | 2 | 2 TB HDD | 1–32 | 64 TB | 
| ds2\.8xlarge | 36 | 244 | 16 | 16 TB HDD | 2–128 | 2 PB | 


**Dense compute node types**  

| Node type | vCPU | RAM \(GiB\) | Default slices per node | Storage per node | Node range | Total capacity | 
| --- | --- | --- | --- | --- | --- | --- | 
| dc2\.large | 2 | 15 | 2 | 160 GB NVMe\-SSD | 1–32 | 5\.12 TB | 
| dc2\.8xlarge | 32 | 244 | 16 | 2\.56 TB NVMe\-SSD | 2–128 | 326 TB | 
| dc1\.large1 | 2 | 15 | 2 | 160 GB SSD | 1–32 | 5\.12 TB | 
| dc1\.8xlarge1 | 32 | 244 | 32 | 2\.56 TB SSD | 2–128 | 326 TB | 

1 We recommend DC2 node types over DC1 node types\. For more information on how to upgrade, see [Upgrading from DC1 node types to DC2 node types](#rs-migrating-from-dc1-to-dc2)\. 
<a name="rs-old-node-names"></a>
**Previous node type names**  
In previous releases of Amazon Redshift, certain node types had different names\. You can use the previous names in the Amazon Redshift API and AWS CLI\. However, we recommend that you update any scripts that reference those names to use the current names instead\. The current and previous names are as follows\. 


| Current name | Previous names | 
| --- | --- | 
| ds2\.xlarge | ds1\.xlarge, dw\.hs1\.xlarge, dw1\.xlarge | 
| ds2\.8xlarge | ds1\.8xlarge, dw\.hs1\.8xlarge, dw1\.8xlarge | 
| dc1\.large | dw2\.large | 
| dc1\.8xlarge | dw2\.8xlarge | 

#### Determining the number of nodes<a name="how-many-nodes"></a>

Because Amazon Redshift distributes and runs queries in parallel across all of a cluster’s compute nodes, you can increase query performance by adding nodes to your cluster\.  When you run a cluster with at least two compute nodes, data on each node is mirrored on disks of another node to reduce the risk of incurring data loss\. 

You can monitor query performance in the Amazon Redshift console and with Amazon CloudWatch metrics\. You can also add or remove nodes as needed to achieve the balance between price and performance for your cluster\. When you request an additional node, Amazon Redshift takes care of all the details of deployment, load balancing, and data maintenance\. For more information about cluster performance, see [Monitoring Amazon Redshift cluster performance](metrics.md)\. 

Reserved nodes are appropriate for steady\-state production workloads, and offer significant discounts over on\-demand nodes\. You can purchase reserved nodes after running experiments and proof\-of\-concepts to validate your production configuration\. For more information, see [Purchasing Amazon Redshift reserved nodes](purchase-reserved-node-instance.md)\. 

When you pause a cluster, you suspend on\-demand billing during the time the cluster is paused\. During this paused time, you only pay for backup storage\. This frees you from planning and purchasing data warehouse capacity ahead of your needs, and enables you to cost\-effectively manage environments for development or test purposes\. 

For information about pricing of on\-demand and reserved nodes, see [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/)\. 

## Use EC2\-VPC when you create your cluster<a name="cluster-platforms"></a>

 Amazon Redshift clusters run in Amazon EC2 instances that are configured for the Amazon Redshift node type and size that you select\. Create your cluster using EC2\-VPC\. If you are still using EC2\-Classic, we recommend you use EC2\-VPC to get improved performance and security\. For more information about these networking platforms, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. Your AWS account settings determine whether EC2\-VPC or EC2\-Classic are available to you\. 

**Note**  
To prevent connection issues between SQL client tools and the Amazon Redshift database, we recommend doing one of two things\. You can configure an inbound rule that enables the hosts to negotiate packet size\. Alternatively, you can disable TCP/IP jumbo frames by setting the maximum transmission unit \(MTU\) to 1500 on the network interface \(NIC\) of your Amazon EC2 instances\. For more information about these approaches, see [Queries appear to hang and sometimes fail to reach the cluster](connecting-drop-issues.md)\. 

### EC2\-VPC<a name="cluster-platforms-ec2-vpc"></a>

When using EC2\-VPC, your cluster runs in a virtual private cloud \(VPC\) that is logically isolated to your AWS account\. If you provision your cluster in the EC2\-VPC, you control access to your cluster by associating one or more VPC security groups with the cluster\. For more information, see [Security Groups for Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. 

To create a cluster in a VPC, you must first create an Amazon Redshift cluster subnet group by providing subnet information of your VPC, and then provide the subnet group when launching the cluster\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\. 

 For more information about Amazon Virtual Private Cloud \(Amazon VPC\), see the [Amazon VPC product detail page](https://aws.amazon.com/vpc/)\. 

### EC2\-Classic<a name="cluster-platforms-ec2-classic"></a>


|  | 
| --- |
| The EC2\-Classic platform is retiring on August 15, 2022\. We recommend that you migrate your clusters from EC2\-Classic platform to a EC2\-VPC platform\. For more information, see [Upgrading a DS2 cluster on EC2\-Classic to EC2\-VPC](#rs-upgrading-ds2-cluster-ec2-classic-to-ec2-vpc) and [EC2\-Classic Networking is Retiring – Here’s How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/)\. | 

In EC2\-Classic, your cluster runs in a single, flat network that you share with other AWS customers\. If you provision your cluster in the EC2\-Classic, you control access to your cluster by associating one or more Amazon Redshift cluster security groups with the cluster\. For more information, see [Amazon Redshift cluster security groups](working-with-security-groups.md)\. 

### Launch a cluster<a name="choose-rs-cluster-platform"></a>

Your AWS account can either launch instances of both EC2\-VPC and EC2\-Classic, or only EC2\-VPC, on a region\-by\-region basis\. To determine which networking platform your account supports, and then launch a cluster, do the following: 

1. Decide which AWS Region you want to deploy a cluster\. For a list of AWS Regions in which Amazon Redshift is available, see [Amazon Redshift endpoints](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\.

1. Find out which Amazon EC2 platforms your account supports in the chosen AWS Region\. You can find this information in the Amazon EC2 console\. For step\-by\-step instructions, see [Supported Platforms](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-supported-platforms.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

1. If your account supports both of the platforms, we recommend EC2\-VPC\. If your account supports only EC2\-VPC, you must deploy your cluster in VPC\. 

1. Launch your Amazon Redshift cluster\. You can create a cluster using the Amazon Redshift console, or by using the Amazon Redshift API, AWS CLI, or SDK libraries\. For more information about these options and links to the related documentation, see [What is Amazon Redshift?](welcome.md)\. 

## Overview of RA3 node types<a name="rs-ra3-node-types"></a>

We recommend that you upgrade existing workloads running on DS2 node type clusters to RA3 node types to take advantage of improved performance and to get more storage capacity\. RA3 nodes provide the following advantages: 
+ They are flexible to grow your compute capacity without increasing your storage costs\. And they scale your storage without over\-provisioning compute capacity\.
+ They use high performance SSDs for your hot data and Amazon S3 for cold data\. Thus they provide ease of use, cost\-effective storage, and high query performance\.
+ They use high bandwidth networking built on the AWS Nitro System to further reduce the time taken for data to be offloaded to and retrieved from Amazon S3\.

Consider choosing RA3 node types in these cases: 
+ You need the flexibility to scale and pay for compute separate from storage\.
+ You query a fraction of your total data\. 
+ Your data volume is growing rapidly or is expected to grow rapidly\. 
+ You want the flexibility to size the cluster based only on your performance needs\. 

To use RA3 node types, your AWS Region must support RA3\. For more information, see [RA3 node type availability in AWS Regions](#ra3-regions)\. 

**Important**  
You can use ra3\.xlplus node types only with cluster version 1\.0\.21262 or later\. You can view the version of an existing cluster with the Amazon Redshift console\. For more information, see [Determining the cluster maintenance version](#cluster-maintenance-version)\.   
Make sure that you use the new Amazon Redshift console when working with RA3 node types\.   
In addition, to use RA3 node types with Amazon Redshift operations that use the maintenance track, the maintenance track value must be set to a cluster version that supports RA3\. For more information about maintenance tracks, see [Choosing cluster maintenance tracks](#rs-mgmt-maintenance-tracks)\. 

Consider the following when using single\-node RA3 node types\.
+ Datasharing producers and consumers are supported\.
+ To change node types, only classic resize is supported\. Changing the node type with elastic resize or snapshot restore is not supported\. The following scenarios are supported: 
  + Classic resize of a 1\-node ds2\.xlarge to a 1\-node ra3\.xlplus, and vice versa\.
  + Classic resize of a 1\-node ds2\.xlarge to a multiple\-node ra3\.xlplus, and vice versa\.
  + Classic resize of a multiple\-node ds2\.xlarge to a 1\-node ra3\.xlplus, and vice versa\.
  + Classic resize of a 1\-node dc2\.xlarge to a 1\-node ra3\.xlplus, and vice versa\.
  + Classic resize of a 1\-node dc2\.xlarge to a multiple\-node ra3\.xlplus, and vice versa\.
  + Classic resize of a multiple\-node dc2\.xlarge to a 1\-node ra3\.xlplus, and vice versa\.

### Working with Amazon Redshift managed storage<a name="rs-managed-storage"></a>

With Amazon Redshift managed storage, you can store and process all your data in Amazon Redshift while getting more flexibility to scale compute and storage capacity separately\. You continue to ingest data with the COPY or INSERT command\.   To optimize performance and manage automatic data placement across tiers of storage, Amazon Redshift takes advantage of optimizations such as data block temperature, data block age, and workload patterns\. When needed, Amazon Redshift scales storage automatically to Amazon S3 without requiring any manual action\.  

For information about storage costs, see [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/)\. 

### Managing RA3 node types<a name="rs-managing-ra3"></a>

To take advantage of separating compute from storage, you can create or upgrade your cluster with the RA3 node type\. To use the RA3 node types, create your clusters in a virtual private cloud \(EC2\-VPC\)\. 

To change the number of nodes of Amazon Redshift cluster with an RA3 node type, do one of the following:
+ Add or remove nodes with the elastic resize operation\. In some situations, removing nodes from a RA3 cluster isn't allowed with elastic resize\. For example, when a 2:1 node count upgrade puts the number of slices per node at 32\. For more information, see [Resizing clusters](managing-cluster-operations.md#rs-resize-tutorial)\. If elastic resize isn't available, use classic resize\. 
+ Add or remove nodes with the classic resize operation\. Choose this option when you are resizing to a configuration that isn't available through elastic resize\. Elastic resize is quicker than classic resize\. For more information, see [Resizing clusters](managing-cluster-operations.md#rs-resize-tutorial)\. 

### RA3 node type availability in AWS Regions<a name="ra3-regions"></a>

The RA3 node types are available only in the following AWS Regions: 
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ US East \(Ohio\) Region \(us\-east\-2\)
+ US West \(N\. California\) Region \(us\-west\-1\)
+ US West \(Oregon\) Region \(us\-west\-2\) 
+ Africa \(Cape Town\) Region \(af\-south\-1\) 
+ Asia Pacific \(Hong Kong\) Region \(ap\-east\-1\) 
+ Asia Pacific \(Hyderabad\) Region \(ap\-south\-2\) 
+ Asia Pacific \(Jakarta\) Region \(ap\-southeast\-3\) – only ra3\.4xlarge and ra3\.16xlarge node types are supported 
+ Asia Pacific \(Melbourne\) Region \(ap\-southeast\-4\)
+ Asia Pacific \(Mumbai\) Region \(ap\-south\-1\) 
+ Asia Pacific \(Osaka\) Region \(ap\-northeast\-3\) 
+ Asia Pacific \(Seoul\) Region \(ap\-northeast\-2\)
+ Asia Pacific \(Singapore\) Region \(ap\-southeast\-1\) 
+ Asia Pacific \(Sydney\) Region \(ap\-southeast\-2\)
+ Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)
+ Canada \(Central\) Region \(ca\-central\-1\)
+ China \(Beijing\) Region \(cn\-north\-1\) 
+ China \(Ningxia\) Region \(cn\-northwest\-1\) 
+ Europe \(Frankfurt\) Region \(eu\-central\-1\) 
+ Europe \(Zurich\) Region \(eu\-central\-2\) 
+ Europe \(Ireland\) Region \(eu\-west\-1\)
+ Europe \(London\) Region \(eu\-west\-2\)
+ Europe \(Milan\) Region \(eu\-south\-1\) 
+ Europe \(Spain\) Region \(eu\-south\-2\)
+ Europe \(Paris\) Region \(eu\-west\-3\)
+ Europe \(Stockholm\) Region \(eu\-north\-1\) 
+ Middle East \(Bahrain\) Region \(me\-south\-1\) 
+ Middle East \(UAE\) Region \(me\-central\-1\) – only ra3\.4xlarge and ra3\.16xlarge node types are supported 
+ South America \(São Paulo\) Region \(sa\-east\-1\)
+ AWS GovCloud \(US\-East\) \(us\-gov\-east\-1\)
+ AWS GovCloud \(US\-West\) \(us\-gov\-west\-1\)

## Upgrading to RA3 node types<a name="rs-upgrading-to-ra3"></a>

To upgrade your existing node type to RA3, you have the following options to change the node type: 
+ Restore from a snapshot – Amazon Redshift uses the most recent snapshot of your DS2 or DC2 cluster and restores it to create a new RA3 cluster\. As soon as the cluster creation is complete \(usually within minutes\), RA3 nodes are ready to run your full production workload\. As compute is separate from storage, hot data is brought in to the local cache at fast speeds thanks to a large networking bandwidth\. If you restore from the latest DS2 or DC2 snapshot, RA3 preserves hot block information of the DS2 or DC2 workload and populates its local cache with the hottest blocks\.   For more information, see [Restoring a cluster from a snapshot](working-with-snapshots.md#working-with-snapshot-restore-cluster-from-snapshot)\. 

  To keep the same endpoint for your applications and users, you can rename the new RA3 cluster with the same name as the original DS2 or DC2 cluster\. To rename the cluster, modify the cluster in the Amazon Redshift console or `ModifyCluster` API operation\. For more information, see [Renaming clusters](managing-cluster-operations.md#rs-mgmt-rename-cluster) or [`ModifyCluster` API operation](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyCluster.html) in the *Amazon Redshift API Reference*\.
+ Elastic resize – resize the cluster using elastic resize\. When you use elastic resize to change node type, Amazon Redshift automatically creates a snapshot, creates a new cluster, deletes the old cluster, and renames the new cluster\. The elastic resize operation can be run on\-demand or can be scheduled to run at a future time\. You can quickly upgrade your existing DS2 or DC2 node type clusters to RA3 with elastic resize\. For more information, see [Elastic resize](managing-cluster-operations.md#elastic-resize)\. 

The following table shows recommendations when upgrading to RA3 node types\. \(These recommendations also apply to reserved nodes\.\)


| Existing node type | Existing number of nodes | Recommended new node type | Upgrade action | 
| --- | --- | --- | --- | 
| ds2\.xlarge | 1 | ra3\.xlplus | Create a 1\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 2 | ra3\.xlplus | Create a 2\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 3 | ra3\.xlplus | Create a 2\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 4 | ra3\.xlplus | Create a 3\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 5 | ra3\.xlplus | Create a 4\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 6 | ra3\.xlplus | Create a 4\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 7 | ra3\.xlplus | Create a 5\-node ra3\.xlplus cluster1\. | 
| ds2\.xlarge | 8 | ra3\.4xlarge | Create a 2\-node ra3\.4xlarge cluster1\. | 
| ds2\.xlarge | 9 | ra3\.4xlarge | Create a 3\-node ra3\.4xlarge cluster1\. | 
| ds2\.xlarge | 10 | ra3\.4xlarge | Create a 3\-node ra3\.4xlarge cluster1\. | 
| ds2\.xlarge | 11\-128 | ra3\.4xlarge | Create 1 node of ra3\.4xlarge for every 4 nodes of ds2\.xlarge1\. | 
| ds2\.8xlarge | 2–15 | ra3\.4xlarge | Create 2 nodes of ra3\.4xlarge for every 1 node of ds2\.8xlarge1\. | 
| ds2\.8xlarge | 16–128 | ra3\.16xlarge | Create 1 node of ra3\.16xlarge for every 2 nodes of ds2\.8xlarge1\. | 
| dc2\.8xlarge | 2–15 | ra3\.4xlarge | Create 2 nodes of ra3\.4xlarge for every 1 node of dc2\.8xlarge1\.  | 
| dc2\.8xlarge | 16–128 | ra3\.16xlarge | Create 1 node of ra3\.16xlarge for every 2 nodes of dc2\.8xlarge1\. | 
| dc2\.large | 1–4 | none | Keep existing dc2\.large cluster\.  | 
| dc2\.large | 5–15 | ra3\.xlplus | Create 3 nodes of ra3\.xlplus for every 8 nodes of dc2\.large1\.  | 
| dc2\.large | 16–32 | ra3\.4xlarge | Create 1 node of ra3\.4xlarge for every 8 nodes of dc2\.large1,2\. | 

1Extra nodes might be needed depending on workload requirements\. Add or remove nodes based on the compute requirements of your required query performance\. 

2Clusters with the dc2\.large node type are limited to 32 nodes\.

The minimum number of nodes for some RA3 node types is 2 nodes\. Take this into consideration when creating an RA3 cluster\.

### Upgrade DS2 reserved nodes to RA3 reserved nodes during elastic resize or snapshot restore<a name="rs-upgrading-to-ra3-reserved-nodes"></a>

If you have DS2 reserved nodes, you can upgrade them with the RA3 reserved\-node upgrade feature, using the Amazon Redshift console or the AWS CLI\. On the console, there are a couple of ways you can do this\.

One way is to upgrade DS2 reserved nodes to RA3 during an elastic resize\. If you have reserved nodes and select RA3 nodes, the console walks you through the reserved\-node upgrade process\. From a technical standpoint, elastic resize works the same for both reserved nodes and nodes that aren't reserved\.

If you change the cluster size from the recommended size, when you configure the elastic resize, RA3 reserved\-node upgrade isn't available and doesn't appear on the console\. \(You can still upgrade DS2 reserved nodes to RA3, but the resize doesn't include RA3 reserved\-node upgrade as part of the process\.\) Also note that the cluster size you want may not be available because of cluster\-size limitations for elastic resize\. For instance, if you have a 4\-node DS2 reserved\-node cluster, you may not be able to select a 3\-node RA3 cluster\. In this case, you can perform a classic resize to get the cluster size you want\.

A couple of steps occur after the cluster resize\. First, data is migrated to the RA3 cluster\. Then, the DS2 reserved\-node lease is converted to an RA3 reserved\-node lease\. Note that the time for data migration can vary, depending on the size of the cluster and whether the resize is elastic or classic\. In the case of a classic resize, it's common for data migration to take several hours\. 

After you start the resize, track progress by viewing messages in **Events**, available on the **Amazon Redshift dashboard**\. There is an event notification for the resize, and another for the reserved\-node upgrade\. For information about working with events, see [Amazon Redshift events](working-with-events.md)\. After the resize, the active resized cluster appears on the AWS Management Console\. You can also view the converted RA3 reserved\-node lease\. The source DS2 reserved node\(s\) may still appear on the console for about a day\. You aren't billed for them\. **Don't delete the source DS2 reserved node until you verify that the RA3 cluster is active and the converted reserved\-node lease is generated\.**

The other way that you can use the RA3 reserved\-node upgrade feature is when you restore from a snapshot\. If you select the RA3 node type and you have DS2 reserved nodes, you can select the RA3 reserved\-node upgrade feature at that point\. When you restore from a snapshot, it's restored to an RA3 reserved\-node cluster\. As mentioned previously, if you choose a cluster size other than the recommended size, the RA3 reserved\-node upgrade selection isn't available on the console\.

For more information about resizing your cluster and upgrading nodes, see [Getting the leader node IP address](managing-cluster-operations.md#cluster-resize-intro)\. There, you can find a detailed description of the process and also answers about what happens to the cluster and to the data when you resize\. For more detail about the steps in the elastic\-resize process, see [Elastic resize](managing-cluster-operations.md#elastic-resize)\. For more information about restoring from a snapshot, see [Restoring a cluster from a snapshot](working-with-snapshots.md#working-with-snapshot-restore-cluster-from-snapshot)\.

If you have more questions about upgrading reserved nodes to RA3, for instance about upgrading DC2 reserved nodes to RA3, contact AWS Support\. For information about pricing of on\-demand and reserved nodes, see [Amazon Redshift pricing](https://aws.amazon.com/redshift/pricing/)\.

If you have already purchased DS2 reserved nodes, contact AWS for help with converting DS2 reserved nodes to RA3 reserved nodes\. To contact AWS for more information, see [Amazon Redshift RA3 instances with managed storage](https://pages.awscloud.com/Redshift_RA3instances.html)\. 

## Upgrading from DC1 node types to DC2 node types<a name="rs-migrating-from-dc1-to-dc2"></a>

To take advantage of performance improvements, you can upgrade your DC1 clusters to DC2 node types\. 

Clusters that use the DC2 node type must be launched in a virtual private cloud \(EC2\-VPC\)\.  

If your DC1 cluster is not in a VPC: 

1. Create a snapshot of your DC1 cluster\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\.

1. Create a VPC, or choose an existing VPC in your account\. For more information, see [Managing clusters in a VPC](managing-clusters-vpc.md)\. 

1. Restore your snapshot to a new DC2 cluster in the VPC\. For more information, see [Restoring a cluster from a snapshot](working-with-snapshots.md#working-with-snapshot-restore-cluster-from-snapshot)\. 

If your DC1 cluster is already in a VPC, choose one of the following methods: 
+ Resize your DC1 cluster and change the node type to DC2 as part of the operation\. Your cluster is not available for a period of time during the resize operation\. For more information, see [Resizing clusters in Amazon Redshift](managing-cluster-operations.md#rs-resize-tutorial)\. 
+ Create a snapshot of your DC1 cluster, then restore your snapshot to a DC2 cluster in the VPC\. For more information, see [Restoring a cluster from a snapshot](working-with-snapshots.md#working-with-snapshot-restore-cluster-from-snapshot)\. 

Consider the following when upgrading from DC1 to DC2 node types\.
+ DC1 clusters that are 100% full might not upgrade to an equivalent number of DC2 nodes\. If more disk space is needed, you can:
  + Resize to a configuration with more available disk space\. 
  + Clean up unneeded data by truncating tables or deleting rows\. 
+ DC2 clusters don't support EC2\-Classic networking\. If your DC1 cluster is not running in a VPC, create one for your DC2 migration\. For more information, see [Managing clusters in a VPC](managing-clusters-vpc.md)\. 
+ If you resize the cluster, it might be put into read\-only mode for the duration of the operation\.  For more information, see [Resizing clusters in Amazon Redshift](managing-cluster-operations.md#rs-resize-tutorial)\. 
+ If you have purchased DC1 reserved nodes, you can upgrade your DC1 reserved nodes to DC2 nodes for the remainder of your term\. For more information about how to change your reservation with the AWS CLI, see [Upgrading reserved nodes with the AWS CLI](purchase-reserved-node-offering-console.md#reserved-node-upgrade-cli)\. 
+ If you use restore to upgrade from dc1\.large to dc2\.large, and change the number of nodes, then the snapshot must have been created at cluster version 1\.0\.10013 or later\. 
+ If you use restore to upgrade from dc1\.8xlarge to dc2\.8xlarge, then the snapshot must have been created at cluster version 1\.0\.10013 or later\. 
+ If you use elastic resize to upgrade from DC1 to DC2, and change the number of nodes, then the cluster must be at cluster version 1\.0\.10013 or later\. 
+ If a snapshot of dc1\.8xlarge cluster to upgrade is from a cluster earlier than version 1\.0\.10013, then first restore the snapshot from the dc1\.8xlarge cluster into a new dc1\.8xlarge cluster with the same number of nodes\. Then use one of the following methods to upgrade the new dc1\.8xlarge: 
  + Use a snapshot from the new restored cluster to upgrade to dc2\.8xlarge\. 
  + Use elastic resize to upgrade the new restored cluster to dc2\.8xlarge\. 

## Upgrading a DS2 cluster on EC2\-Classic to EC2\-VPC<a name="rs-upgrading-ds2-cluster-ec2-classic-to-ec2-vpc"></a>

Amazon Redshift clusters run in Amazon EC2 instances that are configured for the Amazon Redshift node type and size that you choose\. We recommend that you upgrade your cluster on EC2\-Classic to launch in a VPC using EC2\-VPC for improved performance and security\.

**To upgrade your DS2 cluster on EC2\-Classic to EC2\-VPC**

1. Create a snapshot of your DS2 cluster\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\.

1. Create a VPC, or choose an existing VPC in your account\. For more information, see [Managing clusters in a VPC](managing-clusters-vpc.md)\. 

1. Restore your snapshot to a new DS2 cluster in the VPC\. For more information, see [Restoring a cluster from a snapshot](working-with-snapshots.md#working-with-snapshot-restore-cluster-from-snapshot)\. 

## Region and Availability Zone considerations<a name="az-considerations"></a>

 Amazon Redshift is available in several AWS Regions\. By default, Amazon Redshift provisions your cluster in a randomly selected Availability Zone \(AZ\) within the AWS Region that you choose\. All the cluster nodes are provisioned in the same Availability Zone\. 

 You can optionally request a specific Availability Zone if Amazon Redshift is available in that zone\. For example, if you already have an Amazon EC2 instance running in one Availability Zone, you might want to create your Amazon Redshift cluster in the same zone to reduce latency\. On the other hand, you might want to choose another Availability Zone for higher availability\. Amazon Redshift might not be available in all Availability Zones within an AWS Region\.

 For a list of supported AWS Regions where you can provision an Amazon Redshift cluster, see [Amazon Redshift endpoints](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\.

## Cluster maintenance<a name="rs-cluster-maintenance"></a>

Amazon Redshift periodically performs maintenance to apply upgrades to your cluster\. During these updates, your Amazon Redshift cluster isn't available for normal operations\. You have several ways to control how we maintain your cluster\. For example, you can control when we deploy updates to your clusters\. You can also choose whether your cluster runs the most recently released version, or the version released previously to the most recently released version\. Finally, you have the option to defer non\-mandatory maintenance updates for a period of time\.

**Topics**
+ [Maintenance windows](#rs-maintenance-windows)
+ [Deferring maintenance](#rs-mgmt-defer-maintenance)
+ [Choosing cluster maintenance tracks](#rs-mgmt-maintenance-tracks)
+ [Managing cluster versions](#rs-mgmt-cluster-version)
+ [Rolling back the cluster version](#rs-mgmt-rollback-version)
+ [Determining the cluster maintenance version](#cluster-maintenance-version)

### Maintenance windows<a name="rs-maintenance-windows"></a>

 Amazon Redshift assigns a 30\-minute maintenance window at random from an 8\-hour block of time per AWS Region, occurring on a random day of the week \(Monday through Sunday, inclusive\)\. 

#### Default maintenance windows<a name="rs-default-maintenance-windows"></a>

The following list shows the time blocks for each AWS Region from which the default maintenance windows are assigned: 
+ US East \(N\. Virginia\) Region: 03:00–11:00 UTC
+ US East \(Ohio\) Region: 03:00–11:00 UTC
+ US West \(N\. California\) Region: 06:00–14:00 UTC
+ US West \(Oregon\) Region: 06:00–14:00 UTC
+ Africa \(Cape Town\) Region: 20:00–04:00 UTC
+ Asia Pacific \(Hong Kong\) Region: 13:00–21:00 UTC
+ Asia Pacific \(Hyderabad\) Region: 16:30–00:30 UTC
+ Asia Pacific \(Jakarta\) Region: 15:00–23:00 UTC
+ Asia Pacific \(Melbourne\) Region: 12:00–20:00 UTC
+ Asia Pacific \(Mumbai\) Region: 16:30–00:30 UTC
+ Asia Pacific \(Osaka\) Region: 13:00–21:00 UTC
+ Asia Pacific \(Seoul\) Region: 13:00–21:00 UTC
+ Asia Pacific \(Singapore\) Region: 14:00–22:00 UTC
+ Asia Pacific \(Sydney\) Region: 12:00–20:00 UTC
+ Asia Pacific \(Tokyo\) Region: 13:00–21:00 UTC
+ Canada \(Central\) Region: 03:00–11:00 UTC
+ China \(Beijing\) Region: 13:00–21:00 UTC
+ China \(Ningxia\) Region: 13:00–21:00 UTC
+ Europe \(Frankfurt\) Region: 06:00–14:00 UTC
+ Europe \(Ireland\) Region: 22:00–06:00 UTC
+ Europe \(London\) Region: 22:00–06:00 UTC
+ Europe \(Milan\) Region: 21:00–05:00 UTC
+ Europe \(Paris\) Region: 23:00–07:00 UTC
+ Europe \(Stockholm\) Region: 23:00–07:00 UTC
+ Europe \(Zurich\) Region: 20:00–04:00 UTC
+ Europe \(Spain\) Region: 21:00–05:00 UTC
+ Middle East \(Bahrain\) Region: 13:00–21:00 UTC
+ Middle East \(UAE\) Region: 18:00–02:00 UTC
+ South America \(São Paulo\) Region: 19:00–03:00 UTC

If a maintenance event is scheduled for a given week, it starts during the assigned 30\-minute maintenance window\. While Amazon Redshift is performing maintenance, it terminates any queries or other operations that are in progress\. Most maintenance completes during the 30\-minute maintenance window, but some maintenance tasks might continue running after the window closes\. If there are no maintenance tasks to perform during the scheduled maintenance window, your cluster continues to operate normally until the next scheduled maintenance window\. 

You can change the scheduled maintenance window by modifying the cluster, either programmatically or by using the Amazon Redshift console\. The window must be at least 30 minutes and not longer than 24 hours\. For more information, see [Managing clusters using the console](managing-clusters-console.md)\.

### Deferring maintenance<a name="rs-mgmt-defer-maintenance"></a>

If you need to reschedule your cluster’s maintenance window, you have the option to defer maintenance by up to 45 days\. For example, if your cluster's maintenance window is set to Wednesday 8:30 – 9:00 UTC and you need to have access to your cluster at that time, you can defer the maintenance to a later time period\. We will not perform any maintenance on your cluster when you have specified a deferment, unless we need to update hardware\.

If we need to update hardware or make other mandatory updates during your period of deferment, we notify you and make the required changes\. Your cluster isn't available during these updates\.

If you defer your cluster's maintenance, the maintenance window following your period of deferment is mandatory\. It can't be deferred\.

**Note**  
You can't defer maintenance after it has started\.

For more information, see [Modifying a cluster](managing-clusters-console.md#modify-cluster)\.

### Choosing cluster maintenance tracks<a name="rs-mgmt-maintenance-tracks"></a><a name="rs-maintenance-tracks"></a>

When Amazon Redshift releases a new cluster version, your cluster is updated during its maintenance window\. You can control whether your cluster is updated to the most recent approved release or to the previous release\. 

The maintenance track controls which cluster version is applied during a maintenance window\. When Amazon Redshift releases a new cluster version, that version is assigned to the *current* track, and the previous version is assigned to the *trailing* track\. To set the maintenance track for the cluster, specify one of the following values:
+ **Current** – Use the most current approved cluster version\.
+ **Trailing** – Use the cluster version before the current version\.
+ **Preview** – Use the cluster version that contains new features available for preview\. 

For example, suppose that your cluster is currently running version 1\.0\.2762 and the Amazon Redshift current version is 1\.0\.3072\. If you set the maintenance track value to **Current**, your cluster is updated to version 1\.0\.3072 \(the next approved release\) during the next maintenance window\. If you set the maintenance track value to **Trailing**, your cluster isn't updated until there is a new release after 1\.0\.3072\. 

**Preview tracks**

A **Preview** track might not always be available to choose\. When you choose a **Preview** track, a track name must also be selected\. Preview tracks and its related resources are temporary, have functional limitations, and might not contain all current Amazon Redshift features available in other tracks\. When working with preview tracks: 
+ Use the new Amazon Redshift console when working with preview tracks\. For example, when you create a cluster to use with preview features\. 
+ You can't switch a cluster from one preview track to another\. 
+ You can't switch a cluster to a preview track from a current or trailing track\. 
+ You can't switch a cluster from a preview track to a current or trailing track\. 
+ You can't restore from a snapshot created from a different preview track\.
+ You can only use the preview track when creating a new cluster, or when restoring from a snapshot\. 
+ You can't restore from a snapshot created from a different preview track, or with a cluster maintenance version later than the preview track cluster version\. For example, when you restore a cluster to a preview track, you can only use a snapshot created from an earlier cluster maintenance version than that of the preview track\. 

**Switching between maintenance tracks**

Changing tracks for a cluster is generally a one\-time decision\. You should exercise caution in changing tracks\. If you change the maintenance track from **Trailing** to **Current**, we will update the cluster to the **Current** track release version during the next maintenance window\. However, if you change the cluster's maintenance track to **Trailing** we won't update your cluster until there is a new release after the **Current** track release version\. 

**Maintenance tracks and restore**

A snapshot inherits the source cluster's maintenance track\. If you change the source cluster's maintenance track after you take a snapshot, the snapshot and the source cluster are on different tracks\. When you restore from the snapshot, the new cluster will be on the maintenance track that was inherited from the source cluster\. You can change the maintenance track after the restore operation completes\. Resizing a cluster doesn't affect the cluster's maintenance track\. 

### Managing cluster versions<a name="rs-mgmt-cluster-version"></a>

A maintenance track is a series of releases\. You can decide if your cluster is on the **Current** track or the **Trailing** track\. If you put your cluster on the **Current** track, it will always be upgraded to the most recent cluster release version during its maintenance window\. If you put your cluster on the **Trailing** track, it will always run the cluster release version that was released immediately before the most recently released version\. 

The **Release status** column in the Amazon Redshift console list of clusters indicates whether one of your clusters is available for upgrade\. 

### Rolling back the cluster version<a name="rs-mgmt-rollback-version"></a>

If your cluster is up to date with the latest cluster version, you can choose to roll it back to the previous version\.

For detailed information about features and improvements included with each cluster version, see [Cluster version history](rs-mgmt-cluster-version-notes.md)\.

**To roll back to a previous cluster version**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster to roll back\. 

1. For **Actions**, choose **Roll back cluster version**\. The **Roll back cluster version** page appears\.

1. If there is a version available for roll back, follow the instructions on the page\. 

1. Choose **Roll back now**\. 

### Determining the cluster maintenance version<a name="cluster-maintenance-version"></a>

You can determine the Amazon Redshift engine and database version with the Amazon Redshift console\.

**To find the version of a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster name from the list to open its details\. The details of the cluster are displayed, which can include **Cluster performance**, **Query monitoring**, **Databases**, **Datashares**, **Schedules**, **Maintenance**, and **Properties** tabs\. 

1. Choose the **Maintenance** tab for more details\. 

1. In the **Maintenance** section, find **Current cluster version**\.

**Note**  
Although the console displays this information in one field, it's two parameters in the Amazon Redshift API, `ClusterVersion` and `ClusterRevisionNumber`\. For more information, see [Cluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Cluster.html) in the *Amazon Redshift API Reference*\. 

## Default disk space alarm<a name="rs-clusters-default-disk-usage-alarm"></a>

When you create an Amazon Redshift cluster, you can optionally configure an Amazon CloudWatch alarm to monitor the average percentage of disk space that is used across all of the nodes in your cluster\. We’ll refer to this alarm as the *default disk space alarm*\. 

The purpose of default disk space alarm is to help you monitor the storage capacity of your cluster\. You can configure this alarm based on the needs of your data warehouse\. For example, you can use the warning as an indicator that you might need to resize your cluster\. You might resize either to a different node type or to add nodes, or perhaps to purchase reserved nodes for future expansion\. 

The default disk space alarm triggers when disk usage reaches or exceeds a specified percentage for a certain number of times and at a specified duration\. By default, this alarm triggers when the percentage that you specify is reached, and stays at or above that percentage for five minutes or longer\. You can edit the default values after you launch the cluster\. 

When the CloudWatch alarm triggers, Amazon Simple Notification Service \(Amazon SNS\) sends a notification to specified recipients to warn them that the percentage threshold is reached\. Amazon SNS uses a topic to specify the recipients and message that are sent in a notification\. You can use an existing Amazon SNS topic; otherwise, a topic is created based on the settings that you specify when you launch the cluster\. You can edit the topic for this alarm after you launch the cluster\. For more information about creating Amazon SNS topics, see [Getting Started with Amazon Simple Notification Service](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html)\. 

After you launch the cluster, you can view and edit the alarm from the cluster’s **Status** window under **CloudWatch Alarms**\. The name is **percentage\-disk\-space\-used\-default\-<*string*>**\. You can open the alarm to view the Amazon SNS topic that it is associated with and edit alarm settings\. If you did not select an existing Amazon SNS topic to use, the one created for you is named **<*clustername*>\-default\-alarms \(<*recipient*>\)**; for example, **examplecluster\-default\-alarms \(notify@example\.com\)**\. 

 For more information about configuring and editing the default disk space alarm, see [Creating a cluster](managing-clusters-console.md#create-cluster) and [Creating or editing a disk space alarm](managing-clusters-console.md#rs-mgmt-edit-default-disk-space-alarm)\. 

**Note**  
 If you delete your cluster, the alarm associated with the cluster will not be deleted but it will not trigger\. You can delete the alarm from the CloudWatch console if you no longer need it\. 

## Cluster status<a name="rs-mgmt-cluster-status"></a>

The cluster status displays the current state of the cluster\. The following table provides a description for each cluster status\. 


****  

| Status | Description | 
| --- | --- | 
| available | The cluster is running and available\. | 
| available, prep\-for\-resize | The cluster is being prepared for elastic resize\. The cluster is running and available for read and write queries, but cluster operations, such as creating a snapshot, are not available\. | 
| available, resize\-cleanup  | An elastic resize operation is completing data transfer to the new cluster nodes\. The cluster is running and available for read and write queries, but cluster operations, such as creating a snapshot, are not available\. | 
| cancelling\-resize | The resize operation is being cancelled\. | 
| creating | Amazon Redshift is creating the cluster\. For more information, see [Creating a cluster](managing-clusters-console.md#create-cluster)\. | 
| deleting | Amazon Redshift is deleting the cluster\. For more information, see [Deleting a cluster](managing-clusters-console.md#delete-cluster)\. | 
| final\-snapshot | Amazon Redshift is taking a final snapshot of the cluster before deleting it\. For more information, see [Deleting a cluster](managing-clusters-console.md#delete-cluster)\. | 
| hardware\-failure |  The cluster suffered a hardware failure\. If you have a single\-node cluster, the node cannot be replaced\. To recover your cluster, restore a snapshot\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\.  | 
| incompatible\-hsm |  Amazon Redshift cannot connect to the hardware security module \(HSM\)\. Check the HSM configuration between the cluster and HSM\. For more information, see [Encryption for Amazon Redshift using hardware security modules](working-with-db-encryption.md#working-with-HSM)\.  | 
| incompatible\-network |  There is an issue with the underlying network configuration\. Make sure that the VPC in which you launched the cluster exists and its settings are correct\. For more information, see [Managing clusters in a VPC](managing-clusters-vpc.md)\.  | 
| incompatible\-parameters | There is an issue with one or more parameter values in the associated parameter group, and the parameter value or values cannot be applied\. Modify the parameter group and update any invalid values\. For more information, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\.  | 
| incompatible\-restore |  There was an issue restoring the cluster from the snapshot\. Try restoring the cluster again with a different snapshot\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\.  | 
| modifying |  Amazon Redshift is applying changes to the cluster\. For more information, see [Modifying a cluster](managing-clusters-console.md#modify-cluster)\.  | 
| paused |  The cluster is paused\. For more information, see [Pausing and resuming clusters](managing-cluster-operations.md#rs-mgmt-pause-resume-cluster)\.  | 
| rebooting |  Amazon Redshift is rebooting the cluster\. For more information, see [Rebooting a cluster](managing-clusters-console.md#reboot-cluster)\.  | 
| renaming |  Amazon Redshift is applying a new name to the cluster\. For more information, see [Renaming clusters](managing-cluster-operations.md#rs-mgmt-rename-cluster)\.  | 
| resizing |  Amazon Redshift is resizing the cluster\. For more information, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| rotating\-keys |  Amazon Redshift is rotating encryption keys for the cluster\. For more information, see [Encryption key rotation in Amazon Redshift](working-with-db-encryption.md#working-with-key-rotation)\.  | 
| storage\-full |  The cluster has reached its storage capacity\. Resize the cluster to add nodes or to choose a different node size\. For more information, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\.  | 
| updating\-hsm |  Amazon Redshift is updating the HSM configuration\. \.  | 