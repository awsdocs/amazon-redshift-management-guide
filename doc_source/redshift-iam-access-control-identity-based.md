# Using identity\-based policies \(IAM policies\) for Amazon Redshift<a name="redshift-iam-access-control-identity-based"></a>

This topic provides examples of identity\-based policies in which an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available for you to manage access to your Amazon Redshift resources\. For more information, see [Overview of managing access permissions to your Amazon Redshift resources](redshift-iam-access-control-overview.md)\.

The following shows an example of a permissions policy\. The policy allows a user to create, delete, modify, and reboot all clusters, and then denies permission to delete or modify any clusters where the cluster identifier starts with `production`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowClusterManagement",
      "Action": [
        "redshift:CreateCluster",
        "redshift:DeleteCluster",
        "redshift:ModifyCluster",
        "redshift:RebootCluster"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"DenyDeleteModifyProtected",
      "Action": [
        "redshift:DeleteCluster",
        "redshift:ModifyCluster"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:cluster:production*"
      ],
      "Effect": "Deny"
    }
  ]
}
```

The policy has two statements: 
+ The first statement grants permissions for a user to a user to create, delete, modify, and reboot clusters\. The statement specifies a wildcard character \(\*\) as the `Resource` value so that the policy applies to all Amazon Redshift resources owned by the root AWS account\. 
+ The second statement denies permission to delete or modify a cluster\. The statement specifies a cluster Amazon Resource Name \(ARN\) for the `Resource` value that includes a wildcard character \(\*\)\. As a result, this statement applies to all Amazon Redshift clusters owned by the root AWS account where the cluster identifier begins with `production`\.

## AWS\-managed \(predefined\) policies for Amazon Redshift<a name="redshift-policy-resources.managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. Managed policies grant necessary permissions for common use cases so you can avoid having to investigate what permissions are needed\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

You can also create your own custom IAM policies to allow permissions for Amazon Redshift API operations and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 

The following sections describe AWS managed policies, which you can attach to users in your account, and are specific to Amazon Redshift\.

## AmazonRedshiftReadOnlyAccess<a name="redshift-policy-managed-policies-read-only"></a>

Grants read\-only access to all Amazon Redshift resources for an AWS account\.

You can find the [AmazonRedshiftReadOnlyAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftReadOnlyAccess) policy on the IAM console\.

## AmazonRedshiftFullAccess<a name="redshift-policy-managed-policies-full-access"></a>

Grants full access to all Amazon Redshift resources for an AWS account\. Additionally, this policy grants full access to all Amazon Redshift Serverless resources\.

You can find the [AmazonRedshiftFullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftFullAccess) policy on the IAM console\.

## AmazonRedshiftQueryEditor<a name="redshift-policy-managed-policies-query-editor"></a>

Grants full access to the query editor on the Amazon Redshift console\.

You can find the [AmazonRedshiftQueryEditor](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftQueryEditor) policy on the IAM console\.

## AmazonRedshiftDataFullAccess<a name="redshift-policy-managed-policies-data-full-access"></a>

Grants full access to the Amazon Redshift Data API operations and resources for an AWS account\. 

You can find the [AmazonRedshiftDataFullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftDataFullAccess) policy on the IAM console\.

## AmazonRedshiftQueryEditorV2FullAccess<a name="redshift-policy-managed-policies-query-editor-V2"></a>

Grants full access to the Amazon Redshift query editor v2 operations and resources\. This policy also grants access to other required services\.

You can find the [AmazonRedshiftQueryEditorV2FullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftQueryEditorV2FullAccess) policy on the IAM console\.

## AmazonRedshiftQueryEditorV2NoSharing<a name="redshift-policy-managed-policies-query-editor-V2-no-sharing"></a>

Grants the ability to work with Amazon Redshift query editor v2 without sharing resources\. This policy also grants access to other required services\. The principal using this policy can't tag its resources \(such as queries\) to share them with other principals in the same AWS account\. 

You can find the [AmazonRedshiftQueryEditorV2NoSharing](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftQueryEditorV2NoSharing) policy on the IAM console\.

## AmazonRedshiftQueryEditorV2ReadSharing<a name="redshift-policy-managed-policies-query-editor-V2-read-sharing"></a>

Grants the ability to work with Amazon Redshift query editor v2 with limited sharing of resources\. This policy also grants access to other required services\. The principal using this policy can tag its resources \(such as queries\) to share them with other principals in the same AWS account\. The granted principal can read the resources shared with its team but can't update them\. 

You can find the [AmazonRedshiftQueryEditorV2ReadSharing](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftQueryEditorV2ReadSharing) policy on the IAM console\.

## AmazonRedshiftQueryEditorV2ReadWriteSharing<a name="redshift-policy-managed-policies-query-editor-V2-write-sharing"></a>

Grants the ability to work with Amazon Redshift query editor v2 with sharing of resources\. This policy also grants access to other required services\. The principal using this policy can tag its resources \(such as queries\) to share them with other principals in the same AWS account\. The granted principal can read and update the resources shared with its team\. 

You can find the [AmazonRedshiftQueryEditorV2ReadWriteSharing](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftQueryEditorV2ReadWriteSharing) policy on the IAM console\.

## AmazonRedshiftServiceLinkedRolePolicy<a name="redshift-policy-managed-policies-service-linked-role-policy"></a>

You can't attach AmazonRedshiftServiceLinkedRolePolicy to your IAM entities\. This policy is attached to a service\-linked role that allows Amazon Redshift to access account resources\. For more information, see [Using service\-linked roles for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/using-service-linked-roles.html)\. 

## AmazonRedshiftAllCommandsFullAccess<a name="redshift-policy-managed-policies-service-linked-role-commands"></a>

Grants the ability to use the IAM role created from the Amazon Redshift console and set it as default for the cluster to run the COPY from Amazon S3, UNLOAD, CREATE EXTERNAL SCHEMA, CREATE EXTERNAL FUNCTION, and CREATE MODEL commands\. The policy also grants permissions to run SELECT statements for related services, such as Amazon S3, CloudWatch Logs, Amazon SageMaker, or AWS Glue\.

You can find the [AmazonRedshiftAllCommandsFullAccess](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftAllCommandsFullAccess) policy on the IAM console\.

You can also create your own custom IAM policies to allow permissions for Amazon Redshift API operations and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\. 





## Amazon Redshift updates to AWS\-managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Amazon Redshift since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Amazon Redshift Document history page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AmazonRedshiftQueryEditorV2NoSharing](#redshift-policy-managed-policies-query-editor-V2-no-sharing) – Update to an existing policy  |  To grant permission to use notebooks, Amazon Redshift added permission for the following actions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)  | October 17, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – Update to an existing policy  |  To grant permission to use notebooks, Amazon Redshift added permission for the following actions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)  | October 17, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadWriteSharing](#redshift-policy-managed-policies-query-editor-V2-write-sharing) – Update to an existing policy  |  To grant permission to use notebooks, Amazon Redshift added permission for the following actions: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)  | October 17, 2022 | 
|  [AmazonRedshiftServiceLinkedRolePolicy](#redshift-policy-managed-policies-service-linked-role-policy) – Update to an existing policy  |  Amazon Redshift added the namespace `AWS/Redshift` to allow publishing metrics to CloudWatch\.  | September 7, 2022 | 
|  [AmazonRedshiftQueryEditorV2NoSharing](#redshift-policy-managed-policies-query-editor-V2-no-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the actions `sqlworkbench:ListQueryExecutionHistory` and `sqlworkbench:GetQueryExecutionHistory`\. This grants permission to see query history\.  | August 30, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the actions `sqlworkbench:ListQueryExecutionHistory` and `sqlworkbench:GetQueryExecutionHistory`\. This grants permission to see query history\.  | August 30, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadWriteSharing](#redshift-policy-managed-policies-query-editor-V2-write-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the actions `sqlworkbench:ListQueryExecutionHistory` and `sqlworkbench:GetQueryExecutionHistory`\. This grants permission to see query history\.  | August 30, 2022 | 
|  [AmazonRedshiftFullAccess](#redshift-policy-managed-policies-full-access) – Update to an existing policy  |  Permissions for Amazon Redshift Serverless are added to the existing AmazonRedshiftFullAccess managed policy\.  | July 22, 2022 | 
|  [AmazonRedshiftDataFullAccess](#redshift-policy-managed-policies-data-full-access) – Update to an existing policy  |  Amazon Redshift updated redshift\-serverless:GetCredentials default scoping condition of tag aws:ResourceTag/RedshiftDataFullAccess permission from StringEquals to StringLike to grant access to resources tagged with tag key RedshiftDataFullAccess and any tag value\.  | July 11, 2022 | 
|  [AmazonRedshiftDataFullAccess](#redshift-policy-managed-policies-data-full-access) – Update to an existing policy  |  Amazon Redshift added new permissions to allow redshift\-serverless:GetCredentials for temporary credentials to Amazon Redshift Serverless\.  | July 8, 2022 | 
|  [AmazonRedshiftQueryEditorV2NoSharing](#redshift-policy-managed-policies-query-editor-V2-no-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the action `sqlworkbench:GetAccountSettings`\. This grants permission to get account settings\.  | June 15, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the action `sqlworkbench:GetAccountSettings`\. This grants permission to get account settings\.  | June 15, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadWriteSharing](#redshift-policy-managed-policies-query-editor-V2-write-sharing) – Update to an existing policy  |  Amazon Redshift added permission to the action `sqlworkbench:GetAccountSettings`\. This grants permission to get account settings\.  | June 15, 2022 | 
|  [AmazonRedshiftServiceLinkedRolePolicy](#redshift-policy-managed-policies-service-linked-role-policy) – Update to an existing policy  |  To enable public access to new Amazon Redshift Serverless endpoints, Amazon Redshift allocates and associates Elastic IP addresses to the VPC endpoint's Elastic network interface in the customer account\. It does this via permissions provided through the service linked role\. To enable this use case, actions to allocate and release an Elastic IP address are added to the Amazon Redshift Serverless service linked role\.   | May 26, 2022 | 
|  [AmazonRedshiftQueryEditorV2FullAccess](#redshift-policy-managed-policies-query-editor-V2) – Update to an existing policy  |  Permissions to the action `sqlworkbench:ListTaggedResources`\. It is scoped specifically to Amazon Redshift query editor v2 resources\. This policy update gives the right to call `tag:GetResources` only through query editor v2\.   | February 22, 2022 | 
|  [AmazonRedshiftQueryEditorV2NoSharing](#redshift-policy-managed-policies-query-editor-V2-no-sharing) – Update to an existing policy  |  Permissions to the action `sqlworkbench:ListTaggedResources`\. It is scoped specifically to Amazon Redshift query editor v2 resources\. This policy update gives the right to call `tag:GetResources` only through query editor v2\.   | February 22, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – Update to an existing policy  |  Permissions to the action `sqlworkbench:ListTaggedResources`\. It is scoped specifically to Amazon Redshift query editor v2 resources\. This policy update gives the right to call `tag:GetResources` only through query editor v2\.   | February 22, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadWriteSharing](#redshift-policy-managed-policies-query-editor-V2-write-sharing) – Update to an existing policy  |  Permissions to the action `sqlworkbench:ListTaggedResources`\. It is scoped specifically to Amazon Redshift query editor v2 resources\. This policy update gives the right to call `tag:GetResources` only through query editor v2\.   | February 22, 2022 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – Update to an existing policy  |  Permission for the action `sqlworkbench:AssociateQueryWithTab` is added to the managed policy\. Adding it allows customers to create editor tabs linked to a query that is shared with them\.  | February 22, 2022 | 
|  [AmazonRedshiftServiceLinkedRolePolicy](#redshift-policy-managed-policies-service-linked-role-policy) – Update to an existing policy  |  Amazon Redshift added permissions for new actions to allow management of Amazon Redshift network and VPC resources\.  | November 22, 2021 | 
|  [AmazonRedshiftAllCommandsFullAccess](#redshift-policy-managed-policies-service-linked-role-commands) – New policy  |  Amazon Redshift added a new policy to allow using the IAM role created from the Amazon Redshift console and set it as default for the cluster to run the COPY from Amazon S3, UNLOAD, CREATE EXTERNAL SCHEMA, CREATE EXTERNAL FUNCTION, CREATE MODEL, or CREATE LIBRARY commands\.  | November 18, 2021 | 
|  [AmazonRedshiftServiceLinkedRolePolicy](#redshift-policy-managed-policies-service-linked-role-policy) – Update to an existing policy  |  Amazon Redshift added permissions for new actions to allow management of Amazon Redshift CloudWatch log groups and log streams, including audit\-log export\.  | November 15, 2021 | 
|  [AmazonRedshiftFullAccess](#redshift-policy-managed-policies-full-access) – Update to an existing policy  |  Amazon Redshift added new permissions to allow model explainability, DynamoDB, Redshift Spectrum, and Amazon RDS federation\.  | October 07, 2021 | 
|  [AmazonRedshiftQueryEditorV2FullAccess](#redshift-policy-managed-policies-query-editor-V2) – New policy  |  Amazon Redshift added a new policy to allow full access to Amazon Redshift query editor v2\.  | September 24, 2021 | 
|  [AmazonRedshiftQueryEditorV2NoSharing](#redshift-policy-managed-policies-query-editor-V2-no-sharing) – New policy  |  Amazon Redshift added a new policy to allow using Amazon Redshift query editor v2 without sharing resources\.  | September 24, 2021 | 
|  [AmazonRedshiftQueryEditorV2ReadSharing](#redshift-policy-managed-policies-query-editor-V2-read-sharing) – New policy  |  Amazon Redshift added a new policy to allow read sharing within Amazon Redshift query editor v2\.  | September 24, 2021 | 
|  [AmazonRedshiftQueryEditorV2ReadWriteSharing](#redshift-policy-managed-policies-query-editor-V2-write-sharing) – New policy  |  Amazon Redshift added a new policy to allow read and update sharing within Amazon Redshift query editor v2\.  | September 24, 2021 | 
|  [AmazonRedshiftFullAccess](#redshift-policy-managed-policies-full-access) – Update to an existing policy  |  Amazon Redshift added new permissions to allow `sagemaker:*Job*`\.  | August 18, 2021 | 
|  [AmazonRedshiftDataFullAccess](#redshift-policy-managed-policies-data-full-access) – Update to an existing policy  |  Amazon Redshift added new permissions to allow `AuthorizeDataShare`\.  | August 12, 2021 | 
|  [AmazonRedshiftDataFullAccess](#redshift-policy-managed-policies-data-full-access) – Update to an existing policy  |  Amazon Redshift added new permissions to allow `BatchExecuteStatement`\.  | July 27, 2021 | 
|  Amazon Redshift started tracking changes  |  Amazon Redshift started tracking changes for its AWS\-managed policies\.  | July 27, 2021 | 

## Permissions required to use Redshift Spectrum<a name="redshift-spectrum-policy-resources"></a>

Amazon Redshift Spectrum requires permissions to other AWS services to access resources\. For details about permissions in IAM policies for Redshift Spectrum, see [IAM policies for Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-iam-policies.html) in the *Amazon Redshift Database Developer Guide\.*

## Permissions required to use the Amazon Redshift console<a name="redshift-policy-resources.required-permissions.console"></a>

For a user to work with the Amazon Redshift console, that user must have a minimum set of permissions that allows the user to describe the Amazon Redshift resources for their AWS account\. These permissions must also allow the user to describe other related information, including Amazon EC2 security, Amazon CloudWatch, Amazon SNS, and network information\.

If you create an IAM policy that is more restrictive than the minimum required permissions, the console doesn't function as intended for users with that IAM policy\. To ensure that those users can still use the Amazon Redshift console, also attach the `AmazonRedshiftReadOnlyAccess` managed policy to the user\. How to do this is described in [AWS\-managed \(predefined\) policies for Amazon Redshift](#redshift-policy-resources.managed-policies)\.

For information to give a user access to the query editor on the Amazon Redshift console, see [Permissions required to use the Amazon Redshift console query editor](#redshift-policy-resources.required-permissions.query-editor)\. 

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the Amazon Redshift API\. 

## Permissions required to use the Amazon Redshift console query editor<a name="redshift-policy-resources.required-permissions.query-editor"></a>

For a user to work with the Amazon Redshift query editor, that user must have a minimum set of permissions to Amazon Redshift and Amazon Redshift Data API operations\. To connect to a database using a secret, you must also have Secrets Manager permissions\.

To give a user access to the query editor on the Amazon Redshift console, attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` AWS\-managed policies\. The `AmazonRedshiftQueryEditor` policy allows the user permission to retrieve the results of only their own SQL statements\. That is, statements submitted by the same `aws:userid` as shown in this section of the `AmazonRedshiftQueryEditor` AWS\-managed policy\.

