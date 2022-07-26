# Logging Amazon Redshift Serverless calls with AWS CloudTrail<a name="serverless-logging-using-cloudtrail"></a>

Amazon Redshift Serverless is integrated with AWS CloudTrail\. CloudTrail is a service that provides a record of actions taken by a user, role, or an AWS service in Amazon Redshift Serverless\. CloudTrail captures all API calls for Amazon Redshift Serverless as events\. The calls captured include calls from the Amazon Redshift Serverless console and code calls to the Amazon Redshift Serverless operations\. 

If you create a CloudTrail trail, you can enable continuous delivery of CloudTrail events to an Amazon S3 bucket, including events for Amazon Redshift Serverless\. If you don't configure a trail, you can still view the most recent events in the CloudTrail console in **Event history**\. Using the information collected by CloudTrail, you can determine certain things\. These include the request that was made to Amazon Redshift Serverless, the IP address from which the request was made, who made the request, when it was made, and additional details\. 

To learn more about CloudTrail, see the [AWS CloudTrail User Guide](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

## Working with Amazon Redshift Serverless information in CloudTrail<a name="service-name-info-in-cloudtrail"></a>

CloudTrail is enabled on your AWS account when you create the account\. When activity occurs in Amazon Redshift Serverless, that activity is recorded in a CloudTrail event along with other AWS service events in **Event history**\. You can view, search, and download recent events in your AWS account\. For more information, see [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) in the *AWS CloudTrail User Guide*\. 

For an ongoing record of events in your AWS account, including events for Amazon Redshift Serverless, create a trail\. A *trail* enables CloudTrail to deliver log files to an Amazon S3 bucket\. By default, when you create a trail in the console, the trail applies to all AWS Regions\. The trail logs events from all Regions in the AWS partition and delivers the log files to the Amazon S3 bucket that you specify\. Additionally, you can configure other AWS services to further analyze and act upon the event data collected in CloudTrail logs\. For more information, see the following in the *AWS CloudTrail User Guide*:
+ [Overview for Creating a Trail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-and-update-a-trail.html)
+ [CloudTrail Supported Services and Integrations](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-aws-service-specific-topics.html#cloudtrail-aws-service-specific-topics-integrations)
+ [Configuring Amazon SNS Notifications for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html)
+ [Receiving CloudTrail Log Files from Multiple Regions](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/receive-cloudtrail-log-files-from-multiple-regions.html) and [Receiving CloudTrail Log Files from Multiple Accounts](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

All Amazon Redshift Serverless actions are logged by CloudTrail\.    For example, calls to the `CreateWorkgroup`, `ListWorkgroups` and `UpdateWorkgroup` actions generate entries in the CloudTrail log files\. 

Every event or log entry contains information about who generated the request\. The identity information helps you determine the following: 
+ Whether the request was made with root or IAM user credentials\.
+ Whether the request was made with temporary security credentials for a role or federated user\.
+ Whether the request was made by another AWS service\.

For more information, see [CloudTrail userIdentity Element](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html) in the *AWS CloudTrail User Guide*\.

## Understanding log file entries for Amazon Redshift Serverless<a name="understanding-service-name-entries"></a>

A *trail* is a configuration that enables delivery of events as log files to an Amazon S3 bucket that you specify\. CloudTrail log files contain one or more log entries\. An *event* represents a single request from any source and includes information about the requested action, the date and time of the action, request parameters, and so on\. CloudTrail log files aren't an ordered stack trace of the public API calls, so they don't appear in any specific order\. 

The following example shows a CloudTrail log entry that demonstrates the `CreateNamespace` action\.

```
{
        "eventVersion": "1.08",
        "userIdentity": {
            "type": "AssumedRole",
            "principalId": "AAKEOFPINEXAMPLE:admin",
            "arn": "arn:aws:sts::111111111111:assumed-role/admin/admin",
            "accountId": "111111111111",
            "accessKeyId": "AAKEOFPINEXAMPLE",
            "sessionContext": {
                "sessionIssuer": {
                    "type": "Role",
                    "principalId": "AAKEOFPINEXAMPLE",
                    "arn": "arn:aws:iam::111111111111:role/admin",
                    "accountId": "111111111111",
                    "userName": "admin"
                },
                "webIdFederationData": {},
                "attributes": {
                    "creationDate": "2022-03-21T20:51:58Z",
                    "mfaAuthenticated": "false"
                }
            }
        },
        "eventTime": "2022-03-21T23:15:40Z",
        "eventSource": "redshift-serverless.amazonaws.com",
        "eventName": "CreateNamespace",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "56.23.155.33",
        "userAgent": "aws-cli/2.4.14 Python/3.8.8 Linux/5.4.181-109.354.amzn2int.x86_64 exe/x86_64.amzn.2 prompt/off command/redshift-serverless.create-namespace",
        "requestParameters": {
            "adminUserPassword": "HIDDEN_DUE_TO_SECURITY_REASONS",
            "adminUsername": "HIDDEN_DUE_TO_SECURITY_REASONS",
            "dbName": "dev",
            "namespaceName": "testnamespace"
        },
        "responseElements": {
            "namespace": {
                "adminUsername": "HIDDEN_DUE_TO_SECURITY_REASONS",
                "creationDate": "Mar 21, 2022 11:15:40 PM",
                "defaultIamRoleArn": "",
                "iamRoles": [],
                "logExports": [],
                "namespaceArn": "arn:aws:redshift-serverless:us-east-1:111111111111:namespace/befa5123-16c2-4449-afca-1d27cb40fc99",
                "namespaceId": "8b726a0c-16ca-4799-acca-1d27cb403599",
                "namespaceName": "testnamespace",
                "status": "AVAILABLE"
            }
        },
        "requestID": "ed4bb777-8127-4dae-aea3-bac009999163",
        "eventID": "1dbee944-f889-4beb-b228-7ad0f312464",
        "readOnly": false,
        "eventType": "AwsApiCall",
        "managementEvent": true,
        "recipientAccountId": "111111111111",
        "eventCategory": "Management",
    }
```