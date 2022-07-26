# Get started with Amazon Redshift Serverless using the console<a name="serverless-console-workflows"></a>

## Creating a workgroup with an accompanying namespace<a name="serverless-console-workgroups-create-workgroup-wizard"></a>

### Creating a workgroup<a name="serverless-console-create-workgroup"></a>

1. Choose **Create workgroup**\.

1. Enter the workgroup name\.

1. Choose a **Virtual private cloud \(VPC\)** for Amazon Redshift Serverless\. This assigns the workgroup to a specific virtual network in your AWS environment\. For more information about VPCs, see [Overview of VPCs and subnets](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html)\.

1. Choose one or more **VPC security groups**\. For more information, see [Control traffic to resources using security groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)\.

1. Under **Subnet**, specify one or more subnets to associate with your database\. These subnets are contained in the VPC you chose previously and must be in three distinct Availability Zones\. Choose **Continue**\.

### Choosing a namespace<a name="serverless-console-workgroups-choose-namespace"></a>

1. Choose either **Create a new namespace**, and enter the namespace name, or **Add to an existing namespace**, and select the namespace from the drop\-down list\.

1. For **Database name and password**, specify the name of the first database\. You can also specify an admin other than your default console admin, by editing the **Admin user credentials**\.

1. For **Permissions**, you choose **Associate IAM role **to associate specific IAM roles with your namespace and workgroup\. For more information about associating IAM roles with Amazon Redshift, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

1. You can customize your encryption settings by creating a new key or choosing a key other than the default\. For **Audit logging**, choose the logs to export\. Each log type specifies different metadata\. Choose **Continue** to review your choices\.

### Review workgroup selections<a name="serverless-console-workgroups-review-workgroup"></a>

1. Review your settings under **Review and create**\. It shows the settings you chose in the previous steps\.

1. Choose **Save**\.

After you create the workgroup, it's added to the **Workgroups** list\.

## Managing usage limits<a name="serverless-workgroup-max-rpu"></a>

Under the **Limits** tab for a workgroup, you can add one or more usage limits to control the maximum RPUs you use in a given time period, or to set a data sharing usage limit\.

1. Choose **Manage usage limits**\. Choose **Add limit** on the **Redshift processing units \(RPUs\) maximum** panel\.

1. First, you choose a **Frequency**, which is either **Daily**, **Weekly**, or **Monthly**\. This sets the time period for the limit\. Choosing **Daily** in this instance gives you more detailed control\.

1. Set a usage limit, in number of hours\.

1. Set the action\. These are the following:
   + **Log to system table** \- Adds a record to a system table, which you can query to determine if the limit is exceeded\.
   + **Alert** \- Uses Amazon SNS to set up notification subscriptions and send notifications if a limit is breached\. You can choose an existing Amazon SNS topic or create a new one\.
   + **Turn off user queries** \- Disables queries to stop use of Amazon Redshift Serverless\. It also sends a notification\.

   The first two actions are informational, but the last turns off query processing\.

1. Optionally, you can set a **Cross\-Region data sharing usage limit**, which limits how much data transferred from producer Region to consumer Region consumers can query\. To do this, choose **Add limit** and follow the steps\.

1. Choose **Save changes** to save the limit\.

## Managing query limits<a name="serverless-workgroup-query-limits"></a>

Under the **Limits** tab for a workgroup, you can add a limit to specify the query execution timeout\.

1. Choose **Manage query limits**\. Choose **Add limit** on the **Manage query limits** dialogue\.

1. Enter an integer value for the **Limit**, in seconds\. This specifies the time that elapses before a query times out\.

   If you previously set the timeout limit, you can delete it and specify a new value\.

1. Choose **Save changes** to save the limit\.

When you change your query limit and configuration parameters, your database will restart\.

## Deleting a workgroup<a name="serverless_delete-workgroup"></a>

You can delete a workgroup using the console\. Before you do this, make sure that you have your data backed up and snapshots in place\. Resources deleted as part of the workgroup in many cases can't be retrieved\.

Complete the following steps:

1. Choose **Amazon Redshift Serverless**, choose **Workgroup configuration** and choose **Delete Amazon Redshift Serverless instance**\.

   

1. A dialogue opens\. When you choose to delete the workgroup, all usage limits are removed, all VPC endpoints are removed, and access to VPC endpoints is removed\.

   Type *delete* and select **Delete** to confirm\.

After you complete the steps, the status of the workgroup is *Deleting* and a banner indicates that the workgroup is being deleted\. While the delete process is in progress, some features under the **Serverless dashboard** are disabled\. But you can configure provisioned clusters on the **Provisioned clusters dashboard**\.

After you delete the workgroup, it doesn't appear with the namespace\. You can choose the **Create workgroup** button to create a new one\.

You can delete an existing workgroup and associate a new workgroup with a different configuration to the same namespace\. When creating the new workgroup, choose the base capacity that works with the size of the data associated with the namespace\.

You can associate a workgroup with a namespace that was created with a customer\-managed key \(CMK\)\. For more information about AWS KMS, see [AWS KMS concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)\.

## Edit security and encryption<a name="serverless-console-configuration-edit-network-settings"></a>

1. Choose **Namespace configuration** from the main menu on the console, choose the namespace to edit, and choose **Edit security and encryption**\. A dialog appears\.

1. You can select **Customize encryption settings** and then **Choose an AWS customer managed key** to change the key used to encrypt your resources\.

1. For **Audit logging**, choose the logs to export\. Each log type specifies different metadata\.

1. To complete the configuration update, choose **Save changes**\.

## Deleting a namespace<a name="serverless-console-namespace-delete"></a>

If you want to delete a namespace with an associated workgroup, you have to first delete the workgroup\.

On the Amazon Redshift Serverless console, complete the following steps:

1. Choose **Namespace configuration** from the left menu and then choose the namespace you want to delete from the list\.

1. Choose **Actions** and select **Delete namespace**\.

1. A dialogue box opens\. You can keep your data by creating a manual snapshot prior to completing the delete operation\.

   Type *delete* and select **Delete** to confirm\.