```
{
    "Sid": "DataAPIIAMSessionPermissionsRestriction",
    "Action": [
        "redshift-data:GetStatementResult",
        "redshift-data:CancelStatement",
        "redshift-data:DescribeStatement",
        "redshift-data:ListStatements"
    ],
    "Effect": "Allow",
    "Resource": "*",
    "Condition": {
        "StringEquals": {
            "redshift-data:statement-owner-iam-userid": "${aws:userid}"
        }
    }
}
```

To allow a user to retrieve the results of SQL statements of others in the same IAM role, create your own policy without the condition to limit access to the current user\. Also limit access to change a policy to an administrator\.

## Permissions required to use the query editor v2<a name="redshift-policy-resources.required-permissions.query-editor-v2"></a>

For a user to work with the Amazon Redshift query editor v2, that user must have a minimum set of permissions to Amazon Redshift, the query editor v2 operations, and other AWS services such as AWS Key Management Service, AWS Secrets Manager, and tagging service\. 

To give a user full access to the query editor v2, attach the `AmazonRedshiftQueryEditorV2FullAccess` AWS\-managed policy\. The `AmazonRedshiftQueryEditorV2FullAccess` policy allows the user permission to share query editor v2 resources, such as queries, with others in the same team\. For details about how access to query editor v2 resources are controlled, see the definition of the specific managed policy for query editor v2 in the IAM console\. 

