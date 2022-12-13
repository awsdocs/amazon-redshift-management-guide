# Database audit logging<a name="db-auditing"></a>

Amazon Redshift logs information about connections and user activities in your database\. These logs help you to monitor the database for security and troubleshooting purposes, a process called *database auditing*\. The logs can be stored in:
+ *Amazon S3 buckets* \- This provides access with data\-security features for users who are responsible for monitoring activities in the database\.
+ *Amazon CloudWatch* \- You can view audit\-logging data using the features built into CloudWatch, such as visualization features and setting actions\.

**Topics**
+ [Amazon Redshift logs](#db-auditing-logs)
+ [Enabling logging](#db-auditing-enable-logging)
+ [Sending audit logs to Amazon CloudWatch](#db-auditing-cloudwatch-provisioned)
+ [Managing log files in Amazon S3](#db-auditing-manage-log-files)
+ [Troubleshooting Amazon Redshift audit logging in Amazon S3](#db-auditing-failures)
+ [Logging Amazon Redshift API calls with AWS CloudTrail](#rs-db-auditing-cloud-trail)
+ [Configuring auditing using the console](db-auditing-console.md)
+ [Configuring logging by using the AWS CLI and Amazon Redshift API](db-auditing-cli-api.md)

## Amazon Redshift logs<a name="db-auditing-logs"></a>

Amazon Redshift logs information in the following log files:
+ *Connection log* – Logs authentication attempts, connections, and disconnections\.
+ *User log* – Logs information about changes to database user definitions\.
+ *User activity log* – Logs each query before it's run on the database\.

The connection and user logs are useful primarily for security purposes\. You can use the connection log to monitor information about users connecting to the database and related connection information\. This information might be their IP address, when they made the request, what type of authentication they used, and so on\. You can use the user log to monitor changes to the definitions of database users\. 

The user activity log is useful primarily for troubleshooting purposes\. It tracks information about the types of queries that both the users and the system perform in the database\. 

The connection log and user log both correspond to information that is stored in the system tables in your database\. You can use the system tables to obtain the same information, but the log files provide a simpler mechanism for retrieval and review\. The log files rely on Amazon S3 permissions rather than database permissions to perform queries against the tables\. Additionally, by viewing the information in log files rather than querying the system tables, you reduce any impact of interacting with the database\.

**Note**  
Log files are not as current as the base system log tables, [STL\_USERLOG](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_USERLOG.html) and [STL\_CONNECTION\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_CONNECTION_LOG.html)\. Records that are older than, but not including, the latest record are copied to log files\. 

### Connection log<a name="db-auditing-connection-log"></a>

Logs authentication attempts, and connections and disconnections\. The following table describes the information in the connection log\. For more information about these fields, see [STL\_CONNECTION\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_CONNECTION_LOG.html) in the *Amazon Redshift Database Developer Guide*\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

### User log<a name="db-auditing-user-log"></a>

 Records details for the following changes to a database user:
+ Create user
+ Drop user
+ Alter user \(rename\)
+ Alter user \(alter properties\)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

### User activity log<a name="db-auditing-user-activity-log"></a>

Logs each query before it is run on the database\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

## Enabling logging<a name="db-auditing-enable-logging"></a>

 Audit logging is not turned on by default in Amazon Redshift\. When you turn on logging on your cluster, Amazon Redshift exports logs to Amazon CloudWatch, or creates and uploads logs to Amazon S3, that capture data from the time audit logging is enabled to the present time\. Each logging update is a continuation of the previous logs\.

Audit logging to CloudWatch or to Amazon S3 is an optional process\. Logging to system tables is not optional and happens automatically\. For more information about logging to system tables, see [System Tables Reference](https://docs.aws.amazon.com/redshift/latest/dg/cm_chap_system-tables.html) in the Amazon Redshift Database Developer Guide\. 

The connection log, user log, and user activity log are enabled together by using the AWS Management Console, the Amazon Redshift API Reference, or the AWS Command Line Interface \(AWS CLI\)\. For the user activity log, you must also enable the `enable_user_activity_logging` database parameter\. If you enable only the audit logging feature, but not the associated parameter, the database audit logs log information for only the connection log and user log, but not for the user activity log\. The `enable_user_activity_logging` parameter is not enabled \(`false`\) by default\. You can set it to `true` to enable the user activity log\. For more information, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\. 

**Note**  
Currently, you can only use Amazon S3\-managed keys \(SSE\-S3\) encryption \(AES\-256\) for audit logging\.

## Sending audit logs to Amazon CloudWatch<a name="db-auditing-cloudwatch-provisioned"></a>

When you enable logging to CloudWatch, Amazon Redshift exports cluster connection, user, and user\-activity log data to an Amazon CloudWatch Logs log group\. The log data doesn't change, in terms of schema\. CloudWatch is built for monitoring applications, and you can use it to perform real\-time analysis or set it to take actions\. You can also use Amazon CloudWatch Logs to store your log records in durable storage\. 

Using CloudWatch to view logs is a recommended alternative to storing log files in Amazon S3\. It doesn't require much configuration, and it may suit your monitoring requirements, especially if you use it already to monitor other services and applications\.

### Log groups and log events in Amazon CloudWatch<a name="db-auditing-cloudwatch-provisioned-log-group"></a>

After selecting which Amazon Redshift logs to export, you can monitor log events in Amazon CloudWatch Logs\. A new log group is automatically created for Amazon Redshift Serverless, under the following prefix, in which `log_type` represents the log type\.

```
/aws/redshift/cluster/<cluster_name>/<log_type>
```

For example, if you choose to export the connection log, log data is stored in the following log group\.

```
/aws/redshift/cluster/cluster1/connectionlog
```

Log events are exported to a log group using the log stream\. To search for information within log events for your serverless endpoint, use the Amazon CloudWatch Logs console, the AWS CLI, or the Amazon CloudWatch Logs API\. For information about searching and filtering log data, see [Creating metrics from log events using filters](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html)\.

In CloudWatch, you can search your log data with a query syntax that provides for granularity and flexibility\. For more information, see [CloudWatch Logs Insights query syntax](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)\.

### Migrating to Amazon CloudWatch audit logging<a name="db-auditing-cloudwatch-provisioned-migration"></a>

In any case where you are sending logs to Amazon S3 and you change the configuration, for example to send logs to CloudWatch, logs that remain in Amazon S3 are unaffected\. You can still query the log data in the Amazon S3 buckets where it resides\.

## Managing log files in Amazon S3<a name="db-auditing-manage-log-files"></a>

The number and size of Amazon Redshift log files in Amazon S3 depends heavily on the activity in your cluster\. If you have an active cluster that is generating a large number of logs, Amazon Redshift might generate the log files more frequently\. You might have a series of log files for the same type of activity, such as having multiple connection logs within the same hour\.

When Amazon Redshift uses Amazon S3 to store logs, you incur charges for the storage that you use in Amazon S3\. Before you configure logging to Amazon S3, plan for how long you need to store the log files\. As part of this, determine when the log files can either be deleted or archived, based on your auditing needs\. The plan that you create depends heavily on the type of data that you store, such as data subject to compliance or regulatory requirements\. For more information about Amazon S3 pricing, go to [Amazon Simple Storage Service \(S3\) Pricing](https://aws.amazon.com/s3/pricing/)\.

### Bucket permissions for Amazon Redshift audit logging<a name="db-auditing-bucket-permissions"></a>

When you turn on logging to Amazon S3, Amazon Redshift collects logging information and uploads it to log files stored in Amazon S3\. You can use an existing bucket or a new bucket\. Amazon Redshift requires the following IAM permissions to the bucket: 
+ `s3:GetBucketAcl` The service requires read permissions to the Amazon S3 bucket so it can identify the bucket owner\. 
+ `s3:PutObject` The service requires put object permissions to upload the logs\. Also, the IAM user or IAM role that enables logging must have `s3:PutObject` permission to the Amazon S3 bucket\. Each time logs are uploaded, the service determines whether the current bucket owner matches the bucket owner at the time logging was enabled\. If these owners don't match, you receive an error\. 

If, when you enable audit logging, you select the option to create a new bucket, correct permissions are applied to it\. However, if you create your own bucket in Amazon S3, or use an existing bucket, make sure to add a bucket policy that includes the bucket name\. Logs are delivered using service\-principal credentials\. For most AWS Regions, you add the Redshift service\-principal name, *redshift\.amazonaws\.com*\. 

The bucket policy uses the following format\. *ServiceName* and *BucketName* are placeholders for your own values\.  Also specify the associated actions and resources in the bucket policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Put bucket policy needed for audit logging",
            "Effect": "Allow",
            "Principal": {
                "Service": "ServiceName"
            },
            "Action": [
                "s3:PutObject",
                "s3:GetBucketAcl"
            ],
            "Resource": [
                "arn:aws:s3:::BucketName",
                "arn:aws:s3:::BucketName/*"
            ]
        }
    ]
}
```

The following example is a bucket policy for the US East \(N\. Virginia\) Region and a bucket named `AuditLogs`\.

```
{
    "Version": "2008-10-17",
    "Statement": [
        {
            "Sid": "Put bucket policy needed for audit logging",
            "Effect": "Allow",
            "Principal": {
                "Service": "redshift.amazonaws.com"
            },
            "Action": [
                "s3:PutObject",
                "s3:GetBucketAcl"
            ],
            "Resource": [
                "arn:aws:s3:::AuditLogs",
                "arn:aws:s3:::AuditLogs/*"
            ]
        }
    ]
}
```

Regions that aren't enabled by default, also known as "opt\-in" Regions, require a Region\-specific service principal name\. For these, the service\-principal name includes the region, in the format `redshift.region.amazonaws.com`\. For example, *redshift\.ap\-east\-1\.amazonaws\.com* for the Asia Pacific \(Hong Kong\) Region\. For a list of the Regions that aren't enabled by default, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.

**Note**  
The Region\-specific service\-principal name corresponds to the Region where the cluster is located\.

#### Best practices for log files<a name="db-auditing-bucket-permissions-confused-deputy"></a>

 When Redshift uploads log files to Amazon S3, large files can be uploaded in parts\. If a multipart upload isn't successful, it's possible for parts of a file to remain in the Amazon S3 bucket\. This can result in additional storage costs, so it's important to understand what occurs when a multipart upload fails\. For a detailed explanation about multipart upload for audit logs, see [Uploading and copying objects using multipart upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/mpuoverview.html) and [Aborting a multipart upload](https://docs.aws.amazon.com/AmazonS3/latest/userguide/abort-mpu.html)\.

For more information about creating S3 buckets and adding bucket policies, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/CreatingaBucket.html) and [Editing Bucket Permissions](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/EditingBucketPermissions.html) in the *Amazon Simple Storage Service User Guide*\. 

### Bucket structure for Amazon Redshift audit logging<a name="db-auditing-bucket-structure"></a>

By default, Amazon Redshift organizes the log files in the Amazon S3 bucket by using the following bucket and object structure: ``

`AWSLogs/AccountID/ServiceName/Region/Year/Month/Day/AccountID_ServiceName_Region_ClusterName_LogType_Timestamp.gz` 

An example is: `AWSLogs/123456789012/redshift/us-east-1/2013/10/29/123456789012_redshift_us-east-1_mycluster_userlog_2013-10-29T18:01.gz`

If you provide an Amazon S3 key prefix, put the prefix at the start of the key\.

For example, if you specify a prefix of myprefix: `myprefix/AWSLogs/123456789012/redshift/us-east-1/2013/10/29/123456789012_redshift_us-east-1_mycluster_userlog_2013-10-29T18:01.gz`

The Amazon S3 key prefix can't exceed 512 characters\. It can't contain spaces \( \), double quotation marks \(“\), single quotation marks \(‘\), a backslash \(\\\)\. There is also a number of special characters and control characters that aren't allowed\. The hexadecimal codes for these characters are as follows:
+ x00 to x20
+ x22
+ x27
+ x5c
+ x7f or larger

## Troubleshooting Amazon Redshift audit logging in Amazon S3<a name="db-auditing-failures"></a>

 Amazon Redshift audit logging can be interrupted for the following reasons: 
+  Amazon Redshift does not have permission to upload logs to the Amazon S3 bucket\. Verify that the bucket is configured with the correct IAM policy\. For more information, see [Bucket permissions for Amazon Redshift audit logging](#db-auditing-bucket-permissions)\. 
+  The bucket owner changed\. When Amazon Redshift uploads logs, it verifies that the bucket owner is the same as when logging was enabled\. If the bucket owner has changed, Amazon Redshift cannot upload logs until you configure another bucket to use for audit logging\. 
+  The bucket cannot be found\. If the bucket is deleted in Amazon S3, Amazon Redshift cannot upload logs\. You either must recreate the bucket or configure Amazon Redshift to upload logs to a different bucket\. 

## Logging Amazon Redshift API calls with AWS CloudTrail<a name="rs-db-auditing-cloud-trail"></a>

Amazon Redshift is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Redshift\. CloudTrail captures all API calls for Amazon Redshift as events\. For more information about Amazon Redshift integration with AWS CloudTrail, see [Logging with CloudTrail](https://docs.aws.amazon.com/redshift/latest/mgmt/logging-with-cloudtrail.html)\.

You can use CloudTrail independently from or in addition to Amazon Redshift database audit logging\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.