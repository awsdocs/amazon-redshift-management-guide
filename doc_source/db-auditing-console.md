# Configuring Auditing Using the Console<a name="db-auditing-console"></a>

You can configure Amazon Redshift to create audit log files and store them in S3\.

## Enabling Audit Logging Using the Console<a name="enable-auditing-logging-task"></a>

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Clusters**\.

1. In the list, click the cluster for which you want to enable logging\.

1. In the cluster details page, click **Database**, and then click **Configure Audit Logging**\.

1. In the **Configure Audit Logging** dialog box, in the **Enable Audit Logging** box, click **Yes**\.

1. For **S3 Bucket**, do one of the following:

   + If you already have an S3 bucket that you want to use, select **Use Existing** and then select the bucket from the **Bucket** list\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-existing.png)

   + If you need a new S3 bucket, select **Create New**, and in the **New Bucket Name** box, type a name\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-new.png)

1. Optionally, in the **S3 Key Prefix** box, type a prefix to add to the S3 bucket\.

1. Click **Save**\.

 After you configure audit logging, the **Cluster** details page updates to display information about the logging configuration\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-details.png)

 On the **Cluster** details page, under **Backup, Maintenance, and Logging**, click **Go to the S3 console** to navigate to the bucket\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-S3.png)

## Modifying the Bucket for Audit Logging<a name="modify-auditing-logging-task"></a>

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Clusters**\.

1. In the list, click the cluster for which you want to modify the bucket used for audit logging\.

1. In the cluster details page, click **Database**, and then click **Configure Audit Logging**\.

1. For **S3 Bucket**, select an existing bucket or create a new bucket\.

1. Optionally, in the **S3 Key Prefix** box, type a prefix to add to the S3 bucket\.

1. Click **Save**\.

## Disabling Audit Logging Using the Console<a name="disable-auditing-logging-task"></a>

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Clusters**\.

1. In the list, click the cluster for which you want to disable logging\.

1. In the cluster details page, click **Database**, and then click **Configure Audit Logging**\.

1. In the **Configure Audit Logging** dialog box, in the **Enable Audit Logging** box, click **No**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cluster-audit-logging-disable.png)

1. Click **Save**\.