Some Amazon Redshift query editor v2 AWS\-managed policies use AWS tags within conditions to scope access to resources\. Within query editor v2, sharing queries is based on the tag key and value `"aws:ResourceTag/sqlworkbench-team": "${aws:PrincipalTag/sqlworkbench-team}"` in the IAM policy attached to principal \(either the IAM user or IAM role\)\. Principals in the same AWS account with the same tag value \(for example, `accounting-team`\), are on the same team in query editor v2\. You can only be associated with one team at a time\. A user with administrative permissions can set up teams in the IAM console by giving all team members the same value for the `sqlworkbench-team` tag\. If the tag value of the `sqlworkbench-team` is changed for an IAM user or an IAM role, there might be a delay until the change is reflected in shared resources\. If the tag value of a resource \(such as a query\) is changed, again there might be a delay until the change is reflected\. Team members must also have the `tag:GetResources` permission to share\.

**Example: To add the `accounting-team` tag for an IAM role**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the console, choose **Roles** and then choose the name of the role that you want to edit\.

1. Choose the **Tags** tab and then choose **Add tags**\.

1. Add the tag key **sqlworkbench\-team** and the value `accounting-team`\.

1. Choose **Save changes**\.

   Now when an IAM principal \(with this IAM role attached\) shares a query with the team, other principals with the same `accounting-team` tag value can view the query\.

