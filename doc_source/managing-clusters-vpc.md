# Managing clusters in a VPC<a name="managing-clusters-vpc"></a>

**Topics**
+ [Overview](#managing-clusters-in-vpc-overview)
+ [Creating a cluster in a VPC](getting-started-cluster-in-vpc.md)
+ [Managing VPC security groups for a cluster](managing-vpc-security-groups.md)
+ [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)

## Overview<a name="managing-clusters-in-vpc-overview"></a>

Amazon Redshift supports both the EC2\-VPC and EC2\-Classic platforms to launch a cluster in a virtual private cloud \(VPC\) based on the Amazon VPC service\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\.

**Note**  
Amazon Redshift supports launching clusters into dedicated tenancy VPCs\. For more information, see [Dedicated instances](https://docs.aws.amazon.com/vpc/latest/userguide/dedicated-instance.html) in the *Amazon VPC User Guide*\.

When provisioning a cluster in VPC, you need to do the following:
+ **Provide VPC information\.**

  When you request Amazon Redshift to create a cluster in your VPC, you must provide your VPC information by creating a cluster subnet group\. This information includes the VPC ID and a list of subnets in your VPC\. When you launch a cluster, you provide the cluster subnet group so that Amazon Redshift can provision your cluster in one of the subnets in the VPC\. For more information about creating subnet groups in Amazon Redshift, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\. For more information about setting up VPC, see [Getting started with Amazon VPC](https://docs.aws.amazon.com/AmazonVPC/latest/GettingStartedGuide/GetStarted.html) in the *Amazon VPC Getting Started Guide*\.
+  **Optionally, configure the publicly accessible options\.** 

   If you configure your cluster to be publicly accessible, you can optionally select an *elastic IP address * to use for the external IP address\. An elastic IP address is a static IP address that is associated with your AWS account\. You can use an elastic IP address to connect to your cluster from outside the VPC\. An elastic IP address gives you the ability to change your underlying configuration without affecting the IP address that clients use to connect to your cluster\. This approach can be helpful for situations such as recovery after a failure\. 

   If you want to use an elastic IP address associated with your own AWS account, you must create it in Amazon EC2 prior to launching your Amazon Redshift cluster\. Otherwise, it will not be available during the launch process\. You can also have Amazon Redshift configure an elastic IP address to use for the VPC\. However, the assigned elastic IP address is managed by the Amazon Redshift service and isn't associated with your AWS account\. For more information, see [Elastic IP addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

  In some cases, you might have a publicly accessible cluster in a VPC that you want to connect to it by using the private IP address from within the VPC\. If so, set the following VPC parameters to `true`: 
  +  `DNS resolution` 
  +  `DNS hostnames` 

  Suppose that you have a publicly accessible cluster in a VPC but don't set those parameters to `true` in the VPC\. In these cases, connections made from within the VPC resolve to the elastic IP address of the cluster instead of the private IP address\. We recommend that you set these parameters to `true` and use the private IP address for a publicly accessible cluster when connecting from within the VPC\. For more information, see [Using DNS with your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) in the *Amazon VPC User Guide\.* 
**Note**  
If you have an existing publicly accessible cluster in a VPC, connections from within the VPC continue to use the elastic IP address to connect to the cluster until you resize the cluster\. This occurs even with the preceding parameters set\. Any new clusters follow the new behavior of using the private IP address when connecting to the publicly accessible cluster from within the same VPC\. 

   The elastic IP address is an external IP address for accessing the cluster outside of a VPC\. It's not related to the *cluster node public IP addresses and private IP addresses* that are displayed in the Amazon Redshift console under **Connection details**\. The public and private cluster node IP addresses appear regardless of whether the cluster is publicly accessible or not\. They are used only in certain circumstances to configure ingress rules on the remote host\. These circumstances occur when you load data from an Amazon EC2 instance or other remote host using a Secure Shell \(SSH\) connection\. For more information, see [Step 1: Retrieve the cluster public key and cluster node IP addresses](https://docs.aws.amazon.com/redshift/latest/dg/load-from-host-steps-retrieve-key-and-ips.html) in the *Amazon Redshift Database Developer Guide\.* 

   The option to associate a cluster with an elastic IP address is available when you create the cluster or restore the cluster from a snapshot\. In some cases, you might want to associate the cluster with an elastic IP address or change an elastic IP address that is associated with the cluster\. To attach an elastic IP address after the cluster is created, first update the cluster so that it is not publicly accessible, then make it both publicly accessible and add an Elastic IP address in the same operation\.  
+ **Associate a VPC security group\.**

  You then grant inbound access using a VPC security group\. This VPC security group must allow access over the database port for the cluster so that you can connect by using SQL client tools\. You can configure this in advance, or add rules to it after you launch the cluster\. For more information, see [Security in your VPC](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\. You cannot use the Amazon Redshift cluster security groups to grant inbound access to the cluster\. 

For more information about working with clusters in a VPC, see [Creating a cluster in a VPC](getting-started-cluster-in-vpc.md)\.

**Restoring a snapshot of a cluster in VPC**  
A snapshot of a cluster in VPC can only be restored in a VPC, not outside the VPC\. You can restore it in the same VPC or another VPC in your account\. For more information about snapshots, see [Amazon Redshift snapshots](working-with-snapshots.md)\.