# Overview of managing access permissions to your Amazon Redshift resources<a name="redshift-iam-access-control-overview"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access the resources are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\), and some services \(such as AWS Lambda\) also support attaching permissions policies to resources\.

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, you decide who is getting the permissions, which resources they get permissions for, and the specific actions that you want to allow on those resources\. 

## Amazon Redshift resources and operations<a name="redshift-iam-accesscontrol.actions-and-resources"></a>

In Amazon Redshift, the primary resource is a *cluster*\. Amazon Redshift supports other resources that can be used with the primary resource such as snapshots, parameter groups, and event subscriptions\. These are referred to as *subresources*\. 

These resources and subresources have unique Amazon Resource Names \(ARNs\) associated with them as shown in the following table\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-overview.html)

Amazon Redshift provides a set of operations to work with the Amazon Redshift resources\. For a list of available operations, see [Amazon Redshift API permissions reference](redshift-policy-resources.resource-permissions.md)\.

## Understanding resource ownership<a name="redshift-iam-access-control-resource-ownership"></a>

A *resource owner* is the AWS account that created a resource\. That is, the resource owner is the AWS account of the *principal entity* \(the root account, an IAM user, or an IAM role\) that authenticates the request that creates the resource\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to create a DB cluster, your AWS account is the owner of the Amazon Redshift resource\.
+ If you create an IAM user in your AWS account and grant permissions to create Amazon Redshift resources to that user, the user can create Amazon Redshift resources\. However, your AWS account, to which the user belongs, owns the Amazon Redshift resources\.
+ If you create an IAM role in your AWS account with permissions to create Amazon Redshift resources, anyone who can assume the role can create Amazon Redshift resources\. Your AWS account, to which the role belongs, owns the Amazon Redshift resources\. 

## Managing access to resources<a name="redshift-iam-accesscontrol-managingaccess"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of Amazon Redshift\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as *identity\-based* policies \(IAM policies\) and policies attached to a resource are referred to as *resource\-based* policies\. Amazon Redshift supports only identity\-based policies \(IAM policies\)\.

### Identity\-based policies \(IAM policies\)<a name="redshift-iam-manage-access-identity-based"></a>

