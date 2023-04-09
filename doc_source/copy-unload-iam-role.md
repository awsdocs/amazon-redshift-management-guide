# Authorizing COPY, UNLOAD, CREATE EXTERNAL FUNCTION, and CREATE EXTERNAL SCHEMA operations using IAM roles<a name="copy-unload-iam-role"></a>

You can use the [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) command to load \(or import\) data into Amazon Redshift and the [UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) command to unload \(or export\) data from Amazon Redshift\. You can use the CREATE EXTERNAL FUNCTION command to create user\-defined functions that invoke functions from AWS Lambda\. 

When you use Amazon Redshift Spectrum, you use the [CREATE EXTERNAL SCHEMA](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_SCHEMA.html) command to specify the location of an Amazon S3 bucket that contains your data\. When you run the COPY, UNLOAD, or CREATE EXTERNAL SCHEMA commands, you provide security credentials\. These credentials authorize your Amazon Redshift cluster to read or write data to and from your target destination, such as an Amazon S3 bucket\. 

When you run the CREATE EXTERNAL FUNCTION, you provide security credentials using the IAM role parameter\. These credentials authorize your Amazon Redshift cluster to invoke Lambda functions from AWS Lambda\. The preferred method to supply security credentials is to specify an AWS Identity and Access Management \(IAM\) role\. For COPY and UNLOAD, you can provide temporary credentials\. For information about creating an IAM role, see [Authorizing Amazon Redshift to access other AWS services on your behalf](authorizing-redshift-service.md)\.

Users need programmatic access if they want to interact with AWS outside of the AWS Management Console\. The way to grant programmatic access depends on the type of user that's accessing AWS\.

To grant users programmatic access, choose one of the following options\.


****  

| Which user needs programmatic access? | To | By | 
| --- | --- | --- | 
|  Workforce identity \(Users managed in IAM Identity Center\)  | Use temporary credentials to sign programmatic requests to the AWS CLI, AWS SDKs, or AWS APIs\. |  Following the instructions for the interface that you want to use\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/copy-unload-iam-role.html)  | 
| IAM | Use temporary credentials to sign programmatic requests to the AWS CLI, AWS SDKs, or AWS APIs\. | Following the instructions in [Using temporary credentials with AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html) in the IAM User Guide\. | 
| IAM | \(Not recommended\)Use long\-term credentials to sign programmatic requests to the AWS CLI, AWS SDKs, or AWS APIs\. |  Following the instructions for the interface that you want to use\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/copy-unload-iam-role.html)  | 

The steps for using an IAM role are as follows:
+ Create an IAM role for use with your Amazon Redshift cluster\.
+ Associate the IAM role with the cluster\.
+ Include the IAM role's ARN when you call the COPY, UNLOAD, CREATE EXTERNAL SCHEMA, or CREATE EXTERNAL FUNCTION command\.

In this topic, you learn how to associate an IAM role with an Amazon Redshift cluster\. 

## Associating IAM roles with clusters<a name="copy-unload-iam-role-associating-with-clusters"></a>

After you have created an IAM role that authorizes Amazon Redshift to access other AWS services for you, you must associate that role with an Amazon Redshift cluster\. You must do this before you can use the role to load or unload data\. 

### Permissions required to associate an IAM role with a cluster<a name="copy-unload-iam-role-associating-with-clusters-perms"></a>

To associate an IAM role with a cluster, a user must have `iam:PassRole` permission for that IAM role\. This permission allows an administrator to restrict which IAM roles a user can associate with Amazon Redshift clusters\. As a best practice, we recommend attaching permissions policies to an IAM role and then assigning it to users and groups as needed\. For more information, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

