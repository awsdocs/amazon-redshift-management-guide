# Amazon Redshift cluster security groups<a name="working-with-security-groups"></a>

When you provision an Amazon Redshift cluster, it is locked down by default so nobody has access to it\. To grant other users inbound access to an Amazon Redshift cluster, you associate the cluster with a security group\. If you are on the EC2\-VPC platform, you can either use an existing Amazon VPC security group or define a new one and then associate it with a cluster\. For more information on managing a cluster on the EC2\-VPC platform, see [Managing clusters in a VPC ](managing-clusters-vpc.md)\.

If you are on the EC2\-Classic platform, you define a cluster security group and associate it with a cluster as described in the following sections\. We recommend that you launch your cluster in a EC2\-VPC platform instead of an EC2\-Classic platform\. However, you can restore an EC2\-Classic snapshot to an EC2\-VPC cluster using the Amazon Redshift console\. For more information, see [Restoring a cluster from a snapshot](managing-snapshots-console.md#snapshot-restore)\.

**Topics**
+ [Overview](#working-with-security-groups-overview)
+ [Managing cluster security groups using the console](managing-security-groups-console.md)
+ [Managing cluster security groups using the AWS SDK for Java](managing-security-groups-java.md)
+ [Manage cluster security groups using the Amazon Redshift CLI and API](manage-security-group-api-cli.md)

## Overview<a name="working-with-security-groups-overview"></a>

A cluster security group consists of a set of rules that control access to your cluster\. Individual rules identify either a range of IP addresses or an Amazon EC2 security group that is allowed access to your cluster\. When you associate a cluster security group with a cluster, the rules that are defined in the cluster security group control access to the cluster\. 

You can create cluster security groups independent of provisioning any cluster\. You can associate a cluster security group with an Amazon Redshift cluster either at the time you provision the cluster or later\. Also, you can associate a cluster security group with multiple clusters\.

Amazon Redshift provides a cluster security group called default, which is created automatically when you launch your first cluster\. Initially, this cluster security group is empty\. You can add inbound access rules to the default cluster security group and then associate it with your Amazon Redshift cluster\. 

If the default cluster security group is enough for you, you don't need to create your own\. However, you can optionally create your own cluster security groups to better manage inbound access to your cluster\. For example, suppose that you are running a service on an Amazon Redshift cluster, and you have a few companies as your customers\. If you don't want to provide the same access to all your customers, you might want to create separate cluster security groups, one for each company\. You can add rules in each cluster security group to identify the Amazon EC2 security groups and the IP address ranges specific to a company\. You can then associate all these cluster security groups with your cluster\.

You can associate a cluster security group with many clusters, and you can associate many cluster security groups with a cluster, subject to AWS service limits\. For more information, see [Amazon Redshift limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_redshift)\.

You can manage cluster security groups using the Amazon Redshift console, and you can manage cluster security groups programmatically by using the Amazon Redshift API or the AWS SDKs\.

Amazon Redshift applies changes to a cluster security group immediately\. So if you have associated the cluster security group with a cluster, inbound cluster access rules in the updated cluster security group apply immediately\.