For more information on how to attach a tag to a principal, including IAM roles and IAM users, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*\. 

You can also set up teams at the session level using an Identity Provider \(IdP\)\. This allows multiple users using the same IAM role to have different team\. The IAM role trust policy must allow the `sts:TagSession` operation\. For more information, see [Permissions required to add session tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_permissions-required) in the *IAM User Guide*\. Add the principal tag attribute to the SAML assertion provided by your IdP\.

```
<Attribute Name="https://aws.amazon.com/SAML/Attributes/PrincipalTag:sqlworkbench-team">
    <AttributeValue>accounting-team</AttributeValue>
</Attribute>
```

Follow the instructions for your Identity provider \(IdP\) to populate the SAML attribute with the content coming from your directory\. For more information about Identity providers \(IdPs\) and Amazon Redshift, see [Using IAM authentication to generate database user credentials](generating-user-credentials.md) and [Identity providers and federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html) in the *IAM User Guide*\. 

The `sqlworkbench:CreateNotebookVersion` grants permission to get the current content of notebook cells and create a notebook version on your account\. Meaning, at the time of version creation, the current content of the notebook is the same as the version’s content\. Later on, the content of the cells in the version stay the same as the current notebook is updated\. The `sqlworkbench:GetNotebookVersion` grants permission to get a version of the notebook\. A user who doesn’t have `sqlworkbench:BatchGetNotebookCell` permission but has `sqlworkbench:CreateNotebookVersion` and `sqlworkbench:GetNotebookVersion` permissions on a notebook has access to notebook cells in the version\. This user without the `sqlworkbench:BatchGetNotebookCell` permission is still able to retrieve the content of a notebook’s cells by first creating a version and then getting this created version\.

## Permissions required to use the Amazon Redshift scheduler<a name="iam-permission-scheduler"></a>

When you use the Amazon Redshift scheduler, you set up an IAM role with a trust relationship to the Amazon Redshift scheduler \(**scheduler\.redshift\.amazonaws\.com**\) to allow the scheduler to assume permissions on your behalf\. You also attach a policy \(permissions\) to the role for the Amazon Redshift API operations that you want to schedule\.

The following example shows the policy document in JSON format to set up a trust relationship with the Amazon Redshift scheduler and Amazon Redshift\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "scheduler.redshift.amazonaws.com",
                    "redshift.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

For more information about trust entities, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

You also must add permission for the Amazon Redshift operations you want to schedule\. 

