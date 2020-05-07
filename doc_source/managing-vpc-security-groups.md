# Managing VPC security groups for a cluster<a name="managing-vpc-security-groups"></a>

When you provision an Amazon Redshift cluster, it is locked down by default so nobody has access to it\. To grant other users inbound access to an Amazon Redshift cluster, you associate the cluster with a security group\. If you are on the EC2\-VPC platform, you can either use an existing Amazon VPC security group or define a new one\. You then associate it with a cluster as described following\. If you are on the EC2\-Classic platform, you define a cluster security group and associate it with a cluster\. For more information on using cluster security groups on the EC2\-Classic platform, see [Amazon Redshift cluster security groups](working-with-security-groups.md)\.

A VPC security group consists of a set of rules that control access to an instance on the VPC, such as your cluster\. Individual rules set access based either on ranges of IP addresses or on other VPC security groups\. When you associate a VPC security group with a cluster, the rules that are defined in the VPC security group control access to the cluster\. 

Each cluster you provision on the EC2\-VPC platform has one or more Amazon VPC security groups associated with it\. Amazon VPC provides a VPC security group called default, which is created automatically when you create the VPC\. Each cluster that you launch in the VPC is automatically associated with the default VPC security group if you don't specify a different VPC security group when you create the cluster\. You can associate a VPC security group with a cluster when you create the cluster, or you can associate a VPC security group later by modifying the cluster\. For more information on associating a VPC security group with a cluster, see [Creating a cluster by using a launch cluster](managing-clusters-console.md#create-cluster-task) and [To modify a cluster](managing-clusters-console.md#modify-cluster-task)\. 

The following table describes the default rules for the default VPC security group\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security_groups.png)

You can change the rules for the default VPC security group as needed for your Amazon Redshift cluster\.

If the default VPC security group is enough for you, you don't need to create more\. However, you can optionally create additional VPC security groups to better manage inbound access to your cluster\. For example, suppose that you are running a service on an Amazon Redshift cluster, and you have several different service levels you provide to your customers\. If you don't want to provide the same access at all service levels, you might want to create separate VPC security groups, one for each service level\. You can then associate these VPC security groups with your cluster\. 

You can create up to 100 VPC security groups for a VPC and associate a VPC security group with many clusters\. However, you can only associate up to five VPC security groups with a given cluster\. 

Amazon Redshift applies changes to a VPC security group immediately\. So if you have associated the VPC security group with a cluster, inbound cluster access rules in the updated VPC security group apply immediately\.

You can create and modify VPC security groups in the [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\. You can also manage VPC security groups programmatically by using the AWS CLI, the AWS EC2 CLI, and the AWS Tools for Windows PowerShell\. For more information about working with VPC security groups, see [Security groups for your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) in the *Amazon VPC User Guide*\.