You can attach policies to IAM identities\. For example, you can do the following: 
+ **Attach a permissions policy to a user or a group in your account** – An account administrator can use a permissions policy that is associated with a particular user\. Such a policy grants permissions for that user to create an Amazon Redshift resource, such as a cluster\. 
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – You can attach an identity\-based permissions policy to an IAM role to grant cross\-account permissions\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\. 

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. The principal in the trust policy can also be an AWS service principal if you want to grant an AWS service permissions to assume the role\.

   For more information about using IAM to delegate permissions, see [Access management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. 

The following is an example policy that allows a user to create, delete, modify, and reboot Amazon Redshift clusters for your AWS account\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid":"AllowManageClusters",
      "Effect":"Allow",
      "Action": [
        "redshift:CreateCluster",
        "redshift:DeleteCluster",
        "redshift:ModifyCluster",
        "redshift:RebootCluster"
      ],
      "Resource":"*"
    }
  ]
}
```

For more information about using identity\-based policies with Amazon Redshift, see [Using identity\-based policies \(IAM policies\) for Amazon Redshift](redshift-iam-access-control-identity-based.md)\. For more information about users, groups, roles, and permissions, see [Identities \(users, groups, and roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the *IAM User Guide*\. 

### Resource\-based policies<a name="redshift-iam-access-control-resource-based"></a>

Other services, such as Amazon S3, also support resource\-based permissions policies\. For example, you can attach a policy to an S3 bucket to manage access permissions to that bucket\. Amazon Redshift doesn't support resource\-based policies\. 

## Specifying policy elements: Actions, effects, resources, and principals<a name="redshift-iam-access-control-specify-actions"></a>

For each Amazon Redshift resource \(see [Amazon Redshift resources and operations](#redshift-iam-accesscontrol.actions-and-resources)\), the service defines a set of API operations \(see [Actions](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Operations.html)\)\. To grant permissions for these API operations, Amazon Redshift defines a set of actions that you can specify in a policy\. Performing an API operation can require permissions for more than one action\. 

The following are the basic policy elements:
+ **Resource** – In a policy, you use an Amazon Resource Name \(ARN\) to identify the resource to which the policy applies\. For more information, see [Amazon Redshift resources and operations](#redshift-iam-accesscontrol.actions-and-resources)\. 
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, the `redshift:DescribeClusters` permission allows the user permissions to perform the Amazon Redshift `DescribeClusters` operation\. 
+ **Effect** – You specify the effect when the user requests the specific action—this can be either allow or deny\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions \(applies to resource\-based policies only\)\. Amazon Redshift doesn't support resource\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the Amazon Redshift API actions and the resources that they apply to, see [Amazon Redshift API permissions reference](redshift-policy-resources.resource-permissions.md)\. 

## Specifying conditions in a policy<a name="redshift-policy-resources.specifying-conditions"></a>

When you grant permissions, you can use the access policy language to specify the conditions when a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in an access policy language, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.

To identify conditions where a permissions policy applies, include a `Condition` element in your IAM permissions policy\. For example, you can create a policy that permits a user to create a cluster using the `redshift:CreateCluster` action, and you can add a `Condition` element to restrict that user to only create the cluster in a specific region\. For details, see [Using IAM policy conditions for fine\-grained access control](#redshift-policy-resources.conditions)\. For a list showing all of condition key values and the Amazon Redshift actions and resources that they apply to, see [Amazon Redshift API permissions reference](redshift-policy-resources.resource-permissions.md)\.

### Using IAM policy conditions for fine\-grained access control<a name="redshift-policy-resources.conditions"></a>

In Amazon Redshift, you can use condition keys to restrict access to resources based on the tags for those resources\. The following are common Amazon Redshift condition keys\.


| Condition key | Description | 
| --- | --- | 
| `aws:RequestTag` | Requires users to include a tag key \(name\) and value whenever they create a resource\.  For more information, see [aws:RequestTag](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requesttag) in the *IAM User Guide*\.  | 
| `aws:ResourceTag` | Restricts user access to resources based on specific tag keys and values\. For more information, see [aws:ResourceTag](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-resourcetag) in the *IAM User Guide*\.  | 
| `aws:TagKeys` | Use this key to compare the tag keys in a request with the keys that you specify in the policy\. For more information, see [aws:TagKeys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-tagkeys) in the *IAM User Guide*\.  | 

For information on tags, see [Tagging overview](amazon-redshift-tagging.md#amazon-redshift-tagging-overview)\.

For a list of the API actions that support the `redshift:RequestTag` and `redshift:ResourceTag` condition keys, see [Amazon Redshift API permissions reference](redshift-policy-resources.resource-permissions.md)\.

The following condition keys can be used with the Amazon Redshift GetClusterCredentials action\.


| Condition key | Description | 
| --- | --- | 
| `redshift:DurationSeconds` | Limits the number of seconds that can be specified for duration\.  | 
| `redshift:DbName` | Restricts database names that can be specified\. | 
| `redshift:DbUser` | Restricts database user names that can be specified\. | 

#### Example 1: Restricting access by using the aws:ResourceTag condition key<a name="redshift-policy-resources.resource-permissions-example1"></a>

Use the following IAM policy to let a user modify an Amazon Redshift cluster only for a specific AWS account in the `us-west-2` region with a tag named `environment` with a tag value of `test`\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowModifyTestCluster",
        "Effect": "Allow",
        "Action": "redshift:ModifyCluster",
        "Resource": "arn:aws:redshift:us-west-2:123456789012:cluster:*",
        "Condition": {
            "StringEquals": {
                "aws:ResourceTag/environment": "test"
            }
        }
    }
}
```

#### Example 2: Restricting access by using the aws:RequestTag condition key<a name="redshift-policy-resources.resource-permissions-example2"></a>

Use the following IAM policy to let a user create an Amazon Redshift cluster only if the command to create the cluster includes a tag named `usage` and a tag value of `production`\. The condition with `aws:TagKeys` and the `ForAllValues` modifier specifies that only the keys `costcenter` and `usage` can be specified in the request\.

```
{
    "Version": "2012-10-17",
    "Statement": {
        "Sid": "AllowCreateProductionCluster",
        "Effect": "Allow",
        "Action": [
            "redshift:CreateCluster",
            "redshift:CreateTags"
        ],
        "Resource": "*",
        "Condition": {
            "StringEquals": {
                "aws:RequestTag/usage": "production"
            },
            "ForAllValues:StringEquals": {
                "aws:TagKeys": [
                    "costcenter",
                    "usage"
                ]
            }
        }
    }
}
```