The following example shows an IAM policy that can be attached to a user that allows the user to take these actions: 
+ Get the details for all Amazon Redshift clusters owned by that user's account\.
+ Associate any of three IAM roles with either of two Amazon Redshift clusters\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:DescribeClusters",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                 "redshift:ModifyClusterIamRoles",
                 "redshift:CreateCluster"
            ],
            "Resource": [
                 "arn:aws:redshift:us-east-1:123456789012:cluster:my-redshift-cluster",
                 "arn:aws:redshift:us-east-1:123456789012:cluster:my-second-redshift-cluster"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": [
                "arn:aws:iam::123456789012:role/MyRedshiftRole",
                "arn:aws:iam::123456789012:role/SecondRedshiftRole",
                "arn:aws:iam::123456789012:role/ThirdRedshiftRole"
             ]
        }
    ]
}
```

After a user has the appropriate permissions, that user can associate an IAM role with an Amazon Redshift cluster\. The IAM role is then ready to use with the COPY or UNLOAD command or other Amazon Redshift commands\.

For more information on IAM policies, see [Overview of IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

### Managing IAM role association with a cluster<a name="managing-iam-role-association-with-cluster"></a>

You can associate an IAM role with an Amazon Redshift cluster when you create the cluster\. Or you can modify an existing cluster and add or remove one or more IAM role associations\. 

Be aware of the following:
+ The maximum number of IAM roles that you can associate is subject to a quota\.
+ An IAM role can be associated with multiple Amazon Redshift clusters\.
+ An IAM role can be associated with an Amazon Redshift cluster only if both the IAM role and the cluster are owned by the same AWS account\. 

#### Using the console to manage IAM role associations<a name="managing-iam-role-association-with-cluster-console"></a>

You can manage IAM role associations for a cluster with the console by using the following procedure\.

**To manage IAM role associations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to update\.

1. For **Actions**, choose **Manage IAM roles** to display the current list IAM roles associated with the cluster\. 

1. On the **Manage IAM roles** page, choose the available IAM roles to add, and then choose **Add IAM role**\. 

1. Choose **Done** to save your changes\. 

#### Using the AWS CLI to manage IAM role associations<a name="w33aac30c30c36c37c33b7c11"></a>

You can manage IAM role associations for a cluster with the AWS CLI by using the following approaches\.

##### Associating an IAM role with a cluster using the AWS CLI<a name="w33aac30c30c36c37c33b7c11b5"></a>

To associate an IAM role with a cluster when the cluster is created, specify the Amazon Resource Name \(ARN\) of the IAM role for the `--iam-role-arns` parameter of the `create-cluster` command\. The maximum number of IAM roles that you can add when calling the `create-cluster` command is subject to a quota\. 

Associating and disassociating IAM roles with Amazon Redshift clusters is an asynchronous process\. You can get the status of all IAM role cluster associations by calling the `describe-clusters` command\.

The following example associates two IAM roles with the newly created cluster named `my-redshift-cluster`\.

```
aws redshift create-cluster \
    --cluster-identifier "my-redshift-cluster" \
    --node-type "dc1.large" \
    --number-of-nodes 16 \
    --iam-role-arns "arn:aws:iam::123456789012:role/RedshiftCopyUnload" \
                    "arn:aws:iam::123456789012:role/SecondRedshiftRole"
```

To associate an IAM role with an existing Amazon Redshift cluster, specify the Amazon Resource Name \(ARN\) of the IAM role for the `--add-iam-roles` parameter of the `modify-cluster-iam-roles` command\. The maximum number of IAM roles that you can add when calling the `modify-cluster-iam-roles` command is subject to a quota\. 

The following example associates an IAM role with an existing cluster named `my-redshift-cluster`\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier "my-redshift-cluster" \
    --add-iam-roles "arn:aws:iam::123456789012:role/RedshiftCopyUnload"
```

##### Disassociating an IAM role from a cluster using the AWS CLI<a name="w33aac30c30c36c37c33b7c11b7"></a>

To disassociate an IAM role from a cluster, specify the ARN of the IAM role for the `--remove-iam-roles` parameter of the `modify-cluster-iam-roles` command\. `modify-cluster-iam-roles` The maximum number of IAM roles that you can remove when calling the `modify-cluster-iam-roles` command is subject to a quota\.

The following example removes the association for an IAM role for the `123456789012` AWS account from a cluster named `my-redshift-cluster`\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier "my-redshift-cluster" \
    --remove-iam-roles "arn:aws:iam::123456789012:role/RedshiftCopyUnload"
```

##### Listing IAM role associations for a cluster using the AWS CLI<a name="w33aac30c30c36c37c33b7c11b9"></a>

To list all of the IAM roles that are associated with an Amazon Redshift cluster, and the status of the IAM role association, call the `describe-clusters` command\. The ARN for each IAM role associated with the cluster is returned in the `IamRoles` list as shown in the following example output\.

Roles that have been associated with the cluster show a status of `in-sync`\. Roles that are in the process of being associated with the cluster show a status of `adding`\. Roles that are being disassociated from the cluster show a status of `removing`\.

```
{
    "Clusters": [
        {
            "ClusterIdentifier": "my-redshift-cluster",
            "NodeType": "dc1.large",
            "NumberOfNodes": 16,
            "IamRoles": [
                {
                    "IamRoleArn": "arn:aws:iam::123456789012:role/MyRedshiftRole",
                    "IamRoleApplyStatus": "in-sync"
                },
                {
                    "IamRoleArn": "arn:aws:iam::123456789012:role/SecondRedshiftRole",
                    "IamRoleApplyStatus": "in-sync"
                }
            ],
            ...
        },
        {
            "ClusterIdentifier": "my-second-redshift-cluster",
            "NodeType": "dc1.large",
            "NumberOfNodes": 10,
            "IamRoles": [
                {
                    "IamRoleArn": "arn:aws:iam::123456789012:role/MyRedshiftRole",
                    "IamRoleApplyStatus": "in-sync"
                },
                {
                    "IamRoleArn": "arn:aws:iam::123456789012:role/SecondRedshiftRole",
                    "IamRoleApplyStatus": "in-sync"
                },
                {
                    "IamRoleArn": "arn:aws:iam::123456789012:role/ThirdRedshiftRole",
                    "IamRoleApplyStatus": "in-sync"
                }
            ],
            ...
        }
    ]
}
```

For more information on using the AWS CLI, see *[AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)*\.