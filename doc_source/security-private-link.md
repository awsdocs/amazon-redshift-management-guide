# Connecting to Amazon Redshift using an interface VPC endpoint<a name="security-private-link"></a>

You can connect directly to Amazon Redshift API service using an interface VPC endpoint \(AWS PrivateLink\) in your virtual private cloud \(VPC\) instead of connecting over the internet\. For information about Amazon Redshift API actions, see [Actions](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Operations.html) in the *Amazon Redshift API Reference*\. For more information about AWS PrivateLink, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\. Note that JDBC/ODBC connection to the cluster is not part of Amazon Redshift API service\.

When you use an interface VPC endpoint, communication between your VPC and Amazon Redshift is conducted entirely within the AWS network, which can provide greater security\. Each VPC endpoint is represented by one or more elastic network interfaces with private IP addresses in your VPC subnets\. For more information on elastic network interfaces, see [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the *Amazon EC2 User Guide for Linux Instances\.*  

An interface VPC endpoint connects your VPC directly to Amazon Redshift\. It doesn't use an internet gateway, network address translation \(NAT\) device, virtual private network \(VPN\) connection, or AWS Direct Connect connection\. The instances in your VPC don't need public IP addresses to communicate with the Amazon Redshift API\. 

To use Amazon Redshift through your VPC, you have two options\. One is to connect from an instance that is inside your VPC\. The other is to connect your private network to your VPC by using an AWS VPN option or AWS Direct Connect\. For more information about AWS VPN options, see [VPN connections](https://docs.aws.amazon.com/vpc/latest/userguide/vpn-connections.html) in the *Amazon VPC User Guide*\. For information about AWS Direct Connect, see [Creating a Connection](https://docs.aws.amazon.com/directconnect/latest/UserGuide/create-connection.html) in the *AWS Direct Connect User Guide*\. 

You can create an interface VPC endpoint to connect to Amazon Redshift using the AWS Management Console or AWS Command Line Interface \(AWS CLI\) commands\. For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint)\.

After you create an interface VPC endpoint, you can enable private DNS host names for the endpoint\. When you do, the default Amazon Redshift endpoint \(`https://redshift.Region.amazonaws.com`\) resolves to your VPC endpoint\.

If you don't enable private DNS host names, Amazon VPC provides a DNS endpoint name that you can use in the following format\.

```
VPC_endpoint_ID.redshift.Region.vpce.amazonaws.com
```

For more information, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.

Amazon Redshift supports making calls to all of its [API operations](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Operations.html) inside your VPC\. 

You can attach VPC endpoint policies to a VPC endpoint to control access for AWS Identity and Access Management \(IAM\) principals\. You can also associate security groups with a VPC endpoint to control inbound and outbound access based on the origin and destination of network traffic\. An example is a range of IP addresses\. For more information, see [Controlling Access to Services with VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

## Creating a VPC endpoint policy for Amazon Redshift<a name="security-private-link-vpc_endpoint-policy"></a>

You can create a policy for VPC endpoints for Amazon Redshift to specify the following:
+ The principal that can or can't perform actions
+ The actions that can be performed
+ The resources on which actions can be performed

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

Following, you can find examples of VPC endpoint policies\.

**Topics**
+ [Example: VPC endpoint policy to deny all access from a specified AWS account](#security-private-link-example-1)
+ [Example: VPC endpoint policy to allow VPC access only to a specified IAM role](#security-private-link-example-1.1)
+ [Example: VPC endpoint policy to allow VPC access only to a specified IAM principal \(user\)](#security-private-link-example-2)
+ [Example: VPC endpoint policy to allow read\-only Amazon Redshift operations](#security-private-link-example-3)
+ [Example: VPC endpoint policy denying access to a specified cluster](#security-private-link-example-4)

### Example: VPC endpoint policy to deny all access from a specified AWS account<a name="security-private-link-example-1"></a>

The following VPC endpoint policy denies the AWS account `123456789012` all access to resources using this endpoint\.

```
{
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "*",
            "Effect": "Deny",
            "Resource": "*",
            "Principal": {
                "AWS": [
                    "123456789012"
                ]
            }
        }
    ]
}
```

### Example: VPC endpoint policy to allow VPC access only to a specified IAM role<a name="security-private-link-example-1.1"></a>

The following VPC endpoint policy allows full access only to the IAM role *`redshiftrole`* in AWS account *123456789012*\. All other IAM principals are denied access using the endpoint\.

```
   {
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::123456789012:role/redshiftrole"
                ]
            }
        }]
}
```

This is only a sample\. In most use cases we recommend attaching permissions for specific actions to narrow the scope of permissions\.

### Example: VPC endpoint policy to allow VPC access only to a specified IAM principal \(user\)<a name="security-private-link-example-2"></a>

The following VPC endpoint policy allows full access only to the IAM user *`redshiftadmin`* in AWS account *123456789012*\. All other IAM principals are denied access using the endpoint\.

```
   {
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": {
                "AWS": [
                    "arn:aws:iam::123456789012:user/redshiftadmin"
                ]
            }
        }]
}
```

This is only a sample\. In most use cases we recommend attaching permissions to a role before assigning to a user\. Additionally, we recommend using specific actions to narrow the scope of permissions\.

### Example: VPC endpoint policy to allow read\-only Amazon Redshift operations<a name="security-private-link-example-3"></a>

The following VPC endpoint policy allows only AWS account *`123456789012`* to perform the specified Amazon Redshift actions\. 

The actions specified provide the equivalent of read\-only access for Amazon Redshift\. All other actions on the VPC are denied for the specified account\. Also, all other accounts are denied any access\. For a list of Amazon Redshift actions, see [Actions, Resources, and Condition Keys for Amazon Redshift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonredshift.html) in the *IAM User Guide\.*

```
  {
    "Statement": [
        {
            "Action": [
                "redshift:DescribeAccountAttributes",
                "redshift:DescribeClusterParameterGroups",
                "redshift:DescribeClusterParameters",
                "redshift:DescribeClusterSecurityGroups",
                "redshift:DescribeClusterSnapshots",
                "redshift:DescribeClusterSubnetGroups",
                "redshift:DescribeClusterVersions",
                "redshift:DescribeDefaultClusterParameters",
                "redshift:DescribeEventCategories",
                "redshift:DescribeEventSubscriptions",
                "redshift:DescribeHsmClientCertificates",
                "redshift:DescribeHsmConfigurations",
                "redshift:DescribeLoggingStatus",
                "redshift:DescribeOrderableClusterOptions",
                "redshift:DescribeQuery",
                "redshift:DescribeReservedNodeOfferings",
                "redshift:DescribeReservedNodes",
                "redshift:DescribeResize",
                "redshift:DescribeSavedQueries",
                "redshift:DescribeScheduledActions",
                "redshift:DescribeSnapshotCopyGrants",
                "redshift:DescribeSnapshotSchedules",
                "redshift:DescribeStorage",
                "redshift:DescribeTable",
                "redshift:DescribeTableRestoreStatus",
                "redshift:DescribeTags",
                "redshift:FetchResults",
                "redshift:GetReservedNodeExchangeOfferings"            
            ],
            "Effect": "Allow",
            "Resource": "*",
            "Principal": {
                "AWS": [
                    "123456789012"
                ]
            }
        }
    ]
}
```

### Example: VPC endpoint policy denying access to a specified cluster<a name="security-private-link-example-4"></a>

The following VPC endpoint policy allows full access for all accounts and principals\. At the same time, it denies any access for AWS account *`123456789012`* to actions performed on the Amazon Redshift cluster with cluster ID `my-redshift-cluster`\. Other Amazon Redshift actions that don't support resource\-level permissions for clusters are still allowed\. For a list of Amazon Redshift actions and their corresponding resource type, see [Actions, Resources, and Condition Keys for Amazon Redshift](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonredshift.html) in the *IAM User Guide\.* 

```
 {
    "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "*",
            "Effect": "Deny",
            "Resource": "arn:aws:redshift:us-east-1:123456789012:cluster:my-redshift-cluster",
            "Principal": {
                "AWS": [
                    "123456789012"
                ]
            }
        }
    ]
}
```