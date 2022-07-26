# Changing the AWS KMS key for a namespace<a name="serverless-workgroups-and-namespaces-rotate-kms-key"></a>

In Amazon Redshift, encryption protects data at rest\. Amazon Redshift Serverless uses AWS KMS key encryption automatically to encrypt both your Amazon Redshift Serverless resources and snapshots\. As a best practice, most organizations review the type of data they store and have a plan to rotate encryption keys on a schedule\. The frequency for rotating keys can vary, depending on your policies for data security\. Amazon Redshift Serverless supports changing the AWS KMS key for the namespace so you can adhere to your organization's security policies\.

When you change the AWS KMS key, the data remains unchanged\.

## Changing an AWS KMS key using the console<a name="serverless-workgroups-and-namespaces-rotate-kms-key-console"></a>

In Amazon Redshift, encryption protects data at rest\. Amazon Redshift Serverless uses AWS KMS key encryption automatically to encrypt both Amazon Redshift Serverless and snapshots\. As a best practice, most organizations review the type of data they store and have a plan to rotate encryption keys on a schedule\. The frequency for rotating keys can vary, depending on your policies for data security\. Amazon Redshift Serverless supports changing the AWS KMS key for the namespace so you can adhere to your organization's security policies\.

When you change the AWS KMS key, the data remains unchanged\.

1. Sign in to the [AWS console](console.aws.amazon.com)\. Select the Amazon Redshift console\.

1. On the navigation menu, choose **Namespace configuration**\. Choose your namespace from the list\.

1. From the **Security and encryption** tab, choose **Edit**\.

1. Choose **Customize encryption settings** and then choose a key for the namespace\. You can optionally create a new key\.

## Changing AWS KMS encryption keys using the AWS CLI<a name="serverless-workgroups-and-namespaces-rotate-kms-key-cli"></a>

Use `update-namespace` to change the AWS KMS key for the namespace\. The following shows the syntax for the command:

```
aws redshift-serverless update-namespace
--namespace-name
[--kms-key-id <id-of-kms-key>]
// other parameters omitted here
```

You must have a namespace created or the CLI command results in an error\. 

The time it takes to change the key depends on the amount of data in Amazon Redshift Serverless\. This typically takes fifteen minutes per 8TB of stored data\.

## Limitations<a name="serverless-workgroups-and-namespaces-rotate-kms-key-limitations"></a>

You can’t change from a customer managed KMS Key to an AWS KMS key\. In this case, you have to create a new namespace\.

You can’t perform other actions while the key is being changed\.