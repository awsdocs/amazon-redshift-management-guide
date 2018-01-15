# Database Audit Logging<a name="db-auditing"></a>


+ [Overview](#db-auditing-overview)
+ [Amazon Redshift Logs](#db-auditing-logs)
+ [Enabling Logging](#db-auditing-enable-logging)
+ [Managing Log Files](#db-auditing-manage-log-files)
+ [Troubleshooting Amazon Redshift Audit Logging](#db-auditing-failures)
+ [Logging Amazon Redshift API Calls with AWS CloudTrail](#rs-db-auditing-cloud-trail)
+ [Amazon Redshift Account IDs in AWS CloudTrail Logs](#rs-db-auditing-cloud-trail-rs-acct-ids)
+ [Configuring Auditing Using the Console](db-auditing-console.md)
+ [Configuring Logging by Using the Amazon Redshift CLI and API](db-auditing-cli-api.md)

## Overview<a name="db-auditing-overview"></a>

Amazon Redshift logs information about connections and user activities in your database\. These logs help you to monitor the database for security and troubleshooting purposes, which is a process often referred to as database auditing\. The logs are stored in the Amazon Simple Storage Service \(Amazon S3\) buckets for convenient access with data security features for users who are responsible for monitoring activities in the database\.

## Amazon Redshift Logs<a name="db-auditing-logs"></a>

Amazon Redshift logs information in the following log files:

+ *Connection log* — logs authentication attempts, and connections and disconnections\.

+ *User log* — logs information about changes to database user definitions\.

+ *User activity log* — logs each query before it is run on the database\.

The connection and user logs are useful primarily for security purposes\. You can use the connection log to monitor information about the users who are connecting to the database and the related connection information, such as their IP address, when they made the request, what type of authentication they used, and so on\. You can use the user log to monitor changes to the definitions of database users\. 

The user activity log is useful primarily for troubleshooting purposes\. It tracks information about the types of queries that both the users and the system perform in the database\. 

The connection log and user log both correspond to information that is stored in the system tables in your database\. You can use the system tables to obtain the same information, but the log files provide an easier mechanism for retrieval and review\. The log files rely on Amazon S3 permissions rather than database permissions to perform queries against the tables\. Additionally, by viewing the information in log files rather than querying the system tables, you reduce any impact of interacting with the database\.

**Note**  
Log files are not as current as the base system log tables, [STL\_USERLOG](http://docs.aws.amazon.com/redshift/latest/dg/r_STL_USERLOG.html) and [STL\_CONNECTION\_LOG](http://docs.aws.amazon.com/redshift/latest/dg/r_STL_CONNECTION_LOG.html)\. Records that are older than, but not including, the latest record are copied to log files\. 

### Connection Log<a name="db-auditing-connection-log"></a>

Logs authentication attempts, and connections and disconnections\. The following table describes the information in the connection log\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

### User Log<a name="db-auditing-user-log"></a>

 Records details for the following changes to a database user:

+ Create user

+ Drop user

+ Alter user \(rename\)

+ Alter user \(alter properties\)

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

### User Activity Log<a name="db-auditing-user-activity-log"></a>

Logs each query before it is run on the database\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

## Enabling Logging<a name="db-auditing-enable-logging"></a>

 Audit logging is not enabled by default in Amazon Redshift\. When you enable logging on your cluster, Amazon Redshift creates and uploads logs to Amazon S3 that capture data from the creation of the cluster to the present time\. Each logging update is a continuation of the information that was already logged\. 

**Note**  
Audit logging to Amazon S3 is an optional, manual process\. When you enable logging on your cluster, you are enabling logging to Amazon S3 only\. Logging to system tables is not optional and happens automatically for the cluster\. For more information about logging to system tables, see [System Tables Reference](http://docs.aws.amazon.com/redshift/latest/dg/cm_chap_system-tables.html) in the Amazon Redshift Database Developer Guide\. 

The connection log, user log, and user activity log are enabled together by using the AWS Management Console, the Amazon Redshift API Reference, or the AWS Command Line Interface \(AWS CLI\)\. For the user activity log, you must also enable the `enable_user_activity_logging` database parameter\. If you enable only the audit logging feature, but not the associated parameter, the database audit logs will log information for only the connection log and user log, but not for the user activity log\. The `enable_user_activity_logging` parameter is disabled \(`false`\) by default, but you can set it to `true` to enable the user activity log\. For more information, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\. 

## Managing Log Files<a name="db-auditing-manage-log-files"></a>

The number and size of Amazon Redshift log files in Amazon S3 will depend heavily on the activity in your cluster\. If you have an active cluster that is generating a large number of logs, Amazon Redshift might generate the log files more frequently\. You might have a series of log files for the same type of activity, such as having multiple connection logs within the same hour\.

Because Amazon Redshift uses Amazon S3 to store logs, you will incur charges for the storage that you use in Amazon S3\. Before you configure logging, you should have a plan for how long you need to store the log files, and determine when they can either be deleted or archived based on your auditing needs\. The plan that you create depends heavily on the type of data that you store, such as data subject to compliance or regulatory requirements\. For more information about Amazon S3 pricing, go to [Amazon Simple Storage Service \(S3\) Pricing](https://aws.amazon.com/s3/pricing/)\.

### Bucket Permissions for Amazon Redshift Audit Logging<a name="db-auditing-bucket-permissions"></a>

When you enable logging, Amazon Redshift collects logging information and uploads it to log files stored in Amazon S3\. You can use an existing bucket or a new bucket\. Amazon Redshift requires the following IAM permissions to the bucket: 

+ *s3:GetBucketAcl* The service requires read permissions to the Amazon S3 bucket so it can identify the bucket owner\. 

+ *s3:PutObject* The service requires put object permissions to upload the logs\. Each time logs are uploaded, the service determines whether the current bucket owner matches the bucket owner at the time logging was enabled\. If these owners do not match, logging is still enabled but no log files can be uploaded until you select a different bucket\.

If you want to use a new bucket, and have Amazon Redshift create it for you as part of the configuration process, the correct permissions will be applied to the bucket\. However, if you create your own bucket in Amazon S3 or use an existing bucket, you need to add a bucket policy that includes the bucket name, and the Amazon Redshift Account ID that corresponds to your region from the following table:

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

The bucket policy uses the following format, where *BucketName* and *AccountId* are placeholders for your own values: 

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Put bucket policy needed for audit logging",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::AccountId:user/logs"
			},
			"Action": "s3:PutObject",
			"Resource": "arn:aws:s3:::BucketName/*"
		},
		{
			"Sid": "Get bucket policy needed for audit logging ",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::AccountID:user/logs"
			},
			"Action": "s3:GetBucketAcl",
			"Resource": "arn:aws:s3:::BucketName"
		}
	]
}
```

The following example is a bucket policy for the US East \(N\. Virginia\) Region and bucket named AuditLogs\.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Put bucket policy needed for audit logging",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::193672423079:user/logs"
			},
			"Action": "s3:PutObject",
			"Resource": "arn:aws:s3:::AuditLogs/*"
		},
		{
			"Sid": "Get bucket policy needed for audit logging ",
			"Effect": "Allow",
			"Principal": {
				"AWS": "arn:aws:iam::193672423079:user/logs"
			},
			"Action": "s3:GetBucketAcl",
			"Resource": "arn:aws:s3:::AuditLogs"
		}
	]
}
```

For more information about creating Amazon S3 buckets and adding bucket policies, go to [Creating a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/CreatingaBucket.html) and [Editing Bucket Permissions](http://docs.aws.amazon.com/AmazonS3/latest/user-guide/EditingBucketPermissions.html) in the Amazon Simple Storage Service Console User Guide\. 

### Bucket Structure for Amazon Redshift Audit Logging<a name="db-auditing-bucket-structure"></a>

By default, Amazon Redshift organizes the log files in the Amazon S3 bucket by using the following bucket and object structure: `AWSLogs/AccountID/ServiceName/Region/Year/Month/Day/AccountID_ServiceName_Region_ClusterName_LogType_Timestamp.gz` 

For example: `AWSLogs/123456789012/redshift/us-east-1/2013/10/29/123456789012_redshift_us-east-1_mycluster_userlog_2013-10-29T18:01.gz`

If you provide an Amazon S3 key prefix, the prefix is placed at the start of the key\.

For example, if you specify a prefix of myprefix: `myprefix/AWSLogs/123456789012/redshift/us-east-1/2013/10/29/123456789012_redshift_us-east-1_mycluster_userlog_2013-10-29T18:01.gz`

The Amazon S3 key prefix cannot exceed 512 characters\. It cannot contain spaces \( \), double quotation marks \(“\), single quotation marks \(‘\), a backslash \(\\\)\. There are also a number of special characters and control characters that are not allowed\. The hexadecimal codes for these characters are:

+ x00 to x20

+ x22

+ x27

+ x5c

+ x7f or larger

## Troubleshooting Amazon Redshift Audit Logging<a name="db-auditing-failures"></a>

 Amazon Redshift audit logging can be interrupted for the following reasons: 

+  Amazon Redshift does not have permission to upload logs to the Amazon S3 bucket\. Verify that the bucket is configured with the correct IAM policy\. For more information, see [Bucket Permissions for Amazon Redshift Audit Logging](#db-auditing-bucket-permissions)\. 

+  The bucket owner changed\. When Amazon Redshift uploads logs, it verifies that the bucket owner is the same as when logging was enabled\. If the bucket owner has changed, Amazon Redshift cannot upload logs until you configure another bucket to use for audit logging\. For more information, see [Modifying the Bucket for Audit Logging](db-auditing-console.md#modify-auditing-logging-task)\. 

+  The bucket cannot be found\. If the bucket is deleted in Amazon S3, Amazon Redshift cannot upload logs\. You either need to recreate the bucket or configure Amazon Redshift to upload logs to a different bucket\. For more information, see [Modifying the Bucket for Audit Logging](db-auditing-console.md#modify-auditing-logging-task)\. 

## Logging Amazon Redshift API Calls with AWS CloudTrail<a name="rs-db-auditing-cloud-trail"></a>

Amazon Redshift is integrated with AWS CloudTrail, a service that captures Amazon Redshift API calls and delivers the log files to an Amazon S3 bucket that you specify\. CloudTrail captures API calls from the Amazon Redshift console or from your code\. Using the information collected by CloudTrail, you can determine the request that was made to Amazon Redshift, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to turn it on and find your log files, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\. 

You can use CloudTrail independently from or in addition to Amazon Redshift database audit logging\. 

### Amazon Redshift Information in CloudTrail<a name="rs-db-auditing-cloud-trail-redshift-info"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to Amazon Redshift actions are tracked in CloudTrail log files, where they are written with other AWS service records\. CloudTrail determines when to create and write to a new file based on a time period and file size\. 

All Amazon Redshift actions are logged by CloudTrail and are documented in the [Amazon Redshift API Reference](http://docs.aws.amazon.com/redshift/latest/APIReference/)\. For example, calls to the `CreateCluster`, `DeleteCluster`, and `DescribeCluster` operations generate entries in the CloudTrail log files\. 

Every log entry contains information about who generated the request\. The user identity information in the log entry helps you determine the following: 

+ Whether the request was made with root or AWS Identity and Access Management \(IAM\) user credentials 

+ Whether the request was made with temporary security credentials for an [IAM role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) or a [federated user](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html) whose security credentials are validated by an external identity provider instead of directly by AWS 

+ Whether the request was made by another AWS service 

For more information, see the [CloudTrail userIdentity Element](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\. 

You can store your log files in your Amazon S3 bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted with Amazon S3 server\-side encryption \(SSE\)\. 

If you want to be notified upon log file delivery, you can configure CloudTrail to publish Amazon SNS notifications when new log files are delivered\. For more information, see [Configuring Amazon SNS Notifications for CloudTrail]()\. 

You also can aggregate Amazon Redshift log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. 

For more information, see [Receiving CloudTrail Log Files from Multiple Regions](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html) and [Receiving CloudTrail Log Files from Multiple Accounts](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)\.

### Understanding Amazon Redshift Log File Entries<a name="rs-db-auditing-cloud-trail-log-file"></a>

CloudTrail log files can contain one or more log entries\. Each entry lists multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. Log entries are not an ordered stack trace of the public API calls, so they don’t appear in any specific order\. The following example shows a CloudTrail log entry for a sample CreateCluster call\. 

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::123456789012:user/Admin",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "Admin",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2017-03-03T16:51:56Z"
            }
        },
        "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2017-03-03T16:56:09Z",
    "eventSource": "redshift.amazonaws.com",
    "eventName": "CreateCluster",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "52.95.4.13",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
        "clusterIdentifier": "my-dw-instance",
        "allowVersionUpgrade": true,
        "enhancedVpcRouting": false,
        "encrypted": false,
        "clusterVersion": "1.0",
        "masterUsername": "awsuser",
        "masterUserPassword": "****",
        "automatedSnapshotRetentionPeriod": 1,
        "port": 5439,
        "dBName": "mydbtest",
        "clusterType": "single-node",
        "nodeType": "dc1.large",
        "publiclyAccessible": true,
        "vpcSecurityGroupIds": [
            "sg-95f606fc"
        ]
    },
    "responseElements": {
        "nodeType": "dc1.large",
        "preferredMaintenanceWindow": "sat:05:30-sat:06:00",
        "clusterStatus": "creating",
        "vpcId": "vpc-84c22aed",
        "enhancedVpcRouting": false,
        "masterUsername": "awsuser",
        "clusterSecurityGroups": [],
        "pendingModifiedValues": {
            "masterUserPassword": "****"
        },
        "dBName": "mydbtest",
        "clusterVersion": "1.0",
        "encrypted": false,
        "publiclyAccessible": true,
        "tags": [],
        "clusterParameterGroups": [
            {
                "parameterGroupName": "default.redshift-1.0",
                "parameterApplyStatus": "in-sync"
            }
        ],
        "allowVersionUpgrade": true,
        "automatedSnapshotRetentionPeriod": 1,
        "numberOfNodes": 1,
        "vpcSecurityGroups": [
            {
                "status": "active",
                "vpcSecurityGroupId": "sg-95f606fc"
            }
        ],
        "iamRoles": [],
        "clusterIdentifier": "my-dw-instance",
        "clusterSubnetGroupName": "default"
    },
    "requestID": "4c506036-0032-11e7-b8bf-d7aa466e9920",
    "eventID": "13ba5550-56ac-405b-900a-8a42b0f43c45",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```

The following example shows a CloudTrail log entry for a sample DeleteCluster call\.

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "IAMUser",
        "principalId": "AIDAMVNPBQA3EXAMPLE",
        "arn": "arn:aws:iam::123456789012:user/Admin",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "userName": "Admin",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2017-03-03T16:58:23Z"
            }
        },
        "invokedBy": "signin.amazonaws.com"
    },
    "eventTime": "2017-03-03T17:02:34Z",
    "eventSource": "redshift.amazonaws.com",
    "eventName": "DeleteCluster",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "52.95.4.13",
    "userAgent": "signin.amazonaws.com",
    "requestParameters": {
        "clusterIdentifier": "my-dw-instance",
        "skipFinalClusterSnapshot": true
    },
    "responseElements": null,
    "requestID": "324cb76a-0033-11e7-809b-1bbbef7710bf",
    "eventID": "59bcc3ce-e635-4cce-b47f-3419a36b3fa5",
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```

## Amazon Redshift Account IDs in AWS CloudTrail Logs<a name="rs-db-auditing-cloud-trail-rs-acct-ids"></a>

When Amazon Redshift calls another AWS service on your behalf, the call is logged with an account ID that belongs to the Amazon Redshift service instead of your own account ID\. For example, when Amazon Redshift calls AWS Key Management Service \(AWS KMS\) actions such as CreateGrant, Decrypt, Encrypt, and RetireGrant to manage encryption on your cluster, the calls are logged by AWS CloudTrail using an Amazon Redshift account ID\.

Amazon Redshift uses the account IDs in the following table when calling other AWS services\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)

The following example shows a CloudTrail log entry for the AWS KMS Decrypt operation that was called by Amazon Redshift\.

```
{

    "eventVersion": "1.05",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AROAI5QPCMKLTL4VHFCYY:i-0f53e22dbe5df8a89",
        "arn": "arn:aws:sts::790247189693:assumed-role/prod-23264-role-wp/i-0f53e22dbe5df8a89",
        "accountId": "790247189693",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2017-03-03T16:24:54Z"
            },
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AROAI5QPCMKLTL4VHFCYY",
                "arn": "arn:aws:iam::790247189693:role/prod-23264-role-wp",
                "accountId": "790247189693",
                "userName": "prod-23264-role-wp"
            }
        }
    },
    "eventTime": "2017-03-03T17:16:51Z",
    "eventSource": "kms.amazonaws.com",
    "eventName": "Decrypt",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "52.14.143.61",
    "userAgent": "aws-internal/3",
    "requestParameters": {
        "encryptionContext": {
            "aws:redshift:createtime": "20170303T1710Z",
            "aws:redshift:arn": "arn:aws:redshift:us-east-2:123456789012:cluster:my-dw-instance-2"
        }
    },
    "responseElements": null,
    "requestID": "30d2fe51-0035-11e7-ab67-17595a8411c8",
    "eventID": "619bad54-1764-4de4-a786-8898b0a7f40c",
    "readOnly": true,
    "resources": [
        {
            "ARN": "arn:aws:kms:us-east-2:123456789012:key/f8f4f94f-e588-4254-b7e8-078b99270be7",
            "accountId": "123456789012",
            "type": "AWS::KMS::Key"
        }
    ],
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012",
    "sharedEventID": "c1daefea-a5c2-4fab-b6f4-d8eaa1e522dc"

}
```