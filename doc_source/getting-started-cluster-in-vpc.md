# Creating a cluster in a VPC<a name="getting-started-cluster-in-vpc"></a>

The following are the general steps how you can deploy a cluster in your virtual private cloud \(VPC\)\. 

## New console<a name="cluster-vpc"></a>

**To create a cluster in a VPC**

1. Set up a VPC\.

   You can create your cluster either in the default VPC for your account, if your account has one, or a VPC that you have created\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\. To create a VPC, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) in the *Amazon VPC User Guide\.* Make a note of the VPC identifier, subnet, and subnet's Availability Zone\. You need this information when you launch your cluster\. 
**Note**  
You must have at least one subnet defined in your VPC so you can add it to the cluster subnet group in the next step\. If you use the VPC wizard, a subnet for your VPC is automatically created for you\. For more information about adding a subnet to your VPC, see [Adding a subnet to your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#AddaSubnet) in the *Amazon VPC User Guide\.*

1. Create an Amazon Redshift cluster subnet group to specify which subnet your Amazon Redshift cluster can use in the VPC\.

   You can create a cluster subnet group using either the Amazon Redshift console or programmatically\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\.

1. Authorize access for inbound connections in a VPC security group that you associate with the cluster\.

   You can enable a client outside the VPC \(on the public internet\) to connect to the cluster\. To do this, you associate the cluster with a VPC security group that grants inbound access to the port that you used when you launched the cluster\. For examples of security group rules, see [Security group rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules) in the *Amazon VPC User Guide*\. 

1. Follow the steps in [Getting started with Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html) in the *Amazon Redshift Getting Started* to create a cluster\. Make the following modifications when creating your cluster:
   + To display the **Additional configurations** section, switch off **Use defaults**\. 
   + In the **Network and security** section, specify the **Virtual private cloud \(VPC\)**, **Cluster subnet group**, and **VPC security group** that you set up\.

## Original console<a name="cluster-vpc-originalconsole"></a>

**To create a cluster in a VPC**

1. Set up a VPC\.

   You can create your cluster either in the default VPC for your account, if your account has one, or a VPC that you have created\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\. To create a VPC, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) in the *Amazon VPC User Guide\.* Make a note of the VPC identifier, subnet, and subnet's Availability Zone\. You need this information when you launch your cluster\. 
**Note**  
You must have at least one subnet defined in your VPC so you can add it to the cluster subnet group in the next step\. If you use the VPC wizard, a subnet for your VPC is automatically created for you\. For more information about adding a subnet to your VPC, For more information about adding a subnet to your VPC, see [Adding a subnet to your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#AddaSubnet) in the *Amazon VPC User Guide\.*

1. Create an Amazon Redshift cluster subnet group that specifies which of the subnets in the VPC can be used by the Amazon Redshift cluster\.

   You can create a cluster subnet group using either the Amazon Redshift console or programmatically\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\.

1. Authorize access for inbound connections in a VPC security group that you associate with the cluster\.

   You can enable a client outside the VPC \(on the public internet\) to connect to the cluster\. To do this, you associate the cluster with a VPC security group that grants inbound access to the port that you used when you launched the cluster\. For examples of security group rules, see [Security group rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules) in the *Amazon VPC User Guide*\. 

1. Launch a cluster in your VPC\.

   You can use the procedure described in the Getting Started to launch the cluster in your VPC\. For more information, see [Step 2: Launch a cluster](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-launch-sample-cluster.html)\. As you follow the wizard, in the **Configure Network Options** of the **ADDITIONAL CONFIGURATION** page, specify the following information:
   + **Choose a VPC** – Select the VPC from the drop\-down list\.
   + **Cluster Subnet Group** – Select the cluster subnet group you created in step 2\.
   + **Publicly Accessible** – Select **Yes** if you want the cluster to have a public IP address that can be accessed from the public internet\. 
**Note**  
When choosing **Yes**, your cluster is publicly accessible outside the VPC and subject to security threats\. 

     Select **No** if you want the cluster to have a private IP addressed that can only be accessed from within the VPC\.  
   +  **Choose a Public IP Address** – Select **Yes** if you want to select an elastic IP address that you already have configured\. Otherwise, select **No** to have Amazon Redshift create an elastic IP address for your instance\. 
   +  **Elastic IP** – Select an elastic IP address to use to connect to the cluster from outside of the VPC\. 
   +  **Availability Zone** – Select **No Preference** to have Amazon Redshift select the Availability Zone that the cluster will be created in\. Otherwise, select a specific Availability Zone\. 
   + Select the VPC security group that grants authorized devices access to the cluster\.

   The following is an example screen shot of the **Configure Networking Options** section of the **ADDITIONAL CONFIGURATION** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-clusters-launchcluster-30-vpc-configurenetworking.png)

Now you are ready to use the cluster\. You can follow the Getting Started steps to test the cluster by uploading sample data and trying example queries\.