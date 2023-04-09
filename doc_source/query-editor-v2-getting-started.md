# Configuring your AWS account<a name="query-editor-v2-getting-started"></a>

When you choose the query editor v2 from the Amazon Redshift console, a new tab in your browser opens with the query editor v2 interface\. With the proper permissions, you can access data in an Amazon Redshift cluster or workgroup owned by your AWS account that is in the current AWS Region\.

The first time an administrator configures query editor v2 for your AWS account, they choose the AWS KMS key that is used to encrypt query editor v2 resources\. By default, an AWS owned key is used to encrypt resources\. Or an administrator can use a customer managed key by choosing the Amazon Resource Name \(ARN\) for the key in the configuration page\. After configuring an account, AWS KMS encryption settings can't be changed\. For more information about creating and using a customer managed key with query editor v2, see [Creating an AWS KMS customer managed key to use with query editor v2](#query-editor-v2-kms-key)\. The administrator can also optionally choose an **S3 bucket** and path that is used for some features, such as loading data from a file\. For more information, see [Loading data from a local file](query-editor-v2-loading.md#query-editor-v2-loading-data-local)\. 

Amazon Redshift query editor v2 supports authentication, encryption, isolation, and compliance to keep your data at rest and data in transit secure\. For more information about data security and query editor v2, see the following: 
+ [Encryption at rest](security-server-side-encryption.md)
+ [Encryption in transit](security-encryption-in-transit.md)
+ [Configuration and vulnerability analysis in Amazon Redshift](security-vulnerability-analysis-and-management.md)

AWS CloudTrail captures API calls and related events made by or on behalf of your AWS account and delivers the log files to an Amazon S3 bucket that you specify\. You can identify which users and accounts called AWS, the source IP address from which the calls were made, and when the calls occurred\. To learn more about how query editor v2 runs on AWS CloudTrail, see [Logging with CloudTrail](logging-with-cloudtrail.md)\. For more information about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)\. 

