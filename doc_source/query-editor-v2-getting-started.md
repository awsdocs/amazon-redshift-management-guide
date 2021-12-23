# Configuring your AWS account<a name="query-editor-v2-getting-started"></a>

When you choose the query editor v2 from the Amazon Redshift console, a new tab in your browser opens with the query editor v2 interface\. With the proper permissions, you can access data in an Amazon Redshift cluster owned by your AWS account that is in the current AWS Region\.

The first time an administrator configures query editor v2 for your AWS account, they choose the AWS KMS key that is used to encrypt query editor v2 resources\. By default, an AWS owned key is used to encrypt resources\. Or an administrator can use a customer managed key by choosing the Amazon Resource Name \(ARN\) for the key in the configuration page\. After configuring an account, AWS KMS encryption settings can't be changed\. 

For more information about creating and using a customer managed key with query editor v2, see [Creating an AWS KMS customer managed key to use with query editor v2](#query-editor-v2-kms-key)\. 

Amazon Redshift query editor v2 supports authentication, encryption, isolation, and compliance to keep your data at rest and data in transit secure\. For more information about data security and query editor v2, see the following: 
+ [Encryption at rest](security-server-side-encryption.md)
+ [Encryption in transit](security-encryption-in-transit.md)
+ [Configuration and vulnerability analysis in Amazon Redshift](security-vulnerability-analysis-and-management.md)

The query editor v2 has adjustable quotas for some of its resources\. For more information, see [Amazon Redshift quotas](amazon-redshift-limits.md#amazon-redshift-limits-quota)\. 

## Resources created with query editor v2<a name="query-editor-v2-resources"></a>

Within query editor v2, you can create resources such as saved queries and charts\. All resources in query editor v2 are associated with an IAM role or IAM user\.

In the query editor v2, you can add and remove tags for saved queries and charts\. You can use these tags when setting up custom IAM policies or to search for resources\. You can also manage tags by using the AWS Resource Groups Tag Editor\.

You can set up IAM roles and IAM users with IAM policies to share queries with others in your same AWS account in the AWS Region\.

## Creating an AWS KMS customer managed key to use with query editor v2<a name="query-editor-v2-kms-key"></a>

**To create a symmetric customer managed key**:

You can create a symmetric customer managed key to encrypt query editor v2 resources using the AWS KMS console or AWS KMS API operations\. For instructions about creating a key, see [Creating symmetric AWS KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-symmetric-cmk) in the *AWS Key Management Service Developer Guide*\.

**Key policy**

Key policies control access to your customer managed key\. Every customer managed key must have exactly one key policy, which contains statements that determine who can use the key and how they can use it\. When you create your customer managed key, you can specify a key policy\. For more information, see [Managing access to AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the *AWS Key Management Service Developer Guide*\. 

To use your customer managed key with Amazon Redshift query editor v2, the following API operations must be allowed in the key policy: 
+ `kms:GenerateDataKey` – Generates a unique symmetric data key to encrypt your data\.
+ `kms:Decrypt` – Decrypts data that was encrypted with the customer managed key\.
+ `kms:DescribeKey` – Provides the customer managed key details to allow the service to validate the key\. 

The following is a sample AWS KMS policy for AWS account `111122223333`\. In the first section, the `kms:ViaService` limits use of the key to the query editor v2 service \(which is named `sqlworkbench.region.amazonaws.com` in the policy\)\. The AWS account using the key must be `111122223333`\. In the second section, the root user and key administrators of AWS account `111122223333` can access to the key\.

```
{
    "Version": "2012-10-17",
    "Id": "key-consolepolicy",
    "Statement": [
        {
            "Sid": "Allow access to principals authorized to use Amazon Redshift Query Editor V2",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": [
                "kms:GenerateDataKey",
                "kms:Decrypt",
                "kms:DescribeKey"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "kms:ViaService": "sqlworkbench.region.amazonaws.com",
                    "kms:CallerAccount": "111122223333"
                }
            }
        },
        {
            "Sid": "Allow access for key administrators",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::111122223333:root"
            },
            "Action": [
                "kms:*"
            ],
            "Resource": "arn:aws:kms:region:111122223333:key/key_ID"
        }
    ]
}
```

The following resources provide more information about AWS KMS keys:
+ For more information about AWS KMS policies, see [Specifying permissions in a policy](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#overview-policy-elements) in the *AWS Key Management Service Developer Guide*\. 
+ For information about troubleshooting AWS KMS policies, see [Troubleshooting key access](https://docs.aws.amazon.com/kms/latest/developerguide/policy-evaluation.html#example-no-iam) in the *AWS Key Management Service Developer Guide*\. 
+ For more information about keys, see [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#kms_keys) in the *AWS Key Management Service Developer Guide*\.

## Accessing the query editor v2<a name="query-editor-v2-configure"></a>

To access the query editor v2, you need permission\. An administrator can attach one of the following AWS\-managed policies to the IAM user or role to grant permission\. These AWS\-managed policies are written with different options that control how tagging resources allows sharing of queries\. You can use the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\) to attach IAM policies\. 
+ **AmazonRedshiftQueryEditorV2FullAccess** – Grants full access to the Amazon Redshift query editor v2 operations and resources\. This policy also grants access to other required services\.
+ **AmazonRedshiftQueryEditorV2NoSharing** – Grants the ability to work with Amazon Redshift query editor v2 without sharing resources\. This policy also grants access to other required services\. 
+ **AmazonRedshiftQueryEditorV2ReadSharing** – Grants the ability to work with Amazon Redshift query editor v2 with limited sharing of resources\. The granted principal can read the resources shared with its team but can’t update them\. This policy also grants access to other required services\. 
+ **AmazonRedshiftQueryEditorV2ReadWriteSharing** – Grants the ability to work with Amazon Redshift query editor v2 with sharing of resources\. The granted principal can read and update the resources shared with its team\. This policy also grants access to other required services\. 

You can also create your own policy based on the permissions allowed and denied in the provided managed policies\. If you use the IAM console policy editor to create your own policy, choose **SQL Workbench** as the service for which you create the policy in the visual editor\. The query editor v2 uses the service name AWS SQL Workbench in the visual editor and IAM Policy Simulator\.  

For a principal \(an IAM user or IAM role\) to connect to an Amazon Redshift cluster, they need the permissions in one of the query editor v2 managed policies\. They also need the `redshift:GetClusterCredentials` permission to the cluster\. To get this permission, someone with administrative permission can attach a policy to the IAM users or IAM roles that need to connect to the cluster by using temporary credentials\. You can scope the policy to specific clusters or be more general\. For more information about permission to use temporary credentials, see [Create an IAM role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\. 

For a principal \(an IAM user or IAM role\) to view their query editor v2 resources \(such as a query\), in addition to the permissions allowed in one of the query editor v2 managed policies, they also need the `tag:GetResources` permission\. The following snippet of a policy is an example:

```
{
    "Sid": "ResourceGroupPermissions",
    "Effect": "Allow",
    "Action": "tag:GetResources",
    "Resource": "*"
}
```

To get this permission, someone with administrative permission can attach a policy to the IAM users or IAM roles used by query editor v2 users\. You can scope the policy to specific clusters or be more general\. For information about permissions and tagging principals to form teams, see [Permissions required to use the query editor v2 ](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor-v2)\. 

For a principal \(an IAM user or IAM role\) to use the SQL Notebooks \(preview\) feature, you need to add the following policy to one of the principal's existing query editor v2 managed policies\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AmazonRedshiftQueryEditorV2NonResourceLevelPermissions",
            "Effect": "Allow",
            "Action": [
                "sqlworkbench:ListNotebooks"
            ],
            "Resource": "*"
        },
        {
            "Sid": "AmazonRedshiftQueryEditorV2CreateOwnedResourcePermissions",
            "Effect": "Allow",
            "Action": [
                "sqlworkbench:CreateNotebook"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestTag/sqlworkbench-resource-owner": "${aws:userid}"
                }
            }
        },
        {
            "Sid": "AmazonRedshiftQueryEditorV2OwnerSpecificPermissions",
            "Effect": "Allow",
            "Action": [
                "sqlworkbench:GetNotebook",
                "sqlworkbench:UpdateNotebook",
                "sqlworkbench:DeleteNotebook",
                "sqlworkbench:CreateNotebookCell",
                "sqlworkbench:DeleteNotebookCell",
                "sqlworkbench:UpdateNotebookCellContent",
                "sqlworkbench:UpdateNotebookCellLayout",
                "sqlworkbench:BatchGetNotebookCell",
                "sqlworkbench:AssociateNotebookWithTab"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/sqlworkbench-resource-owner": "${aws:userid}"
                }
            }
        },
        {
            "Sid": "AmazonRedshiftQueryEditorV2TeamReadAccessPermissions",
            "Effect": "Allow",
            "Action": [
                "sqlworkbench:GetNotebook",
                "sqlworkbench:BatchGetNotebookCell",
                "sqlworkbench:AssociateNotebookWithTab"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/sqlworkbench-team": "${aws:PrincipalTag/sqlworkbench-team}"
                }
            }
        }
    ]
}
```

For more information about managed policies in Amazon Redshift, see [AWS\-managed \(predefined\) policies for Amazon Redshift](redshift-iam-access-control-identity-based.md#redshift-policy-resources.managed-policies)\.

You can use the IAM console to attach IAM policies to an IAM user or an IAM role\. After you attach a policy to a role, you can attach the role to an IAM user\.

**To attach the IAM policies to an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**\.

1. Choose the user that needs access to the query editor v2\.

1. Choose **Add permissions**\.

1. Choose **Attach existing policies directly**\.

1. For **Policy names**, choose the proper policies as previously described\. 

1. Choose **Next: Review**\.

1. Choose **Add permissions**\.

**To attach the IAM policies to an IAM role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles**\.

1. Choose the role that needs access to the query editor v2\.

1. Choose **Attach policies**\.

1. For **Policy names**, choose the proper policies as previously described\. 

1. Choose **Attach policy**\.

For more information about using IAM to manage users and roles, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\. 