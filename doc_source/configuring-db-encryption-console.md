# Configuring database encryption using the console<a name="configuring-db-encryption-console"></a>

You can use the Amazon Redshift console to configure Amazon Redshift to use an HSM and to rotate encryption keys\. For information about how to create clusters using AWS KMS encryption keys, see [Creating a cluster](managing-clusters-console.md#create-cluster) and [Managing clusters using the AWS CLI and Amazon Redshift API](manage-clusters-api-cli.md)\.

**To modify database encryption on a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Modify** to display the configuration page\. 

1. In the **Database configuration** section, choose a setting for **Encryption**, then choose **Modify cluster**\. 

## Rotating encryption keys using the Amazon Redshift console<a name="manage-key-rotation-console"></a>

You can use the following procedure to rotate encryption keys by using the Amazon Redshift console\.

**To rotate the encryption keys for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to update encryption keys\.

1. For **Actions**, choose **Rotate encryption** to display the **Rotate encryption keys** page\. 

1. On the **Rotate encryption keys** page, choose **Rotate encryption keys**\. 