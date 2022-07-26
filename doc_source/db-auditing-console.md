# Configuring auditing using the console<a name="db-auditing-console"></a>

Configure Amazon Redshift to export audit log data\. Logs can be exported to CloudWatch, or as files to Amazon S3 buckets\.

## Enabling audit logging using the console<a name="enable-auditing-logging-task"></a>

### Console steps<a name="cluster-audit-logging"></a>

**To enable audit logging for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster that you want to update\. 

1. Choose the **Properties** tab\. On the **Database configurations** panel, choose **Edit**, then **Edit audit logging**\.

1. On the **Edit audit logging** page, choose **Turn on** and select **S3 bucket** or **CloudWatch**\. We recommend using CloudWatch because administration is easy and it has helpful features for data visualization\.

1. Choose which logs to export\.

1. To save your choices, choose **Save changes**\.