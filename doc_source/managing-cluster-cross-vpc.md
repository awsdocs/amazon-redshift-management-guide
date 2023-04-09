# Working with Redshift\-managed VPC endpoints in Amazon Redshift<a name="managing-cluster-cross-vpc"></a>

By default, an Amazon Redshift cluster is provisioned in a virtual private cloud \(VPC\)\. It can be accessed from another VPC or subnet when you either allow public access or set up an internet gateway, a NAT device, or an AWS Direct Connect connection to route traffic to the cluster\. Or you can access a cluster by setting up a Redshift\-managed VPC endpoint \(powered by AWS PrivateLink\)\. 

You set up a Redshift\-managed VPC endpoint as a private connection between a VPC that contains a cluster and a VPC that is running a client tool\. If the cluster is in another account, then the account owner \(grantor\) needs to grant access to the account \(grantee\) that wants to establish a connection\. With this approach, you can access the data warehouse without using public IP addresses or routing traffic across the internet\. 

The following scenarios describe common reasons to allow access to a cluster using a Redshift\-managed VPC endpoint: 
+ AWS account A wants to allow a VPC in AWS account B to have access to a cluster\. 
+ AWS account A wants to allow a VPC that is also in AWS account A to have access to a cluster\.
+ AWS account A wants to allow a different subnet in the cluster's VPC within AWS account A to have access to a cluster\.

The general workflow to set up a Redshift\-managed VPC endpoint to access a cluster in another account is as follows: 

1. The owner account of the cluster grants access authorization to another account and specifies the AWS account ID and VPC identifier \(or all VPCs\) of the grantee\. 

1. The grantee account is notified that they have permission to create a Redshift\-managed VPC endpoint\.

1. The grantee account creates a Redshift\-managed VPC endpoint\.

1. The grantee account can now access the cluster of the owner account using the Redshift\-managed VPC endpoint\.

You can manage this process with the Amazon Redshift console, the AWS CLI, or the Amazon Redshift API\. 

## Considerations when using Redshift\-managed VPC endpoints<a name="managing-cluster-cross-vpc-considerations"></a>

When using Redshift\-managed VPC endpoints, keep the following in mind: 
+ Make sure that the cluster to access is an RA3 node type\. 
+ Make sure that the cluster to access has cluster relocation turned on\. For information about requirements to turn on cluster relocation, see [Managing cluster relocation in Amazon Redshift](managing-cluster-recovery.md)\. 
+ Make sure that the cluster to access is available within the valid port ranges 5431\-5455 and 8191\-8215\. The default is 5439\.
+ You can modify the VPC security groups associated with an existing Redshift\-managed VPC endpoint\. To modify other settings, delete the current Redshift\-managed VPC endpoint and create a new one\.
+ The number of Redshift\-managed VPC endpoints that you can create is limited to your VPC endpoint quota\. 
+ The Redshift\-managed VPC endpoints aren't accessible from the internet\. A Redshift\-managed VPC endpoint is accessible only within the VPC where the endpoint is provisioned or any VPCs peered with the VPC where the endpoint is provisioned as permitted by the route tables and security groups\.
+ You can't use the Amazon VPC console to manage Redshift\-managed VPC endpoints\.
+ When you create a Redshift\-managed VPC endpoint, the VPC you choose must have a cluster subnet group\. To create a cluster subnet group, see [Managing cluster subnet groups using the console ](managing-cluster-subnet-group-console.md)\.

For information about quotas and naming constraints, see [Quotas and limits in Amazon Redshift](amazon-redshift-limits.md)\. 

For information about pricing, see [AWS PrivateLink pricing](https://aws.amazon.com/privatelink/pricing/)\.

## Managing Redshift\-managed VPC endpoints using the Amazon Redshift console<a name="managing-cluster-cross-vpc-console"></a>

You can configure the use of Redshift\-managed VPC endpoints by using the Amazon Redshift console\.

### Granting access to a cluster<a name="managing-cluster-cross-vpc-console-grantor"></a>

If the VPC that you want to access your cluster is in another AWS account, make sure to authorize it from the owner's \(grantor's\) account\.

**To allow a VPC in another AWS account to have access to your cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. For the cluster that you want to allow access, view the cluster details by choosing the cluster name\. Choose the **Properties** tab of the cluster\. 

   The **Granted accounts** section displays the accounts and corresponding VPCs that have access to your cluster\. 

1. Choose **Grant access** to display a form to enter **Grantee information** to add an account\. 

1. For **AWS account ID**, enter the ID of the account you are granting access\. You can grant access to specific VPCs or all VPCs in the specified account\. 

1. Choose **Grant access** to grant access\. 

### Creating a Redshift\-managed VPC endpoint<a name="managing-cluster-cross-vpc-console-grantee"></a>

If you own a cluster or you have been granted access to it, you can create a Redshift\-managed VPC endpoint for the cluster\. 

**To create a Redshift\-managed VPC endpoint**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Configurations**\. 

   The **Configurations** page displays the Redshift\-managed VPC endpoints that have been created\. To view details for an endpoint, choose its name\.

1. Choose **Create endpoint** to display a form to enter information about the endpoint to add\.

1. Enter values for **Endpoint name**, **AWS account ID**, **Cluster identifier**, **Virtual private cloud \(VPC\)**, **Subnet group**, and other properties of the endpoint\. 

   The subnet group in **Subnet group** defines the subnets and IP addresses where Amazon Redshift deploys the endpoint\. Amazon Redshift chooses a subnet that has IP addresses available for the network interface associated with the endpoint\. 

   The security group rules in Security group define the ports, protocols, and sources for inbound traffic that you are authorizing for your endpoint\. Depending on the port you selected when creating, modifying, or migrating the cluster, you allow access to the selected port via the security group or the CIDR range where your workloads run\.  

1. Choose **Create endpoint** to create the endpoint\. 

After your endpoint is created, you can access the cluster through the URL shown in **Endpoint URL** in the configuration settings for your Redshift\-managed VPC endpoint\. 

## Managing Redshift\-managed VPC endpoints using the AWS CLI<a name="managing-cluster-cross-vpc-cli"></a>

You can use the following Amazon Redshift CLI operations to work with Redshift\-managed VPC endpoints\. For more information, see the *AWS CLI Command Reference*\.
+ [authorize\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/authorize-endpoint-access.html)
+ [revoke\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/revoke-endpoint-access.html)
+ [create\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-endpoint-access.html)
+ [modify\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/modify-endpoint-access.html)
+ [delete\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-endpoint-access.html)
+ [describe\-endpoint\-access](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-endpoint-access.html)
+ [describe\-endpoint\-authorization](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-endpoint-authorization.html)

## Managing Redshift\-managed VPC endpoints using Amazon Redshift API operations<a name="managing-cluster-cross-vpc-api"></a>

You can use the following Amazon Redshift API operations to work with Redshift\-managed VPC endpoints\. For more information, see the *Amazon Redshift API Reference*\.
+ [AuthorizeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_AuthorizeEndpointAccess.html)
+ [RevokeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_RevokeEndpointAccess.html)
+ [CreateEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateEndpointAccess.html)
+ [ModifyEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyEndpointAccess.html)
+ [DeleteEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteEndpointAccess.html)
+ [DescribeEndpointAccess](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEndpointAccess.html)
+ [DescribeEndpointAuthorization](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeEndpointAuthorization.html)