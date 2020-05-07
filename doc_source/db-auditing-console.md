# Configuring auditing using the console<a name="db-auditing-console"></a>

You can configure Amazon Redshift to create audit log files and store them in S3\.

## Enabling audit logging using the console<a name="enable-auditing-logging-task"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-audit-logging"></a>

**To enable audit logging for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to update\. 

1. Choose the **Maintenance and monitoring** tab\. Then view the **Audit logging** section\. 

1. Choose **Edit** tab\.

1. On the **Configure audit logging** page, choose to **Enable audit logging** and enter your choices regarding where the logs are stored\. 

1. Choose **Confirm** to save your choices\. 

### Original console<a name="cluster-audit-logging-originalconsole"></a>

**To enable audit logging for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the list, choose the cluster for which you want to enable logging\.

1. In the cluster details page, choose **Database**, and then choose **Configure Audit Logging**\.

1. In the **Configure Audit Logging** dialog box, in the **Enable Audit Logging** box, choose **Yes**\.

1. For **S3 Bucket**, do one of the following:
   + If you already have an S3 bucket that you want to use, select **Use Existing** and then select the bucket from the **Bucket** list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-existing.png)
   + If you need a new S3 bucket, select **Create New**, and in the **New Bucket Name** box, type a name\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-new.png)

1. \(Optional\) For **S3 Key Prefix**, enter a prefix to add to the S3 bucket\.

1. Choose **Save**\.

 After you configure audit logging, the **Cluster** details page updates to display information about the logging configuration\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-details.png)

 On the **Cluster** details page, under **Backup, Maintenance, and Logging**, choose **Go to the S3 console** to navigate to the bucket\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-S3.png)

## Modifying the bucket for audit logging<a name="modify-auditing-logging-task"></a>

### \(Original console\) Modifying the Amazon S3 bucket for audit logging<a name="cluster-audit-logging-modify-originalconsole"></a>

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the list, choose the cluster for which you want to modify the bucket used for audit logging\.

1. In the cluster details page, choose **Database**, and then choose **Configure Audit Logging**\.

1. For **S3 Bucket**, select an existing bucket or create a new bucket\.

1. \(Optional\) For **S3 Key Prefix**, enter a prefix to add to the S3 bucket\.

1. Choose **Save**\.

## Disabling audit logging using the Console<a name="disable-auditing-logging-task"></a>

### \(Original console\) Disabling audit logging<a name="cluster-audit-logging-disable-originalconsole"></a>

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the list, choose the cluster for which you want to disable logging\.

1. In the cluster details page, choose **Database**, and then choose **Configure Audit Logging**\.

1. In the **Configure Audit Logging** dialog box, in the **Enable Audit Logging** box, choose **No**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-disable.png)

1. Choose **Save**\.