For the scheduler to use the `ResizeCluster` operation, add a permission that is similar to the following to your IAM policy\. Depending on your environment, you might want to make the policy more restrictive\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift:ResizeCluster",
            "Resource": "*"
        }
    ]
}
```

For the steps to create a role for the Amazon Redshift scheduler, see [Creating a role for an AWS service \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\. Make these choices when you create a role in the IAM console: 
+ For **Choose the service that will use this role**: Choose **Redshift**\.
+ For **Select your use case**: Choose **Redshift \- Scheduler**\.
+ Create or attach a policy to the role that allows an Amazon Redshift operation to be scheduled\. Choose **Create policy** or modify the role to attach a policy\. Enter the JSON policy for the operation that is to be scheduled\. 
+ After you create the role, edit the **Trust Relationship** of the IAM role to include the service `redshift.amazonaws.com`\.

The IAM role you create has trusted entities of `scheduler.redshift.amazonaws.com` and `redshift.amazonaws.com`\. It also has an attached policy that allows a supported Amazon Redshift API action, such as, `"redshift:ResizeCluster"`\. 

## Permissions required to use the Amazon EventBridge scheduler<a name="iam-permission-eventbridge-scheduler"></a>

When you use the Amazon EventBridge scheduler, you set up an IAM role with a trust relationship to the EventBridge scheduler \(**events\.amazonaws\.com**\) to allow the scheduler to assume permissions on your behalf\. You also attach a policy \(permissions\) to the role for the Amazon Redshift Data API operations that you want to schedule and a policy for Amazon EventBridge operations\.

You use the EventBridge scheduler when you create scheduled queries with the Amazon Redshift query editor on the console\. 

You can create an IAM role to run scheduled queries on the IAM console\. In this IAM role, attach `AmazonEventBridgeFullAccess` and `AmazonRedshiftDataFullAccess`\. 

The following example shows the policy document in JSON format to set up a trust relationship with the EventBridge scheduler\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "events.amazonaws.com",
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

For more information about trust entities, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

For the steps to create a role for the EventBridge scheduler, see [Creating a role for an AWS service \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\. Make these choices when you create a role in the IAM console: 
+ For **Choose the service that will use this role**: Choose **CloudWatch Events**\.
+ For **Select your use case**: Choose **CloudWatch Events**\.
+ Attach the following permission policies: `AmazonEventBridgeFullAccess` and `AmazonRedshiftDataFullAccess`\. 

The IAM role that you create has a trusted entity of `events.amazonaws.com`\. It also has an attached policy that allows supported Amazon Redshift Data API actions, such as, `"redshift-data:*"`\. 

## Permissions required to use Amazon Redshift machine learning \(ML\) with Amazon SageMaker<a name="iam-permission-ml"></a>

Following, you can find a description of the permissions required to use Amazon Redshift machine learning \(ML\) with Amazon SageMaker for different use cases\.

For your users to use Amazon Redshift ML with Amazon SageMaker, create an IAM role with a more restrictive policy than the default\. You can use the policy following\. You can also modify this policy to meet your needs\.

The following policy shows the permissions required to run SageMaker Autopilot with model explainability from Amazon Redshift\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateAutoMLJob",
                "sagemaker:CreateCompilationJob",
                "sagemaker:CreateEndpoint",
                "sagemaker:DescribeAutoMLJob",
                "sagemaker:DescribeTrainingJob",
                "sagemaker:DescribeCompilationJob",
                "sagemaker:DescribeProcessingJob",
                "sagemaker:DescribeTransformJob",
                "sagemaker:ListCandidatesForAutoMLJob",
                "sagemaker:StopAutoMLJob",
                "sagemaker:StopCompilationJob",
                "sagemaker:StopTrainingJob",
                "sagemaker:DescribeEndpoint",
                "sagemaker:InvokeEndpoint",
                "sagemaker:StopProcessingJob",
                "sagemaker:CreateModel",
                "sagemaker:CreateProcessingJob"
            ],
            "Resource": [
                "arn:aws:sagemaker:*:*:model/*redshift*",
                "arn:aws:sagemaker:*:*:training-job/*redshift*",
                "arn:aws:sagemaker:*:*:automl-job/*redshift*",
                "arn:aws:sagemaker:*:*:compilation-job/*redshift*",
                "arn:aws:sagemaker:*:*:processing-job/*redshift*",
                "arn:aws:sagemaker:*:*:transform-job/*redshift*",
                "arn:aws:sagemaker:*:*:endpoint/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/Endpoints/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/ProcessingJobs/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/TrainingJobs/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/TransformJobs/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "cloudwatch:namespace": [
                        "SageMaker",
                        "/aws/sagemaker/Endpoints",
                        "/aws/sagemaker/ProcessingJobs",
                        "/aws/sagemaker/TrainingJobs",
                        "/aws/sagemaker/TransformJobs"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:BatchGetImage",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl",
                "s3:GetBucketCors",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject",
                "s3:PutBucketAcl",
                "s3:PutBucketCors",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket"
            ],
            "Resource": [
                "arn:aws:s3:::redshift-downloads",
                "arn:aws:s3:::redshift-downloads/*",
                "arn:aws:s3:::*redshift*",
                "arn:aws:s3:::*redshift*/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl",
                "s3:GetBucketCors",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject",
                "s3:PutBucketAcl",
                "s3:PutBucketCors",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIgnoreCase": {
                    "s3:ExistingObjectTag/Redshift": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "redshift.amazonaws.com",
                        "sagemaker.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

The following policy shows the full minimal permissions to allow access to Amazon DynamoDB, Redshift Spectrum and Amazon RDS federation\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateAutoMLJob",
                "sagemaker:CreateCompilationJob",
                "sagemaker:CreateEndpoint",
                "sagemaker:DescribeAutoMLJob",
                "sagemaker:DescribeTrainingJob",
                "sagemaker:DescribeCompilationJob",
                "sagemaker:DescribeProcessingJob",
                "sagemaker:DescribeTransformJob",
                "sagemaker:ListCandidatesForAutoMLJob",
                "sagemaker:StopAutoMLJob",
                "sagemaker:StopCompilationJob",
                "sagemaker:StopTrainingJob",
                "sagemaker:DescribeEndpoint",
                "sagemaker:InvokeEndpoint",
                "sagemaker:StopProcessingJob",
                "sagemaker:CreateModel",
                "sagemaker:CreateProcessingJob"
            ],
            "Resource": [
                "arn:aws:sagemaker:*:*:model/*redshift*",
                "arn:aws:sagemaker:*:*:training-job/*redshift*",
                "arn:aws:sagemaker:*:*:automl-job/*redshift*",
                "arn:aws:sagemaker:*:*:compilation-job/*redshift*",
                "arn:aws:sagemaker:*:*:processing-job/*redshift*",
                "arn:aws:sagemaker:*:*:transform-job/*redshift*",
                "arn:aws:sagemaker:*:*:endpoint/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:DescribeLogStreams",
                "logs:PutLogEvents"
            ],
            "Resource": [
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/Endpoints/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/ProcessingJobs/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/TrainingJobs/*redshift*",
                "arn:aws:logs:*:*:log-group:/aws/sagemaker/TransformJobs/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudwatch:PutMetricData"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "cloudwatch:namespace": [
                        "SageMaker",
                        "/aws/sagemaker/Endpoints",
                        "/aws/sagemaker/ProcessingJobs",
                        "/aws/sagemaker/TrainingJobs",
                        "/aws/sagemaker/TransformJobs"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ecr:BatchCheckLayerAvailability",
                "ecr:BatchGetImage",
                "ecr:GetAuthorizationToken",
                "ecr:GetDownloadUrlForLayer"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl",
                "s3:GetBucketCors",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject",
                "s3:PutBucketAcl",
                "s3:PutBucketCors",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket"
            ],
            "Resource": [
                "arn:aws:s3:::redshift-downloads",
                "arn:aws:s3:::redshift-downloads/*",
                "arn:aws:s3:::*redshift*",
                "arn:aws:s3:::*redshift*/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl",
                "s3:GetBucketCors",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject",
                "s3:PutBucketAcl",
                "s3:PutBucketCors",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIgnoreCase": {
                    "s3:ExistingObjectTag/Redshift": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:Scan",
                "dynamodb:DescribeTable",
                "dynamodb:Getitem"
            ],
            "Resource": [
                "arn:aws:dynamodb:*:*:table/*redshift*",
                "arn:aws:dynamodb:*:*:table/*redshift*/index/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ListInstances"
            ],
            "Resource": [
                "arn:aws:elasticmapreduce:*:*:cluster/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "elasticmapreduce:ListInstances"
            ],
            "Resource": "*",
            "Condition": {
                "StringEqualsIgnoreCase": {
                    "elasticmapreduce:ResourceTag/Redshift": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": "arn:aws:lambda:*:*:function:*redshift*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "glue:CreateDatabase",
                "glue:DeleteDatabase",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:UpdateDatabase",
                "glue:CreateTable",
                "glue:DeleteTable",
                "glue:BatchDeleteTable",
                "glue:UpdateTable",
                "glue:GetTable",
                "glue:GetTables",
                "glue:BatchCreatePartition",
                "glue:CreatePartition",
                "glue:DeletePartition",
                "glue:BatchDeletePartition",
                "glue:UpdatePartition",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:BatchGetPartition"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*redshift*/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetResourcePolicy",
                "secretsmanager:GetSecretValue",
                "secretsmanager:DescribeSecret",
                "secretsmanager:ListSecretVersionIds"
            ],
            "Resource": [
                "arn:aws:secretsmanager:*:*:secret:*redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetRandomPassword",
                "secretsmanager:ListSecrets"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "secretsmanager:ResourceTag/Redshift": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "redshift.amazonaws.com",
                        "glue.amazonaws.com",
                        "sagemaker.amazonaws.com",
                        "athena.amazonaws.com"
                    ]
                }
            }
        }
    ]
}
```

