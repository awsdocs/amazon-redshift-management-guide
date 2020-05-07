# Infrastructure security in Amazon Redshift<a name="security-network-isolation"></a>

As a managed service, Amazon Redshift is protected by the AWS global network security procedures described in the [Amazon Web Services: Overview of security processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Amazon Redshift through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed by using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service \(AWS STS\)](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) to generate temporary security credentials to sign requests\.

You can call these API operations from any network location\. In addition, Amazon Redshift supports resource\-based access policies that can include restrictions based on the source IP address\. \.

## Network isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) based on the Amazon VPC service is your private, logically isolated network in the AWS Cloud\. You can deploy an Amazon Redshift cluster within a VPC by taking the following steps:
+ Create a VPC in an AWS Region\. For more information, see [What is Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) in the *Amazon VPC User Guide\.* 
+ Create two or more private VPC subnets\. For more information, see [VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html) in the *Amazon VPC User Guide\.*
+ Deploy an Amazon Redshift cluster\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\.

An Amazon Redshift cluster is locked down by default upon provisioning\. To allow inbound network traffic from Amazon Redshift clients, associate a VPC security group with an Amazon Redshift cluster\. For more information, see [Amazon Redshift cluster subnet groups](working-with-cluster-subnet-groups.md)\. 

To allow traffic only to or from specific IP address ranges, update the security groups with your VPC\. An example is allowing traffic only from or to your corporate network\.

Amazon Redshift supports deploying clusters into dedicated tenancy VPCs\. For more information, see [Dedicated instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/dedicated-instance.html) in the *Amazon EC2 User Guide\.*