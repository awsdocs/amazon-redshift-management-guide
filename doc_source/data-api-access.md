# Authorizing access to the Amazon Redshift Data API<a name="data-api-access"></a>

To access the Data API, a user must be authorized\. You can authorize a user to access the Data API by adding a managed policy, which is a predefined AWS Identity and Access Management \(IAM\) policy, to that user\. To see the permissions allowed and denied by managed policies, see the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\. 

Amazon Redshift provides the `AmazonRedshiftDataFullAccess` managed policy\. This policy provides full access to Amazon Redshift Data API operations\. This policy also allows scoped access to specific Amazon Redshift, AWS Secrets Manager, and IAM API operations needed to authenticate and access an Amazon Redshift cluster\. 

You can also create your own IAM policy that allows access to specific resources\. To create your policy, use the `AmazonRedshiftDataFullAccess` policy as your starting template\. After you create your policy, add it to each user that requires access to the Data API\.

Consider the following requirements of the IAM policy associated with the IAM user:
+ If you use AWS Secrets Manager to authenticate, confirm the policy allows use of the `secretsmanager:GetSecretValue` action to retrieve the secret tagged with the key `RedshiftDataFullAccess`\.
+ If you use temporary credentials to authenticate to a cluster, confirm the policy allows the use of the `redshift:GetClusterCredentials` action to the database user name `redshift_data_api_user` for any database in the cluster\. This user name must have already been created in your database\.
+ If you use temporary credentials to authenticate to a serverless workgroup, confirm the policy allows the use of the `redshift-serverless:GetCredentials` action to retrieve the workgroup tagged with the key `RedshiftDataFullAccess`\. The database user is mapped 1:1 to the source AWS Identity and Access Management \(IAM\) identity\. For example, IAM user foo is mapped to database user `IAM:foo`, and IAM role bar is mapped to `IAMR:bar`\.  For more information about IAM identities, see [IAM Identities \(users, user groups, and roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the IAM User Guide\.

To run a query on a cluster that is owned by another account, the owning account must provide an IAM role that the Data API can assume in the calling account\. For example, suppose Account B owns a cluster that Account A needs to access\. Account B can attach the AWS\-managed policy `AmazonRedshiftDataFullAccess` to Account B's IAM role\. Then Account B trusts Account A using a trust policy such as the following:``

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": [
            "arn:aws:iam::accountID-of-account-A:role/someRoleA"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Finally, the Account A IAM role needs to be able to assume the Account B IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::accountID-of-account-B:role/someRoleB"
  }
}
```

The following links provide more information about AWS Identity and Access Management in the *IAM User Guide*\.
+ For information about creating an IAM roles, see [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html)\. 
+ For information about creating an IAM policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.
+ For information about adding an IAM policy to a user, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\. 

## Storing database credentials in AWS Secrets Manager<a name="data-api-secrets"></a>

When you call the Data API, you can pass credentials for the cluster or serverless workgroup by using a secret in AWS Secrets Manager\. To pass credentials in this way, you specify the name of the secret or the Amazon Resource Name \(ARN\) of the secret\. 

To store credentials with Secrets Manager, you need `SecretManagerReadWrite` managed policy permission\. For more information about the minimum permissions, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html) in the *AWS Secrets Manager User Guide*\. 

**To store your credentials in a secret for an Amazon Redshift cluster**

1. Use the AWS Secrets Manager console to create a secret that contains credentials for your cluster:
   + When you choose **Store a new secret**, choose **Credentials for Redshift cluster**\. 
   + Store your values for **User name** \(database user\), **Password**, and **DB cluster **\(cluster identifier\) in your secret\. 
   + Tag the secret with the key `RedshiftDataFullAccess`\. The AWS\-managed policy `AmazonRedshiftDataFullAccess` only allows the action `secretsmanager:GetSecretValue` for secrets tagged with the key `RedshiftDataFullAccess`\. 

   For instructions, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

   Note the name and ARN of the secret\. You can use these in calls to the Data API\.

**To store your credentials in a secret for a serverless workgroup**

1. Use AWS Secrets Manager AWS CLI commands to store a secret that contains credentials for your serverless workgroup:
   + Create your secret in a file, for example a JSON file named `mycreds.json`\. Provide the values for **User name** \(database user\) and **Password** in the file\.

     ```
     {
           "username": "myusername",
           "password": "mypassword"
     }
     ```
   + Store your values in your secret and tag the secret with the key `RedshiftDataFullAccess`\.

     ```
     aws secretsmanager create-secret --name MyRedshiftSecret  --tags Key="RedshiftDataFullAccess",Value="serverless" --secret-string file://mycreds.json
     ```

     The following shows the output\.

     ```
     {
         "ARN": "arn:aws:secretsmanager:region:accountId:secret:MyRedshiftSecret-mvLHxf",
         "Name": "MyRedshiftSecret",
         "VersionId": "a1603925-e8ea-4739-9ae9-e509eEXAMPLE"
     }
     ```

   For more information, see [Creating a Basic Secret with AWS CLI](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html#proc-create-api) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

   Note the name and ARN of the secret\. You can use these in calls to the Data API\.

## Creating an Amazon VPC endpoint \(AWS PrivateLink\) for the Data API<a name="data-api-vpc-endpoint"></a>

Amazon Virtual Private Cloud \(Amazon VPC\) enables you to launch AWS resources, such as Amazon Redshift clusters and applications, into a virtual private cloud \(VPC\)\. AWS PrivateLink provides private connectivity between virtual private clouds \(VPCs\) and AWS services securely on the Amazon network\. Using AWS PrivateLink, you can create VPC endpoints, which you can use connect to services across different accounts and VPCs based on Amazon VPC\. For more information about AWS PrivateLink, see [VPC Endpoint Services \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html) in the *Amazon Virtual Private Cloud User Guide*\.

You can call the Data API with Amazon VPC endpoints\. Using an Amazon VPC endpoint keeps traffic between applications in your Amazon VPC and the Data API in the AWS network, without using public IP addresses\. Amazon VPC endpoints can help you meet compliance and regulatory requirements related to limiting public internet connectivity\. For example, if you use an Amazon VPC endpoint, you can keep traffic between an application running on an Amazon EC2 instance and the Data API in the VPCs that contain them\.

After you create the Amazon VPC endpoint, you can start using it without making any code or configuration changes in your application\.

**To create an Amazon VPC endpoint for the Data API**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, for **Service category**, choose **AWS services**\. For **Service Name**, choose **redshift\-data** \(`com.amazonaws.region.redshift-data`\)\.

1. For **VPC**, choose the VPC to create the endpoint in\.

   Choose the VPC that contains the application that makes Data API calls\.

1. For **Subnets**, choose the subnet for each Availability Zone \(AZ\) used by the AWS service that is running your application\.

   To create an Amazon VPC endpoint, specify the private IP address range in which the endpoint is accessible\. To do this, choose the subnet for each Availability Zone\. Doing so restricts the VPC endpoint to the private IP address range specific to each Availability Zone and also creates an Amazon VPC endpoint in each Availability Zone\.

1. For **Enable DNS name**, select **Enable for this endpoint**\.

   Private DNS resolves the standard Data API DNS hostname \(`https://redshift-data.region.amazonaws.com`\) to the private IP addresses associated with the DNS hostname specific to your Amazon VPC endpoint\. As a result, you can access the Data API VPC endpoint using the AWS CLI or AWS SDKs without making any code or configuration changes to update the Data API endpoint URL\.

1. For **Security group**, choose a security group to associate with the Amazon VPC endpoint\.

   Choose the security group that allows access to the AWS service that is running your application\. For example, if an Amazon EC2 instance is running your application, choose the security group that allows access to the Amazon EC2 instance\. The security group enables you to control the traffic to the Amazon VPC endpoint from resources in your VPC\.

1. Choose **Create endpoint**\.

After the endpoint is created, choose the link in the AWS Management Console to view the endpoint details\.

The endpoint **Details** tab shows the DNS hostnames that were generated while creating the Amazon VPC endpoint\.

You can use the standard endpoint \(`redshift-data.region.amazonaws.com`\) or one of the VPC\-specific endpoints to call the Data API within the Amazon VPC\. The standard Data API endpoint automatically routes to the Amazon VPC endpoint\. This routing occurs because the Private DNS hostname was enabled when the Amazon VPC endpoint was created\.

When you use an Amazon VPC endpoint in a Data API call, all traffic between your application and the Data API remains in the Amazon VPCs that contain them\. You can use an Amazon VPC endpoint for any type of Data API call\. For information about calling the Data API, see [Considerations when calling the Amazon Redshift Data API](data-api.md#data-api-calling-considerations)\.