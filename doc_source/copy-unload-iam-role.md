# Authorizing COPY, UNLOAD, and CREATE EXTERNAL SCHEMA operations using IAM roles<a name="copy-unload-iam-role"></a>

You can use the [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) command to load \(or import\) data into Amazon Redshift and the [UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) command to unload \(or export\) data from Amazon Redshift\. When you use Amazon Redshift Spectrum, you use the [CREATE EXTERNAL SCHEMA](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_SCHEMA.html) command to specify the location of an Amazon S3 bucket that contains your data\. When you run the COPY, UNLOAD, or CREATE EXTERNAL SCHEMA commands, you must provide security credentials\. These credentials authorize your Amazon Redshift cluster to read or write data to and from your target destination, such as an Amazon S3 bucket\. The preferred method to supply security credentials is to specify an AWS Identity and Access Management \(IAM\) role\. For COPY and UNLOAD, you can provide AWS access keys\. For information about creating an IAM role, see [Authorizing Amazon Redshift to access other AWS services on your behalf](authorizing-redshift-service.md)\.

The steps for using an IAM role are as follows:
+ Create an IAM role for use with your Amazon Redshift cluster\.
+ Associate the IAM role with the cluster\.
+ Include the IAM role's ARN when you call the COPY, UNLOAD, or CREATE EXTERNAL SCHEMA command\.

In this topic, you learn how to associate an IAM role with an Amazon Redshift cluster\. 

## Associating IAM roles with clusters<a name="copy-unload-iam-role-associating-with-clusters"></a>

After you have created an IAM role that authorizes Amazon Redshift to access other AWS services for you, you must associate that role with an Amazon Redshift cluster\. You must do this before you can use the role to load or unload data\. 

### Permissions required to associate an IAM role with a cluster<a name="w14aac25c24c28c17c11b5"></a>

To associate an IAM role with a cluster, an IAM user must have `iam:PassRole` permission for that IAM role\. This permission allows an administrator to restrict which IAM roles a user can associate with Amazon Redshift clusters\. 

The following example shows an IAM policy that can be attached to an IAM user that allows the user to take these actions: 
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

After an IAM user has the appropriate permissions, that user can associate an IAM role with an Amazon Redshift cluster\. The IAM role is then ready to use with the COPY or UNLOAD command or other Amazon Redshift commands\.

For more information on IAM policies, see [Overview of IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.

### Managing IAM role association with a cluster<a name="w14aac25c24c28c17c11b7"></a>

You can associate an IAM role with an Amazon Redshift cluster when you create the cluster\. Or you can modify an existing cluster and add or remove one or more IAM role associations\. 

Be aware of the following:
+ You can associate a maximum of 10 IAM roles with an Amazon Redshift cluster\.
+ An IAM role can be associated with multiple Amazon Redshift clusters\.
+ An IAM role can be associated with an Amazon Redshift cluster only if both the IAM role and the cluster are owned by the same AWS account\. 

#### Using the console to manage IAM role associations<a name="w14aac25c24c28c17c11b7b9"></a>

You can manage IAM role associations for a cluster with the console by using the following procedure\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

##### New console<a name="cluster-iam-role-manage"></a>

**To manage IAM role associations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to update\.

1. For **Actions**, choose **Manage IAM roles** to display the current list IAM roles associated with the cluster\. 

1. On the **Manage IAM roles** page, choose the available IAM roles to add, and then choose **Add IAM role**\. 

1. Choose **Done** to save your changes\. 

##### Original console<a name="cluster-iam-role-manage-originalconsole"></a>

**To manage IAM role associations**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the list, choose the cluster that you want to manage IAM role associations for\.

1. Choose **See IAM Roles**\.

1. To associate an IAM role with the cluster, choose your IAM role for **Available roles**\. You can also manually enter an IAM role if you don't see it included the list \(for example, if the IAM role hasn't been created yet\)\.

1. To disassociate an IAM role from the cluster, choose **X** for the IAM role that you want to disassociate\.

1. After you have finished modifying the IAM role associations for the cluster, choose **Apply Changes** to update the IAM roles that are associated with the cluster\.

The **Manage IAM Roles** panel shows you the status of your cluster IAM role associations\. Roles that have been associated with the cluster show a status of `in-sync`\. Roles that are in the process of being associated with the cluster show a status of `adding`\. Roles that are being disassociated from the cluster show a status of `removing`\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-iam-roles.png)

#### Using the AWS CLI to manage IAM role associations<a name="w14aac25c24c28c17c11b7c11"></a>

You can manage IAM role associations for a cluster with the AWS CLI by using the following approaches\.

##### Associating an IAM role with a cluster using the AWS CLI<a name="w14aac25c24c28c17c11b7c11b5"></a>

To associate an IAM role with a cluster when the cluster is created, specify the Amazon Resource Name \(ARN\) of the IAM role for the `--iam-role-arns` parameter of the `create-cluster` command\. You can specify up to 10 IAM roles to add when calling the `create-cluster` command\.

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

To associate an IAM role with an existing Amazon Redshift cluster, specify the Amazon Resource Name \(ARN\) of the IAM role for the `--add-iam-roles` parameter of the `modify-cluster-iam-roles` command\. You can specify up to 10 IAM roles to add when calling the `modify-cluster-iam-roles` command\.

The following example associates an IAM role with an existing cluster named `my-redshift-cluster`\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier "my-redshift-cluster" \
    --add-iam-roles "arn:aws:iam::123456789012:role/RedshiftCopyUnload"
```

##### Disassociating an IAM role from a cluster using the AWS CLI<a name="w14aac25c24c28c17c11b7c11b7"></a>

To disassociate an IAM role from a cluster, specify the ARN of the IAM role for the `--remove-iam-roles` parameter of the `modify-cluster-iam-roles` command\. You can specify up to 10 IAM roles to remove when calling the `modify-cluster-iam-roles` command\.

The following example removes the association for an IAM role for the `123456789012` AWS account from a cluster named `my-redshift-cluster`\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier "my-redshift-cluster" \
    --remove-iam-roles "arn:aws:iam::123456789012:role/RedshiftCopyUnload"
```

##### Listing IAM role associations for a cluster using the AWS CLI<a name="w14aac25c24c28c17c11b7c11b9"></a>

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

For more information on using the AWS CLI, see *[AWS command line interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)*\.