# Enabling enhanced VPC routing<a name="enhanced-vpc-enabling-cluster"></a>

You can enable enhanced VPC routing when you create a cluster, or you can modify an existing cluster to enable enhanced VPC routing\.

To work with enhanced VPC routing, your cluster must meet the following requirements and constraints:
+ Your cluster must be in a VPC\. 

  If you attach an Amazon S3 VPC endpoint, your cluster uses the VPC endpoint only for access to Amazon S3 buckets in the same AWS Region\. To access buckets in another AWS Region \(not using the VPC endpoint\) or to access other AWS services, make your cluster publicly accessible or use a [network address translation \(NAT\) gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html)\. For more information, see [Creating a cluster in a VPC](getting-started-cluster-in-vpc.md)\.
+ You must enable Domain Name Service \(DNS\) resolution in your VPC\. Alternatively, if you're using your own DNS server, make sure that DNS requests to Amazon S3 are resolved correctly to the IP addresses that are maintained by AWS\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) in the *Amazon VPC User Guide\.*
+ DNS hostnames must be enabled in your VPC\. DNS hostnames are enabled by default\.
+ Your VPC endpoint policies must allow access to any Amazon S3 buckets used with COPY, UNLOAD, or CREATE LIBRARY calls in Amazon Redshift, including access to any manifest files involved\. For COPY from remote hosts, your endpoint policies must allow access to each host machine\. For more information, see [IAM Permissions for COPY, UNLOAD, and CREATE LIBRARY](https://docs.aws.amazon.com/redshift/latest/dg/copy-usage_notes-access-permissions.html#copy-usage_notes-iam-permissions) in the *Amazon Redshift Database Developer Guide\.*

**To create a cluster with enhanced VPC routing**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose **Create cluster** and enter the **Cluster details** properties\. 

1. To display the **Additional configurations** section, choose to switch off **Use defaults**\. 

1. To enable **Enhanced VPC routing** select **Enabled** to force cluster traffic through the VPC\. 

1. Choose **Create cluster** to create the cluster\. The cluster might take several minutes to be ready to use\.

You can use the following AWS Command Line Interface \(AWS CLI\) operations for Amazon Redshift to enable enhanced VPC routing:
+ [create\-cluster ](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster.html)
+ [modify\-cluster](https://docs.aws.amazon.com/cli/latest/reference/redshift/modify-cluster.html)

 You can use the following Amazon Redshift API actions to enable enhanced VPC routing:
+ [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster.html)
+ [ModifyCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyCluster.html)