Optionally, to use a AWS KMS key for encryption, add the following permissions to the policy\.

```
{
    "Effect": "Allow",
    "Action": [
        "kms:CreateGrant",
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:Encrypt",
        "kms:GenerateDataKey*"
    ],
    "Resource": [
        "arn:aws:kms:<your-region>:<your-account-id>:key/<your-kms-key>"
    ]
}
```

To allow Amazon Redshift and SageMaker to assume the preceding IAM role to interact with other services, add the following trust policy to the role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
          "redshift.amazonaws.com",
          "sagemaker.amazonaws.com"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

In the preceding, the Amazon S3 bucket `redshift-downloads/redshift-ml/` is the location where the sample data used for other steps and examples is stored\. You can remove this bucket if you don't need to load data from Amazon S3\. Or replace it with other Amazon S3 buckets that you use to load data into Amazon Redshift\. 

The **your\-account\-id**, **your\-role**, and **your\-s3\-bucket** values are the account ID, role, and bucket that you specify in your CREATE MODEL command\.

Optionally, you can use the AWS KMS keys section of the sample policy if you specify an AWS KMS key for use with Amazon Redshift ML\. The **your\-kms\-key** value is the key that you use as part of your CREATE MODEL command\.

When you specify a private virtual private cloud \(VPC\) for a hyperparameter tuning job, add the following permissions\.

```
{
            "Effect": "Allow",
            "Action": [
            "ec2:CreateNetworkInterface",
            "ec2:CreateNetworkInterfacePermission",
            "ec2:DeleteNetworkInterface",
            "ec2:DeleteNetworkInterfacePermission",
            "ec2:DescribeNetworkInterfaces",
            "ec2:DescribeVpcs",
            "ec2:DescribeDhcpOptions",
            "ec2:DescribeSubnets",
            "ec2:DescribeSecurityGroups"
            ]
}
```

To work with model explanation, make sure that you have the permissions to call SageMaker API operations\. We recommend that you use the `AmazonSageMakerFullAccess` managed policy\. If you want to create an IAM role with a more restrictive policy, use the policy following\.

```
{
          "Version": "2012-10-17",
          "Statement": [
          {
          "Effect": "Allow",
          "Action": [
          "sagemaker::CreateEndpoint",
          "sagemaker::CreateEndpointConfig",
          "sagemaker::DeleteEndpoint",
          "sagemaker::DeleteEndpointConfig",
          "sagemaker::DescribeEndpoint",
          "sagemaker::DescribeEndpointConfig",
          "sagemaker::DescribeModel",
          "sagemaker::InvokeEndpoint",
          "sagemaker::ListTags"
          ],
          "Resource": "*"
          }
          ]
}
```

