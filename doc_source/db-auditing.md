# Database audit logging<a name="db-auditing"></a>

Amazon Redshift logs information about connections and user activities in your database\. These logs help you to monitor the database for security and troubleshooting purposes, a process called *database auditing*\. The logs are stored in Amazon S3 buckets\. These provide convenient access with data\-security features for users who are responsible for monitoring activities in the database\.

**Topics**
+ [Amazon Redshift logs](#db-auditing-logs)
+ [Enabling logging](#db-auditing-enable-logging)
+ [Managing log files](#db-auditing-manage-log-files)
+ [Troubleshooting Amazon Redshift audit logging](#db-auditing-failures)
+ [Logging Amazon Redshift API calls with AWS CloudTrail](#rs-db-auditing-cloud-trail)
+ [Amazon Redshift account IDs in AWS CloudTrail logs](#rs-db-auditing-cloud-trail-rs-acct-ids)
+ [Configuring auditing using the console](db-auditing-console.md)
+ [Configuring logging by using the AWS CLI and Amazon Redshift API](db-auditing-cli-api.md)

## Amazon Redshift logs<a name="db-auditing-logs"></a>

Amazon Redshift logs information in the following log files:
+ *Connection log* – Logs authentication attempts, connections, and disconnections\.
+ *User log* – Logs information about changes to database user definitions\.
+ *User activity log* – Logs each query before it's run on the database\.

The connection and user logs are useful primarily for security purposes\. You can use the connection log to monitor information about users connecting to the database and related connection information\. This information might be their IP address, when they made the request, what type of authentication they used, and so on\. You can use the user log to monitor changes to the definitions of database users\. 

The user activity log is useful primarily for troubleshooting purposes\. It tracks information about the types of queries that both the users and the system perform in the database\. 

The connection log and user log both correspond to information that is stored in the system tables in your database\. You can use the system tables to obtain the same information, but the log files provide an easier mechanism for retrieval and review\. The log files rely on Amazon S3 permissions rather than database permissions to perform queries against the tables\. Additionally, by viewing the information in log files rather than querying the system tables, you reduce any impact of interacting with the database\.

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

 Audit logging is not turned on by default in Amazon Redshift\. When you turn on logging on your cluster, Amazon Redshift creates and uploads logs to Amazon S3 that capture data from the time audit logging is enabled to the present time\. Each logging update is a continuation of the information that was already logged\.

**Note**  
Audit logging to Amazon S3 is an optional, manual process\. When you enable logging on your cluster, you are enabling logging to Amazon S3 only\. Logging to system tables is not optional and happens automatically for the cluster\. For more information about logging to system tables, see [System Tables Reference](https://docs.aws.amazon.com/redshift/latest/dg/cm_chap_system-tables.html) in the Amazon Redshift Database Developer Guide\. 

The connection log, user log, and user activity log are enabled together by using the AWS Management Console, the Amazon Redshift API Reference, or the AWS Command Line Interface \(AWS CLI\)\. For the user activity log, you must also enable the `enable_user_activity_logging` database parameter\. If you enable only the audit logging feature, but not the associated parameter, the database audit logs log information for only the connection log and user log, but not for the user activity log\. The `enable_user_activity_logging` parameter is not enabled \(`false`\) by default\. You can set it to `true` to enable the user activity log\. For more information, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\. 

**Note**  
Currently, you can only use Amazon S3\-managed keys \(SSE\-S3\) encryption \(AES\-256\) for audit logging\.

## Managing log files<a name="db-auditing-manage-log-files"></a>

The number and size of Amazon Redshift log files in Amazon S3 depends heavily on the activity in your cluster\. If you have an active cluster that is generating a large number of logs, Amazon Redshift might generate the log files more frequently\. You might have a series of log files for the same type of activity, such as having multiple connection logs within the same hour\.

Because Amazon Redshift uses Amazon S3 to store logs, you incur charges for the storage that you use in Amazon S3\. Before you configure logging, you should have a plan for how long you need to store the log files\. As part of this, determine when the log files can either be deleted or archived based on your auditing needs\. The plan that you create depends heavily on the type of data that you store, such as data subject to compliance or regulatory requirements\. For more information about Amazon S3 pricing, go to [Amazon Simple Storage Service \(S3\) Pricing](https://aws.amazon.com/s3/pricing/)\.

### Bucket permissions for Amazon Redshift audit logging<a name="db-auditing-bucket-permissions"></a>

When you turn on logging, Amazon Redshift collects logging information and uploads it to log files stored in Amazon S3\. You can use an existing bucket or a new bucket\. Amazon Redshift requires the following IAM permissions to the bucket: 
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

Regions that aren't enabled by default, also known as "opt\-in" regions, require a region\-specific service principal name\. For these, the service\-principal name includes the region, in the format `redshift.region.amazonaws.com`\. For example, *redshift\.ap\-east\-1\.amazonaws\.com* for the Asia Pacific \(Hong Kong\) Region\. For a list of the regions that aren't enabled by default, see [Managing AWS Regions](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html) in the *AWS General Reference*\.

**Note**  
The region\-specific service\-principal name corresponds with the region where the cluster is located\.

#### Best practices for S3 bucket permissions<a name="db-auditing-bucket-permissions-confused-deputy"></a>

When you give a third party access to your Amazon S3 buckets, make sure to consider security best practices\. You can provide optional information in the bucket policy to designate the bucket owner\. This way, the account owner can permit the role to be assumed only under specific circumstances, and avoid the *confused\-deputy* problem\. For more information, see [The confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) in the *IAM User Guide*\.

 We recommend that you configure your bucket policy in a way to specify access granted to a service principal specifically on behalf of the bucket owner's \(or their partner's\) resources\. The following example illustrates how you can configure your bucket to grant Amazon Redshift permission to upload logs into the bucket, by specifying the SourceArn, while blocking any other account from uploading log files\. You can use either the [SourceArn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) or [SourceAccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) to specify access\.

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
            ],
            "Condition": {
                "StringEquals": {
                    "aws:SourceArn": "arn:aws:redshift:us-east-1:123456789012:cluster:my-cluster"
                }
            }
        }
    ]
}
```

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

## Troubleshooting Amazon Redshift audit logging<a name="db-auditing-failures"></a>

 Amazon Redshift audit logging can be interrupted for the following reasons: 
+  Amazon Redshift does not have permission to upload logs to the Amazon S3 bucket\. Verify that the bucket is configured with the correct IAM policy\. For more information, see [Bucket permissions for Amazon Redshift audit logging](#db-auditing-bucket-permissions)\. 
+  The bucket owner changed\. When Amazon Redshift uploads logs, it verifies that the bucket owner is the same as when logging was enabled\. If the bucket owner has changed, Amazon Redshift cannot upload logs until you configure another bucket to use for audit logging\. For more information, see [Modifying the bucket for audit logging](db-auditing-console.md#modify-auditing-logging-task)\. 
+  The bucket cannot be found\. If the bucket is deleted in Amazon S3, Amazon Redshift cannot upload logs\. You either need to recreate the bucket or configure Amazon Redshift to upload logs to a different bucket\. For more information, see [Modifying the bucket for audit logging](db-auditing-console.md#modify-auditing-logging-task)\. 

## Logging Amazon Redshift API calls with AWS CloudTrail<a name="rs-db-auditing-cloud-trail"></a>

Amazon Redshift is integrated with AWS CloudTrail, a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Redshift\. CloudTrail captures all API calls for Amazon Redshift as events\. These include calls from the Amazon Redshift console and from code calls to the Amazon Redshift API operations\. If you create a trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon Redshift\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine certain details\. These include the request that was made to Amazon Redshift, the IP address it was made from, who made it, when it was made, and other information\. 

You can use CloudTrail independently from or in addition to Amazon Redshift database audit logging\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

### Amazon Redshift information in CloudTrail<a name="rs-db-auditing-cloud-trail-redshift-info"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Amazon Redshift, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)\. 

For an ongoing record of events in your AWS account, including events for Amazon Redshift, create a trail\. A trail enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all regions\. The trail logs events from all regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see: 
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All Amazon Redshift actions are logged by CloudTrail and are documented in the [Amazon Redshift API Reference](https://docs.aws.amazon.com/redshift/latest/APIReference/)\. For example, calls to the `CreateCluster`, `DeleteCluster`, and `DescribeCluster` actions generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see the [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

### Understanding Amazon Redshift log file entries<a name="rs-db-auditing-cloud-trail-log-file"></a>

A trail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An event represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files are not an ordered stack trace of the public API calls, so they do not appear in any specific order\. 

The following example shows a CloudTrail log entry for a sample CreateCluster call\. 

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

### Working with data sharing information in CloudTrail<a name="cloudtrail-datashare"></a>

All Amazon Redshift data sharing API operations are logged by CloudTrail\. For example, calls to the `AuthorizeDataShare`, `DeauthorizeDataShare`, and `DescribeDataShares` operations generate entries in the CloudTrail log files\. For information about the data sharing API operations, see the [Amazon Redshift API Reference](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Operations.html)\.  

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following:
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for an IAM role or federated user\.
+ Whether the request was made by another AWS service\.

For more information about CloudTrail `userIdentity` element, see [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html)\.

#### Understanding log file entries for data sharing<a name="cloudtrail-log-datashare"></a>

A *trail* in CloudTrail is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An *event* represents a single request from any source\. An event includes information about the requested action, the date and time of the action, or request parameters\. CloudTrail log files aren't an ordered stack trace of the public API calls; they don't appear in any specific order\.

The following example shows a CloudTrail log entry that illustrates the `AuthorizeDataShare` operation\. 

```
{
    "eventVersion": "1.08",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AKIAIOSFODNN7EXAMPLE:janedoe",
        "arn": "arn:aws:sts::111122223333:user/janedoe",
        "accountId": "111122223333",
        "accessKeyId": "AKIAI44QH8DHBEXAMPLE",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AKIAIOSFODNN7EXAMPLE:janedoe",
                "arn": "arn:aws:sts::111122223333:user/janedoe",
                "accountId": "111122223333",
                "userName": "janedoe"
            },
            "attributes": {
                "creationDate": "2021-08-02T23:40:45Z",
                "mfaAuthenticated": "false"
            }
        }
    },
    "eventTime": "2021-08-02T23:40:58Z",
    "eventSource": "redshift.amazonaws.com",
    "eventName": "AuthorizeDataShare",
    "awsRegion": "us-east-1",
    "sourceIPAddress": "3.227.36.75",
    "userAgent":"aws-cli/1.18.118 Python/3.6.10 Linux/4.9.217-0.1.ac.205.84.332.metal1.x86_64 botocore/1.17.41", 
    "requestParameters": {
        "dataShareArn": "arn:aws:redshift:us-east-1:111122223333:datashare:4c64c6ec-73d5-42be-869b-b7f7c43c7a53/testshare",
        "consumerIdentifier": "555555555555"
    },
    "responseElements": {
        "dataShareArn": "arn:aws:redshift:us-east-1:111122223333:datashare:4c64c6ec-73d5-42be-869b-b7f7c43c7a53/testshare",
        "producerNamespaceArn": "arn:aws:redshift:us-east-1:123456789012:namespace:4c64c6ec-73d5-42be-869b-b7f7c43c7a53",
        "producerArn": "arn:aws:redshift:us-east-1:111122223333:namespace:4c64c6ec-73d5-42be-869b-b7f7c43c7a53",
        "allowPubliclyAccessibleConsumers": true,
        "dataShareAssociations": [
            {
                "consumerIdentifier": "555555555555",
                "status": "AUTHORIZED",
                "createdDate": "Aug 2, 2021 11:40:56 PM",
                "statusChangeDate": "Aug 2, 2021 11:40:57 PM"
            }
        ]
    },
    "requestID": "87ee1c99-9e41-42be-a5c4-00495f928422",
    "eventID": "03a3d818-37c8-46a6-aad5-0151803bdb09",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "111122223333",
    "eventCategory": "Management"
}
```

You can use Amazon S3 bucket notification and direct Amazon S3 to publish object\-created events to AWS Lambda\. When CloudTrail writes logs to your S3 bucket, Amazon S3 can then invoke your Lambda function by passing the Amazon S3 object\-created event as a parameter\. Your Lambda function can read this log object and process the access records logged by CloudTrail\. For more information, see [Using AWS Lambda with AWS CloudTrail](https://docs.aws.amazon.com/lambda/latest/dg/with-cloudtrail.html)\.

## Amazon Redshift account IDs in AWS CloudTrail logs<a name="rs-db-auditing-cloud-trail-rs-acct-ids"></a>

When Amazon Redshift calls another AWS service for you, the call is logged with an account ID that belongs to Amazon Redshift\. It isn't logged with your account ID\. For example, suppose that Amazon Redshift calls AWS Key Management Service \(AWS KMS\) operations such as `CreateGrant`, `Decrypt`, `Encrypt`, and `RetireGrant` to manage encryption on your cluster\. In this case, the calls are logged by AWS CloudTrail using an Amazon Redshift account ID\.

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