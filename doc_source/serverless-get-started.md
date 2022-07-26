# Get started with Amazon Redshift Serverless<a name="serverless-get-started"></a>

To set up and use Amazon Redshift Serverless, set up your serverless data warehouse and create a database\. To do so, you need IAM permission as described in [ Using identity\-based policies \(IAM policies\) for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html) in the *Amazon Redshift Cluster Management Guide*\. In addition, you must attach a policy similar to the following policy to your IAM role or IAM user\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift-serverless:*",
            "Resource": "*"
        }
    ]
}
```

To get started, open the AWS Management Console, choose the Amazon Redshift console, and then choose **Try Amazon Redshift Serverless**\. 

If you have the correct AWS Identity and Access Management \(IAM\) permissions, the first time you access the Amazon Redshift Serverless console you view the **Get started with Amazon Redshift Serverless** page\. 

Here is where you can **Use default settings** or **Customize settings** to create your namespace, database, and workgroup\. When you customize, you can see database settings\. permissions settings, and network settings\. For more information on the customization steps, see [Setting up Amazon Redshift Serverless for the first time](serverless-console-getting-started.md)\.

Amazon Redshift Serverless initializes the resources for your AWS account in the current AWS Region\. The initialization process can take a few minutes to set up the environment\. The Amazon Redshift query editor v2 is opened in a new tab for you to start using Amazon Redshift Serverless\.

## Permissions required to use Amazon Redshift Serverless<a name="serverless-endpoint-iam-role"></a>

When you use Amazon Redshift Serverless, the IAM role you associate to your Amazon Redshift Serverless instance needs a trust relationship with both `redshift.amazonaws.com` and `redshift-serverless.amazonaws.com` to allow Amazon Redshift to assume permissions on your behalf\. 

The following example shows the policy document in JSON format to set up a trust relationship with Amazon Redshift Serverless\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "redshift-serverless.amazonaws.com",
                    "redshift.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

For more information about trust entities, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.