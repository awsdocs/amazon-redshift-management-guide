# Using service\-linked roles for Amazon Redshift<a name="using-service-linked-roles"></a>

Amazon Redshift uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Amazon Redshift\. Service\-linked roles are predefined by Amazon Redshift and include all the permissions that the service requires to call AWS services on behalf of your Amazon Redshift cluster\. 

A service\-linked role makes setting up Amazon Redshift easier because you don't have to manually add the necessary permissions\. The role is linked to Amazon Redshift use cases and has predefined permissions\. Only Amazon Redshift can assume the role, and only the service\-linked role can use the predefined permissions policy\. Amazon Redshift creates a service\-linked role in your account the first time you create a cluster\. You can delete the service\-linked role only after you delete all of the Amazon Redshift clusters in your account\. This protects your Amazon Redshift resources because you can't inadvertently remove permissions needed for access to the resources\.

Amazon Redshift supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html?id=docs_gateway#redshift_region)\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Amazon Redshift<a name="service-linked-role-permissions"></a>

Amazon Redshift uses the service\-linked role named **AWSServiceRoleForRedshift** – Allows Amazon Redshift to call AWS services on your behalf\.

The AWSServiceRoleForRedshift service\-linked role trusts only redshift\.amazonaws\.com to assume the role\.

The AWSServiceRoleForRedshift service\-linked role permissions policy allows Amazon Redshift to complete the following on all related resources:
+ `ec2:DescribeVpcs `
+ `ec2:DescribeSubnets `
+ `ec2:DescribeNetworkInterfaces `
+ `ec2:DescribeAddress `
+ `ec2:AssociateAddress `
+ `ec2:DisassociateAddress `
+ `ec2:CreateNetworkInterface `
+ `ec2:DeleteNetworkInterface `
+ `ec2:ModifyNetworkInterfaceAttribute`

**To allow an IAM entity to create AWSServiceRoleForRedshift service\-linked roles**

Add the following policy statement to the permissions for that IAM entity:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:CreateServiceLinkedRole"      
    ],
    "Resource": "arn:aws:iam::<AWS-account-ID>:role/aws-service-role/redshift.amazonaws.com/AWSServiceRoleForRedshift",
    "Condition": {"StringLike": {"iam:AWSServiceName": "redshift.amazonaws.com"}}
}
```

**To allow an IAM entity to delete AWSServiceRoleForRedshift service\-linked roles**

Add the following policy statement to the permissions for that IAM entity:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:DeleteServiceLinkedRole",
        "iam:GetServiceLinkedRoleDeletionStatus"
    ],
    "Resource": "arn:aws:iam::<AWS-account-ID>:role/aws-service-role/redshift.amazonaws.com/AWSServiceRoleForRedshift",
    "Condition": {"StringLike": {"iam:AWSServiceName": "redshift.amazonaws.com"}}
}
```

Alternatively, you can use an AWS managed policy to [provide full access](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonRedshiftFullAccess) to Amazon Redshift\.

## Creating a service\-linked role for Amazon Redshift<a name="create-service-linked-role"></a>

You don't need to manually create an AWSServiceRoleForRedshift service\-linked role\. Amazon Redshift creates the service\-linked role for you\. If the AWSServiceRoleForRedshift service\-linked role has been deleted from your account, Amazon Redshift creates the role when you launch a new Amazon Redshift cluster\.

**Important**  
If you were using the Amazon Redshift service before September 18, 2017, when it began supporting service\-linked roles, then Amazon Redshift created the AWSServiceRoleForRedshift role in your account\. To learn more, see [A new role appeared in my IAM account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\. 

## Editing a service\-linked role for Amazon Redshift<a name="edit-service-linked-role"></a>

Amazon Redshift does not allow you to edit the AWSServiceRoleForRedshift service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using the IAM console, the AWS Command Line Interface \(AWS CLI\), or IAM API\. For more information, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\.

## Deleting a service\-linked role for Amazon Redshift<a name="delete-service-linked-role"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don't have an unused entity that is not actively monitored or maintained\. 

Before you can delete a service\-linked role for an account, you need to shut down and delete any clusters in the account\. For more information, see [Shutting down and deleting clusters](managing-cluster-operations.md#rs-mgmt-shutdown-delete-cluster)\.

You can use the IAM console, the AWS CLI, or the IAM API to delete a service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.