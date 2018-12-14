# Creating a Cluster in a VPC<a name="getting-started-cluster-in-vpc"></a>

The following are the general steps how you can deploy a cluster in your VPC\. 

**To create a cluster in a VPC**

1. Set up a VPC\.

   You can create your cluster either in the default VPC for your account, if your account has one, or a VPC that you have created\. For more information, see [Supported Platforms to Launch Your Cluster](working-with-clusters.md#cluster-platforms)\. To create a VPC, follow steps 2 and 3 in the Amazon Virtual Private Cloud [Getting Started Guide](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/GetStarted.html)\. Make a note of the VPC identifier, subnet, and subnet's availability zone\. You will need this information when you launch your cluster\. 
**Note**  
You must have at least one subnet defined in your VPC so you can add it to the cluster subnet group in the next step\. If you use the VPC Wizard, a subnet for your VPC is automatically created for you\. For more information about adding a subnet to your VPC, go to [Adding a Subnet to Your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Subnets.html#AddaSubnet)\.

1. Create an Amazon Redshift cluster subnet group that specifies which of the subnets in the VPC can be used by the Amazon Redshift cluster\.

   You can create cluster subnet group using either the Amazon Redshift console or programmatically\. For more information, see [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)\.

1. Authorize access for inbound connections in a VPC security group that you will associate with the cluster\.

   To enable a client outside the VPC \(on the public Internet\) to connect to the cluster, you must associate the cluster with a VPC security group that grants inbound access to the port that you used when you launched the cluster\. For examples of security group rules, go to [Security Group Rules](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html#SecurityGroupRules) in the *Amazon VPC User Guide*\. 

1. Launch a cluster in your VPC\.

   You can use the procedure described in the Getting Started to launch the cluster in your VPC\. For more information, see [Step 2: Launch a Cluster](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-launch-sample-cluster.html)\. As you follow the wizard, in the **Configure Network Options** of the **ADDITIONAL CONFIGURATION** page, specify the following information:
   + **Choose a VPC** Select the VPC from the drop\-down list\.
   + **Cluster Subnet Group** Select the cluster subnet group you created in step 2\.
   + **Publicly Accessible** Select **Yes** if you want the cluster to have a public IP address that can be accessed from the public internet, select **No** if you want the cluster to have a private IP addressed that can only be accessed from within the VPC\. If your AWS account allows you to create EC2\-Classic clusters, the default is no, otherwise the default is yes\.
   +  **Choose a Public IP Address** Select **Yes** if you want to select an elastic IP \(EIP\) address that you already have configured\. Otherwise, select **No** to have Amazon Redshift create an EIP for your instance\. 
   +  **Elastic IP** Select an EIP to use to connect to the cluster from outside of the VPC\. 
   +  **Availability Zone** Select **No Preference** to have Amazon Redshift select the availability zone that the cluster will be created in\. Otherwise, select a specific availability zone\. 
   + Select the VPC security group that grants authorized devices access to the cluster\.

   The following is an example screen shot of the **Configure Networking Options** section of the **ADDITIONAL CONFIGURATION** page\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-clusters-launchcluster-30-vpc-configurenetworking.png)

Now you are ready to use the cluster\. You can follow the Getting Started steps to test the cluster by uploading sample data and trying example queries\.