The query editor v2 has adjustable quotas for some of its resources\. For more information, see [Quotas for Amazon Redshift objects](amazon-redshift-limits.md#amazon-redshift-limits-quota)\. 

## Resources created with query editor v2<a name="query-editor-v2-resources"></a>

Within query editor v2, you can create resources such as saved queries and charts\. All resources in query editor v2 are associated with an IAM role or  with a user\. We recommend attaching policies to an IAM role and assigning the role to a user\.

In the query editor v2, you can add and remove tags for saved queries and charts\. You can use these tags when setting up custom IAM policies or to search for resources\. You can also manage tags by using the AWS Resource Groups Tag Editor\.

You can set up IAM roles with IAM policies to share queries with others in your same AWS account in the AWS Region\.

## Creating an AWS KMS customer managed key to use with query editor v2<a name="query-editor-v2-kms-key"></a>

**To create a symmetric encryption customer managed key**:

You can create a symmetric encryption customer managed key to encrypt query editor v2 resources using the AWS KMS console or AWS KMS API operations\. For instructions about creating a key, see [Creating symmetric encryption AWS KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-symmetric-cmk) in the *AWS Key Management Service Developer Guide*\.

**Key policy**

Key policies control access to your customer managed key\. Every customer managed key must have exactly one key policy, which contains statements that determine who can use the key and how they can use it\. When you create your customer managed key, you can specify a key policy\. For more information, see [Managing access to AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html#managing-access) in the *AWS Key Management Service Developer Guide*\. 

To use your customer managed key with Amazon Redshift query editor v2, the following API operations must be allowed in the key policy: 
+ `kms:GenerateDataKey` – Generates a unique symmetric data key to encrypt your data\.
+ `kms:Decrypt` – Decrypts data that was encrypted with the customer managed key\.
+ `kms:DescribeKey` – Provides the customer managed key details to allow the service to validate the key\. 

The following is a sample AWS KMS policy for AWS account `111122223333`\. In the first section, the `kms:ViaService` limits use of the key to the query editor v2 service \(which is named `sqlworkbench.region.amazonaws.com` in the policy\)\. The AWS account using the key must be `111122223333`\. In the second section, the root user and key administrators of AWS account `111122223333` can access to the key\.

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you don't use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/accounts/latest/reference/root-user-tasks.html) in the *AWS Account Management Reference Guide*\. 

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

To access the query editor v2, you need permission\. An administrator can attach one of the following AWS managed policies to the  role to grant permission\. \(We recommend attaching policies to an IAM role and assigning the role to a user\.\) These AWS managed policies are written with different options that control how tagging resources allows sharing of queries\. You can use the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\) to attach IAM policies\. 
+ **AmazonRedshiftQueryEditorV2FullAccess** – Grants full access to the Amazon Redshift query editor v2 operations and resources\. This policy also grants access to other required services\.
+ **AmazonRedshiftQueryEditorV2NoSharing** – Grants the ability to work with Amazon Redshift query editor v2 without sharing resources\. This policy also grants access to other required services\. 
+ **AmazonRedshiftQueryEditorV2ReadSharing** – Grants the ability to work with Amazon Redshift query editor v2 with limited sharing of resources\. The granted principal can read the resources shared with its team but can’t update them\. This policy also grants access to other required services\. 
+ **AmazonRedshiftQueryEditorV2ReadWriteSharing** – Grants the ability to work with Amazon Redshift query editor v2 with sharing of resources\. The granted principal can read and update the resources shared with its team\. This policy also grants access to other required services\. 

You can also create your own policy based on the permissions allowed and denied in the provided managed policies\. If you use the IAM console policy editor to create your own policy, choose **SQL Workbench** as the service for which you create the policy in the visual editor\. The query editor v2 uses the service name AWS SQL Workbench in the visual editor and IAM Policy Simulator\.  

For a principal \(a user with an IAM role assigned\) to connect to an Amazon Redshift cluster, they need the permissions in one of the query editor v2 managed policies\. They also need the `redshift:GetClusterCredentials` permission to the cluster\. To get this permission, someone with administrative permission can attach a policy to the  IAM roles used to connect to the cluster by using temporary credentials\. You can scope the policy to specific clusters or be more general\. For more information about permission to use temporary credentials, see [Create an IAM role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\. 

For a principal \(typically a user with an IAM role assigned\) to turn on the ability in the **Account settings** page for others in the account to **Export result set**, they need the `sqlworkbench:UpdateAcountExportSettings` permission attached the role\. This permission is included in the `AmazonRedshiftQueryEditorV2FullAccess` AWS managed policy\.

As new features are added to query editor v2, the AWS managed policies are updated as needed\. If you create your own policy based on the permissions allowed and denied in the provided managed policies, edit your policies to keep them up to date with changes to the managed policies\. For more information about managed policies in Amazon Redshift, see [AWS\-managed \(predefined\) policies for Amazon Redshift](redshift-iam-access-control-identity-based.md#redshift-policy-resources.managed-policies)\.

To provide access, add permissions to your users, groups, or roles:
+ Users and groups in AWS IAM Identity Center \(successor to AWS Single Sign\-On\):

  Create a permission set\. Follow the instructions in [Create a permission set](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.
+ Users managed in IAM through an identity provider:

  Create a role for identity federation\. Follow the instructions in [Creating a role for a third\-party identity provider \(federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\.
+ IAM users:
  + Create a role that your user can assume\. Follow the instructions in [Creating a role for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in the *IAM User Guide*\.
  + \(Not recommended\) Attach a policy directly to a user or add a user to a user group\. Follow the instructions in [Adding permissions to a user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

## Setting up principal tags to connect to query editor v2 as a federated user<a name="query-editor-v2-principal-tags-iam"></a>

To access the query editor v2 as a federated user, either set up your IAM role or user with principal tags\. Or, set up your identity provider \(IdP\) to pass in `RedshiftDbUser` and \(optionally\) `RedshiftDbGroups`\. For more information about using IAM to manage tags, see [Passing session tags in AWS Security Token Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html) in the *IAM User Guide*\. To set up access using AWS Identity and Access Management, an administrator can add tags using the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\. 

**To add principal tags to an IAM role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Roles** in the navigation pane\.

1. Choose the role that needs access to the query editor v2 using a federated user\.

1. Choose the **Tags** tab\.

1. Choose the **Manage tags**\.

1. Choose **Add tag** and enter the **Key** as `RedshiftDbUser` and enter a **Value** of the federated user name\.

1. Optionally choose **Add tag** and enter the **Key** as `RedshiftDbGroups` and enter a **Value** of the group name to associate to the user\.

1. Choose **Save changes** to view the list of tags associated with your chosen IAM role\. Propagating changes might take several seconds\.

1. To use the federated user, refresh your query editor v2 page after the changes have propagated\.

**Setup your identity provider \(IdP\) to pass principal tags**  
The procedure to set up tags using an identity provider \(IdP\) varies by IdP\. See your IdP documentation for instructions on how to pass user and group information to SAML attributes\. When configured correctly, the following attributes appear in your SAML response that is used by the AWS Security Token Service to populate in the principal tags for `RedshiftDbUser` and `RedshiftDbGroups`\. 

```
<Attribute Name="https://aws.amazon.com/SAML/Attributes/PrincipalTag:RedshiftDbUser">
    <AttributeValue>db-user-name</AttributeValue>
</Attribute>
<Attribute Name="https://aws.amazon.com/SAML/Attributes/PrincipalTag:RedshiftDbGroups">
    <AttributeValue>db-groups</AttributeValue>
</Attribute>
```

 The optional *db\_groups* must be a colon\-separated list such as `group1:group2:group3`\.

Additionally, you can set the `TransitiveTagKeys` attribute to persist the tags during role chaining\.

```
<Attribute Name="https://aws.amazon.com/SAML/Attributes/TransitiveTagKeys">
  <AttributeValue>RedshiftDbUser</AttributeValue>
  <AttributeValue>RedshiftDbGroups</AttributeValue>
</Attribute>
```

For more information about setting up query editor v2, see [Permissions required to use the query editor v2 ](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor-v2)\.

For information about how to set up Active Directory Federation Services \(AD FS\), see the blog post: [ Federate access to Amazon Redshift query editor v2 with Active Directory Federation Services \(AD FS\)](http://aws.amazon.com/blogs/big-data/federate-access-to-amazon-redshift-query-editor-v2-with-active-directory-federation-services-ad-fs-part-3/)\. 

For information about how to set up Okta, see the blog post: [ Federate single sign\-on access to Amazon Redshift query editor v2 with Okta](http://aws.amazon.com/blogs/big-data/federate-single-sign-on-access-to-amazon-redshift-query-editor-v2-with-okta/)\. 