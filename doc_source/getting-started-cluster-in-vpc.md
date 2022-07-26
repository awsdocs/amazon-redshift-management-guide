# Creating a cluster in a VPC<a name="getting-started-cluster-in-vpc"></a>

The following are the general steps how you can deploy a cluster in your virtual private cloud \(VPC\)\. 

**To create a cluster in a VPC**

1. Set up a VPC\.

   You can create your cluster either in the default VPC for your account, if your account has one, or a VPC that you have created\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\. To create a VPC, see [Create a VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#Create-VPC) in the *Amazon VPC User Guide\.* Make a note of the VPC identifier, subnet, and subnet's Availability Zone\. You need this information when you launch your cluster\. 
**Note**  
You must have at least one subnet defined in your VPC so you can add it to the cluster subnet group in the next step\. For more information about adding a subnet to your VPC, see [Adding a subnet to your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-subnets.html) in the *Amazon VPC User Guide\.*

1. Create an Amazon Redshift cluster subnet group to specify which subnet your Amazon Redshift cluster can use in the VPC\.

   You can create a cluster subnet group using either the Amazon Redshift console or programmatically\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\.

1. Authorize access for inbound connections in a VPC security group that you associate with the cluster\.

   You can enable a client outside the VPC \(on the public internet\) to connect to the cluster\. To do this, you associate the cluster with a VPC security group that grants inbound access to the port that you used when you launched the cluster\. For examples of security group rules, see [Security group rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules) in the *Amazon VPC User Guide*\. 

1. Follow the steps in [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html) in the *Amazon Redshift Getting Started Guide* to create a cluster\. Make the following modifications when creating your cluster:
   + To display the **Additional configurations** section, switch off **Use defaults**\. 
   + In the **Network and security** section, specify the **Virtual private cloud \(VPC\)**, **Cluster subnet group**, and **VPC security group** that you set up\.

Now you are ready to use the cluster\. You can follow the Getting Started steps to test the cluster by uploading sample data and trying example queries\.