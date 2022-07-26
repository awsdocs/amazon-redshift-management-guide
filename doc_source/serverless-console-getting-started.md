# Setting up Amazon Redshift Serverless for the first time<a name="serverless-console-getting-started"></a>

The first time you select the **Serverless dashboard**, you walk through the steps to set up Amazon Redshift Serverless\. Under **Get started with the serverless experience**, complete the following steps\.

1. Under **Configuration**, choose **Use default settings** for a quick set\-up experience, or **Customize settings** for granular control\. If you choose **Use default settings**, the settings under **Namespace** are set to default values\. These include:
   + **Database name and password**
   + **Admin user credentials**
   + **Default IAM role**
   + **IAM roles** \- If you haven't created any IAM roles, choose **Associate IAM role** to create and associate an IAM role or roles with the namespace\. The IAM role you associate with your namespace must include a trust relationship with `redshift-serverless.amazonaws.com` and `redshift.amazonaws.com`\. For more information, see [Permissions required to use Amazon Redshift Serverless](serverless-get-started.md#serverless-endpoint-iam-role)\.

   The **Workgroup** is set to **default**\. Additionally, network settings are set to the **default VPC**, the **default** subnet, and the **default** cluster security group\.

   If you choose the default settings, skip to the last step in this procedure to save\. If you choose **Customize settings**, complete the remaining steps prior to saving\.

1. Specify the **Namespace** name\. 

1. Specify the database name\.

1. You can optionally select **Customize admin user credentials** if you don't want to use the default IAM admin credentials\. For more information, see [Identity and access management in Amazon Redshift Serverless](serverless-security.md#serverless-iam)\.

1. For **Permissions**, create an IAM role to associate with the namespace, or associate an existing IAM role\.

1. The AWS\-owned KMS key is used by default to encrypt your data\. Instead of using the AWS\-owned KMS key, you can **Customize encryption settings** to choose a **KMS key** you manage\. The AWS KMS key is used to encrypt resources for the namespace\. 

1. Choose logs to export for audit logging\. Audit logging helps with monitoring and troubleshooting\.

1. For **Workgroup**, enter the name, or leave the default name\.

1. Specify **Network and security** settings\. These include the following:
   + **Virtual private cloud \(VPC\)** \- The VPC to define the virtual network for the database\.
   + **VPC security groups** \- These security groups define which subnets and IP ranges can be used in the VPC\.
   + **Subnet** \- The subnets in the VPC that is associated with the database\.

1. You can also choose to **Turn on enhanced VPC routing**, which enhances security\.

1. Choose **Save configuration** to complete setup\.

1. After setup completes, choose **Continue** to go to your **Serverless dashboard**\.