For more information about the `AmazonSageMakerFullAccess` managed policy, see [AmazonSageMakerFullAccess](https://docs.aws.amazon.com/sagemaker/latest/dg/security-iam-awsmanpol.html#security-iam-awsmanpol-AmazonSageMakerFullAccess) in the *Amazon SageMaker Developer Guide*\.

For more information about Amazon Redshift ML, see [Using machine learning in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/machine_learning.html) or [CREATE MODEL](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_MODEL.html)\.

## Permissions required to use the data sharing API operations<a name="iam-permission-datasharing"></a>

To control access to the data sharing API operations, use IAM action\-based policies\. For information about how to manage IAM policies, see [Managing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the *IAM User Guide*\.

In particular, suppose that a producer cluster administrator needs to use the `AuthorizeDataShare` call to authorize egress for a datashare outside of an AWS account\. In this case, you set up an IAM action\-based policy to grant this permission\. Use the `DeauthorizeDataShare` call to revoke egress\.

When using IAM action\-based policies, you can also specify an IAM resource in the policy, such as `DataShareARN`\. The following shows the format and an example for `DataShareARN`\.

```
arn:aws:redshift:region:account-id:datashare:namespace-guid/datashare-name
arn:aws:redshift:us-east-1:555555555555:datashare:86b5169f-01dc-4a6f-9fbb-e2e24359e9a8/SalesShare
```

You can restrict `AuthorizeDataShare` access to a specific datashare by specifying the datashare name in the IAM policy\.

```
{
  "Statement": [
    {
      "Action": [
        "redshift:AuthorizeDataShare",
      ],
      "Resource": [
        "arn:aws:redshift:us-east-1:555555555555:datashare:86b5169f-01dc-4a6f-9fbb-e2e24359e9a8/SalesShare"
      ],
      "Effect": "Deny"
    }
  ]
}
```

You can also restrict the IAM policy to all datashares owned by a specific producer cluster\. To do this, replace the **datashare\-name** value in the policy with a wildcard or an asterisk\. Keep the cluster's `namespace-guid` value\.

```
arn:aws:redshift:us-east-1:555555555555:datashare:86b5169f-01dc-4a6f-9fbb-e2e24359e9a8/*
```

Following is an IAM policy that prevents an entity from calling `AuthorizeDataShare` on the datashares owned by a specific producer cluster\. 

```
{
  "Statement": [
    {
      "Action": [
        "redshift:AuthorizeDataShare",
      ],
      "Resource": [
        "arn:aws:redshift:us-east-1:555555555555:datashare:86b5169f-01dc-4a6f-9fbb-e2e24359e9a8/*"
      ],
      "Effect": "Deny"
    }
  ]
}
```

`DataShareARN` restricts the access based on both the datashare name and the globally unique ID \(GUID\) for the owning cluster's namespace\. It does this by specifying the name as an asterisk\.

## Resource policies for GetClusterCredentials<a name="redshift-policy-resources.getclustercredentials-resources"></a>

To connect to a cluster database using a JDBC or ODBC connection with IAM database credentials, or to programmatically call the `GetClusterCredentials` action, you need a minimum set of permissions\. At a minimum, you need permission to call the `redshift:GetClusterCredentials` action with access to a `dbuser` resource\.

If you use a JDBC or ODBC connection, instead of `server` and `port` you can specify `cluster_id` and `region`, but to do so your policy must permit the `redshift:DescribeClusters` action with access to the `cluster` resource\. 

If you call `GetClusterCredentials` with the optional parameters `Autocreate`, `DbGroups`, and `DbName`, make sure to also allow the actions and permit access to the resources listed in the following table\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html)

For more information about resources, see [Amazon Redshift resources and operations](redshift-iam-access-control-overview.md#redshift-iam-accesscontrol.actions-and-resources)\.

You can also include the following conditions in your policy:
+ `redshift:DurationSeconds`
+ `redshift:DbName`
+ `redshift:DbUser`

For more information about conditions, see [Specifying conditions in a policy](redshift-iam-access-control-overview.md#redshift-policy-resources.specifying-conditions)\. 

## Customer managed policy examples<a name="redshift-iam-accesscontrol.examples"></a>

In this section, you can find example user policies that grant permissions for various Amazon Redshift actions\. These policies work when you are using the Amazon Redshift API, AWS SDKs, or the AWS CLI\. 

**Note**  
All examples use the US West \(Oregon\) Region \(`us-west-2`\) and contain fictitious account IDs\.

### Example 1: Allow user full access to all Amazon Redshift actions and resources<a name="redshift-policy-example-allow-full-access"></a>

The following policy allows access to all Amazon Redshift actions on all resources\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowRedshift",
      "Action": [
        "redshift:*"
      ],
      "Effect": "Allow",
      "Resource": "*"
    }
  ]
}
```

The value `redshift:*` in the `Action` element indicates all of the actions in Amazon Redshift\.

### Example 2: Deny a user access to a set of Amazon Redshift actions<a name="redshift-policy-example-deny-specific-actions"></a>

By default, all permissions are denied\. However, sometimes you need to explicitly deny access to a specific action or set of actions\. The following policy allows access to all the Amazon Redshift actions and explicitly denies access to any Amazon Redshift action where the name starts with `Delete`\. This policy applies to all Amazon Redshift resources in `us-west-2`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowUSWest2Region",
      "Action": [
        "redshift:*"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:redshift:us-west-2:*"
    },
   {
     "Sid":"DenyDeleteUSWest2Region",
     "Action": [
        "redshift:Delete*"
      ],
      "Effect": "Deny",
      "Resource": "arn:aws:redshift:us-west-2:*"
   }
  ]
}
```

### Example 3: Allow a user to manage clusters<a name="redshift-policy-example-allow-manage-clusters"></a>

