# Working with AQUA \(Advanced Query Accelerator\)<a name="managing-cluster-aqua"></a>

AQUA \(Advanced Query Accelerator\) is an analytics query accelerator for Amazon Redshift that uses custom\-designed hardware to speed up queries that scan large datasets\. 

AQUA is a cost\-effective addition to Amazon Redshift–managed storage that is optimized for secure, transactional, multitenant access and high\-throughput analytic queries\. It uses high speed Non\-Volatile Memory Express \(NVMe\) solid\-state storage elements and Nitro\-based acceleration for compression and encryption\. It uses acceleration based on a field\-programmable gate array \(FPGA\) to push as much computation as possible into the storage layer\. These components are connected together in a unique way, allowing data scanning without using a traditional CPU\. At the same time, these components allow intermediate results to be aggregated in high\-speed memory\. AQUA is a data cache and maintains high\-speed connections to Redshift managed storage\.

You don't need to change your databases or applications to use AQUA\. Amazon Redshift identifies the scan portions of queries that can benefit from acceleration and push them to AQUA for processing\. AQUA automatically optimizes query performance on subsets of the data that require extensive scans, filters, and aggregation\. With this approach, you can use AQUA to run queries that scan, filter, and aggregate large datasets\. AQUA excels at queries that require processing\-intensive scans, filters, and aggregation such as those that contain LIKE and SIMILAR TO predicates\. 

AQUA supports authentication, encryption, isolation, and compliance to keep your data at rest and data in transit secure\. For more information about data security and AQUA, see the following: 
+ [Data protection in Amazon Redshift](security-data-protection.md)
+ [Encryption at rest](security-server-side-encryption.md)
+ [Encryption in transit](security-encryption-in-transit.md)

For more information about AQUA, see [When does Amazon Redshift use AQUA to run queries?](managing-cluster-aqua-understanding.md)

AQUA is available on clusters with ra3\.16xlarge and ra3\.4xlarge node types\. 

AQUA is available with release version 1\.0\.24421 or later in the following AWS Regions: 
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ US East \(Ohio\) Region \(us\-east\-2\)
+ US West \(Oregon\) Region \(us\-west\-2\) 
+ Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)
+ Europe \(Ireland\) Region \(eu\-west\-1\)

You can activate and manage AQUA for Amazon Redshift clusters on the Amazon Redshift console, with the AWS CLI, or with Amazon Redshift API operations\. You can do so when you create a cluster,  restore a cluster from a snapshot, or modify an existing cluster\. When you activate an existing cluster, make sure to reboot your cluster for the change to take effect\. 

To activate AQUA on the Amazon Redshift console, navigate to your cluster and choose **Configure AQUA** for **Actions**\. To view a previously defined AQUA configuration for your cluster, navigate to your cluster, and view the **General information** section, **AQUA** information\. 

You have the following choices when you configure AQUA:
+ **Turn on** – You choose to activate AQUA\. AQUA can only be activated in certain AWS Regions and for ra3\.4xlarge and ra3\.16xlarge node types\. 
+ **Turn off** – You choose not to activate AQUA\. 
+ **Automatic** – Amazon Redshift determines whether to use AQUA\. This is the default\. Currently, AQUA isn't activated with this option, but this behavior is subject to change\. 

## Managing AQUA using the AWS CLI<a name="managing-cluster-aqua-cli"></a>

You can use the following AWS CLI commands to activate and manage AQUA\. For more information, see the *AWS CLI Command Reference*\.
+ [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster.html)
+ [restore\-from\-cluster\-snapshot](https://docs.aws.amazon.com/cli/latest/reference/redshift/restore-from-cluster-snapshot.html)
+ [modify\-aqua\-configuration](https://docs.aws.amazon.com/cli/latest/reference/redshift/modify-aqua-configuration.html)

## Managing AQUA using Amazon Redshift API operations<a name="managing-cluster-aqua-api"></a>

You can use the following Amazon Redshift API operations to activate and manage AQUA\. For more information, see the *Amazon Redshift API Reference*\.
+ [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster.html)
+ [RestoreFromClusterSnapshot](https://docs.aws.amazon.com/redshift/latest/APIReference/API_RestoreFromClusterSnapshot.html)
+ [ModifyAquaConfiguration](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyAquaConfiguration.html)