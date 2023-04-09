# Creating a workgroup with a namespace<a name="serverless-console-workgroups-create-workgroup-wizard"></a>

These steps assume you have completed the initial configuration for Amazon Redshift Serverless\. If you haven't created a workgroup and a namespace, and you are looking for instructions that show how to get started with Amazon Redshift Serverless, see [Setting up Amazon Redshift Serverless for the first time](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-console-first-time-setup.html)\.

Complete the following steps to create a workgroup:

1. Choose the **Serverless dashboard**\. Then choose **Create workgroup**\.

1. Enter the workgroup name\.

1. Choose a **Virtual private cloud \(VPC\)** for Amazon Redshift Serverless\. This assigns the workgroup to a specific virtual network in your AWS environment\. For more information about VPCs, see [Overview of VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

1. Choose one or more **VPC security groups**\. For more information, see [Control traffic to resources using security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.

1. Under **Subnet**, specify one or more subnets to associate with your database\. These subnets are contained in the VPC you chose previously and must be in three distinct Availability Zones\. For more information, see [Considerations when using Amazon Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-known-issues.html)\.

1. Select the base RPU capacity that conforms with your requirements\.

## Choose a namespace<a name="serverless-console-workgroups-choose-namespace"></a>

1. Choose either **Create a new namespace**, and enter the namespace name, or **Add to an existing namespace**, and select the namespace from the drop\-down list\.

1. For **Database name and password**, specify the name of the first database\. You can also specify an admin other than your default console admin, by editing the **Admin user credentials**\.

1. For **Permissions**, you choose **Associate IAM role **to associate specific IAM roles with your namespace and workgroup\. For more information about associating IAM roles with Amazon Redshift, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

1. You can customize your encryption settings by creating a new key or choosing a key other than the default\. For **Audit logging**, choose the logs to export\. Each log type specifies different metadata\. Choose **Continue** to review your choices\.

## Review workgroup selections<a name="serverless-console-workgroups-review-workgroup"></a>

1. Review your settings under **Review and create**\. It shows the settings you chose in the previous steps\.

1. Choose **Save**\.

After you create the workgroup, it's added to the **Workgroups** list\.