The following policy allows a user to create, delete, modify, and reboot all clusters, and then denies permission to delete any clusters where the cluster name starts with `protected`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowClusterManagement",
      "Action": [
        "redshift:CreateCluster",
        "redshift:DeleteCluster",
        "redshift:ModifyCluster",
        "redshift:RebootCluster"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"DenyDeleteProtected",
      "Action": [
        "redshift:DeleteCluster"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:cluster:protected*"
      ],
      "Effect": "Deny"
    }
  ]
}
```

### Example 4: Allow a user to authorize and revoke snapshot access<a name="redshift-policy-example-allow-authorize-revoke-snapshot"></a>

The following policy allows a user, for example User A, to do the following:
+ Authorize access to any snapshot created from a cluster named `shared`\.
+ Revoke snapshot access for any snapshot created from the `shared` cluster where the snapshot name starts with `revokable`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowSharedSnapshots",
      "Action": [
        "redshift:AuthorizeSnapshotAccess"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:shared/*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowRevokableSnapshot",
      "Action": [
        "redshift:RevokeSnapshotAccess"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/revokable*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

If User A has allowed User B to access a snapshot, User B must have a policy such as the following to allow User B to restore a cluster from the snapshot\. The following policy allows User B to describe and restore from snapshots, and to create clusters\. The name of these clusters must start with `from-other-account`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowDescribeSnapshots",
      "Action": [
        "redshift:DescribeClusterSnapshots"
      ],
      "Resource": [
        "*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowUserRestoreFromSnapshot",
      "Action": [
        "redshift:RestoreFromClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/*",
        "arn:aws:redshift:us-west-2:444455556666:cluster:from-other-account*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### Example 5: Allow a user to copy a cluster snapshot and restore a cluster from a snapshot<a name="redshift-policy-example-allow-copy-restore-snapshot"></a>

The following policy allows a user to copy any snapshot created from the cluster named `big-cluster-1`, and restore any snapshot where the snapshot name starts with `snapshot-for-restore`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowCopyClusterSnapshot",
      "Action": [
        "redshift:CopyClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:big-cluster-1/*"
      ],
      "Effect": "Allow"
    },
    {
      "Sid":"AllowRestoreFromClusterSnapshot",
      "Action": [
        "redshift:RestoreFromClusterSnapshot"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:snapshot:*/snapshot-for-restore*",
        "arn:aws:redshift:us-west-2:123456789012:cluster:*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

### Example 6: Allow a user access to Amazon Redshift, and common actions and resources for related AWS services<a name="redshift-policy-example-allow-related-services"></a>

 The following example policy allows access to all actions and resources for Amazon Redshift, Amazon Simple Notification Service \(Amazon SNS\), and Amazon CloudWatch\. It also allows specified actions on all related Amazon EC2 resources under the account\. 

**Note**  
 Resource\-level permissions are not supported for the Amazon EC2 actions that are specified in this example policy\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowRedshift",
      "Effect": "Allow",
      "Action": [
        "redshift:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Sid":"AllowSNS",
      "Effect": "Allow",
        "Action": [
          "sns:*"
        ],
        "Resource": [
          "*"
        ]
      },
    {
      "Sid":"AllowCloudWatch",
      "Effect": "Allow",
      "Action": [
        "cloudwatch:*"
      ],
      "Resource": [
        "*"
      ]
    },
    {
      "Sid":"AllowEC2Actions",
      "Effect": "Allow",
      "Action": [
        "ec2:AllocateAddress",
        "ec2:AssociateAddress",
        "ec2:AttachNetworkInterface",
        "ec2:DescribeAccountAttributes",
        "ec2:DescribeAddresses",
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeInternetGateways",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeVpcs"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```

### Example 7: Allow a user to tag resources with the Amazon Redshift console<a name="redshift-policy-example-allow-tagging-with-console"></a>

The following example policy allows a user to tag resources with the Amazon Redshift console using the AWS Resource Groups\. This policy can be attached to a user role that invokes the new or original Amazon Redshift console\. For more information about tagging, see [Tagging resources in Amazon Redshift](amazon-redshift-tagging.md)\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Tagging permissions",
            "Effect": "Allow",
            "Action": [
                "redshift:DeleteTags",
                "redshift:CreateTags",
                "redshift:DescribeTags",
                "tag:UntagResources",
                "tag:TagResources"
            ],
            "Resource": "*"
        }
    ]
}
```

## Example policy for using GetClusterCredentials<a name="redshift-policy-examples-getclustercredentials"></a>

The following policy uses these sample parameter values:
+ Region: `us-west-2` 
+ AWS Account: `123456789012` 
+ Cluster name: `examplecluster` 

The following policy enables the `GetCredentials`, `CreateClusterUser`, and `JoinGroup` actions\. The policy uses condition keys to allow the `GetClusterCredentials` and `CreateClusterUser` actions only when the AWS user ID matches `"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"`\. IAM access is requested for the `"testdb"` database only\. The policy also allows users to join a group named `"common_group"`\. 

```
{
"Version": "2012-10-17",
  "Statement": [
    {
     "Sid": "GetClusterCredsStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:GetClusterCredentials"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbuser:examplecluster/${redshift:DbUser}",
        "arn:aws:redshift:us-west-2:123456789012:dbname:examplecluster/testdb",
        "arn:aws:redshift:us-west-2:123456789012:dbgroup:examplecluster/common_group"
      ],
        "Condition": {
           "StringEquals": {
           "aws:userid":"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"
           }
        }
    },
    {
      "Sid": "CreateClusterUserStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:CreateClusterUser"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbuser:examplecluster/${redshift:DbUser}"
      ],
      "Condition": {
        "StringEquals": {
          "aws:userid":"AIDIODR4TAW7CSEXAMPLE:${redshift:DbUser}@yourdomain.com"
        }
      }
    },
    {
      "Sid": "RedshiftJoinGroupStatement",
      "Effect": "Allow",
      "Action": [
        "redshift:JoinGroup"
      ],
      "Resource": [
        "arn:aws:redshift:us-west-2:123456789012:dbgroup:examplecluster/common_group"
      ]
    }
  ]
}
          
 
  }
}
```