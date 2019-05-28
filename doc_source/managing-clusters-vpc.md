# Managing Clusters in an Amazon Virtual Private Cloud \(VPC\)<a name="managing-clusters-vpc"></a>

**Topics**
+ [Overview](#managing-clusters-in-vpc-overview)
+ [Creating a Cluster in a VPC](getting-started-cluster-in-vpc.md)
+ [Managing VPC Security Groups for a Cluster](managing-vpc-security-groups.md)
+ [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)

## Overview<a name="managing-clusters-in-vpc-overview"></a>

Amazon Redshift supports both the EC2\-VPC and EC2\-Classic platforms to launch a cluster\. For more information, see [Supported Platforms to Launch Your Cluster](working-with-clusters.md#cluster-platforms)\.

**Note**  
Amazon Redshift supports launching clusters into dedicated tenancy VPCs\. For more information, see [Dedicated Instances](https://docs.aws.amazon.com/vpc/latest/userguide/dedicated-instance.html) in the *Amazon VPC User Guide*\.

When provisioning a cluster in VPC, you need to do the following:
+ **Provide VPC information\.**

  When you request Amazon Redshift to create a cluster in your VPC, you must provide your VPC information, such as the VPC ID, and a list of subnets in your VPC by first creating a cluster subnet group\. When you launch a cluster you provide the cluster subnet group so that Amazon Redshift can provision your cluster in one of the subnets in the VPC\. For more information about creating subnet groups in Amazon Redshift, see [Amazon Redshift Cluster Subnet Groups](working-with-cluster-subnet-groups.md)\. For more information about setting up VPC, go to [Getting Started with Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/GetStarted.html) in the *Amazon VPC Getting Started Guide*\.
+  **Optionally, configure the publicly accessible options\.** 

   If you configure your cluster to be publicly accessible, you can optionally select an *elastic IP address \(EIP\)* to use for the external IP address\. An EIP is a static IP address that is associated with your AWS account\. You can use an EIP to connect to your cluster from outside the VPC\. An EIP gives you the ability to change your underlying configuration without affecting the IP address that clients use to connect to your cluster\. This approach can be helpful for situations such as recovery after a failure\. 

   If you want to use an EIP associated with your own AWS account, you must create it in Amazon EC2 prior to launching your Amazon Redshift cluster\. Otherwise, it will not be available during the launch process\. You can also have Amazon Redshift configure an EIP to use for the VPC, but the assigned EIP will be managed by the Amazon Redshift service and will not be associated with your AWS account\. For more information, go to [Elastic IP Addresses \(EIP\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

   If you have a publicly accessible cluster in a VPC, and you want to connect to it by using the private IP address from within the VPC, you must set the following VPC parameters to `true`: 
  +  `DNS resolution` 
  +  `DNS hostnames` 

   If you have a publicly accessible cluster in a VPC, but do not set those parameters to `true` in the VPC, connections made from within the VPC will resolve to the EIP of the cluster instead of the private IP address\. We recommend that you set these parameters to `true` and use the private IP address for a publicly accessible cluster when connecting from within the VPC\. For more information, see [Using DNS with Your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) in the Amazon VPC User Guide\. 
**Note**  
 If you have an existing publicly accessible cluster in a VPC, connections from within the VPC will continue to use the EIP to connect to the cluster even with those parameters set until you resize the cluster\. Any new clusters will follow the new behavior of using the private IP address when connecting to the publicly accessible cluster from within the same VPC\. 

   Also, note that the *EIP* is an external IP address for accessing the cluster outside of a VPC, but it is not related to the *cluster node public IP addresses and private IP addresses* that are displayed in the Amazon Redshift console under **SSH Ingestion Settings**\. The public and private cluster node IP addresses appear regardless of whether the cluster is publicly accessible or not\. They are used only in certain circumstances to configure ingress rules on the remote host when you load data from an Amazon EC2 instance or other remote host using a Secure Shell \(SSH\) connection\. For more information, see [Step 1: Retrieve the cluster public key and cluster node IP addresses](https://docs.aws.amazon.com/redshift/latest/dg/load-from-host-steps-retrieve-key-and-ips.html) in the Amazon Redshift Database Developer Guide\. 

   The option to associate a cluster with an EIP is available only when you create the cluster or restore the cluster from a snapshot\. You can't attach an EIP after the cluster is created or restored\. If you want to associate the cluster with an EIP or change an EIP that is associated with the cluster, you need to restore the cluster from a snapshot and specify the EIP at that time\. 
+ **Associate a VPC security group\.**

  You then grant inbound access using a VPC security group\. This VPC security group must allow access over the database port for the cluster so that you can connect by using SQL client tools\. You can configure this in advance, or add rules to it after you launch the cluster\. For more information, go to [Security in Your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. You cannot use the Amazon Redshift cluster security groups to grant inbound access to the cluster\. 

For more information about working with clusters in a VPC, see [Creating a Cluster in a VPC](getting-started-cluster-in-vpc.md)\.

**Restoring a Snapshot of a Cluster in VPC**  
A snapshot of a cluster in VPC can only be restored in a VPC, not outside the VPC\. You can restore it in the same VPC or another VPC in your account\. For more information about snapshots, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.