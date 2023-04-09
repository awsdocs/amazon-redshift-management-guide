# Scheduling a query<a name="query-editor-schedule-query"></a>

To create a schedule to run a SQL statement, you can use the query editor on the Amazon Redshift console\. You can create a schedule to run your SQL statement at the time intervals that match your business needs\. When it's time for the scheduled query to run, Amazon EventBridge initiates the query\.  

**To create a schedule to run a SQL statement**

1. Open the console and query editor as described in [Using the query editor](query-editor.md#using-query-editor)\. You can only use this query editor with provisioned clusters\.

1. Choose **Schedule** to create a schedule to run an SQL statement\. 

   When you define the schedule, you provide the following information:
   + An IAM role that is used to assume the required permissions to run the query\. For more information, see [Setting up permissions to schedule a query on the Amazon Redshift console](#query-editor-schedule-query-permissions)\. 
   + The authentication values for either AWS Secrets Manager or temporary credentials to authorize access your cluster\. For more information, see [Authenticating a scheduled query](#query-editor-schedule-query-authentication)\. 
   + The name of the scheduled query and a single SQL statement to be run\.
   + The schedule frequency and repeat options or a cron formatted value\. 
   + Optionally, you can enable Amazon SNS notifications to monitor the scheduled query\. If your query is being run but you don't see messages published in your SNS topic, see [ My rule is being triggered but I don't see any messages published into my Amazon SNS topic](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-troubleshooting.html#no-messages-published-sns) in the *Amazon EventBridge User Guide*\.  

You can also manage and update scheduled queries using the Amazon Redshift console\. Depending on your version of the console, scheduled queries might be listed in the following places: 
+ On the **Schedules** tab of the details page of your cluster\.
+ On the **Scheduled queries** tab of the query editor\.

If you choose **Schedule name** from one of these locations, you can view and edit your scheduled query's definition\. 

## Setting up permissions to schedule a query on the Amazon Redshift console<a name="query-editor-schedule-query-permissions"></a>

To schedule queries, the AWS Identity and Access Management \(IAM\) user defining the schedule and the IAM role associated with the schedule must be configured as follows\. 

For the IAM user logged into the Amazon Redshift console, do the following: 
+ Attach the `AmazonEventBridgeFullAccess` AWS managed policy to an IAM role and assign the role to the user\.
+ Attach a policy with the `sts:AssumeRole` permission of the IAM role that you specify when you define the scheduled SQL statement\. 

  The following example shows a policy that assumes a specified IAM role\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
          "Sid": "AssumeIAMRole",
          "Effect": "Allow",
          "Action": "sts:AssumeRole",
          "Resource": "arn:aws:iam::account-id:role/sql-statement-iam-role"
          }
      ]
  }
  ```

For the IAM role that you specify to enable the scheduler to run a query, do the following: 
+ Ensure that this IAM role specifies the EventBridge service principal \(`events.amazonaws.com`\)\. The following is an example trust relationship\. 

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Principal": {
                  "Service": [
                      "events.amazonaws.com"
                  ]
              },
              "Action": "sts:AssumeRole"
          }
      ]
  }
  ```

  For more information about how to create an IAM role for EventBridge events, see [Permissions required to use the Amazon EventBridge scheduler](redshift-iam-access-control-identity-based.md#iam-permission-eventbridge-scheduler)\. 
+ Attach the `AmazonRedshiftDataFullAccess` AWS managed policy to the IAM role\. 
+ To allow users to view schedule history, edit the IAM role to add the `sts:AssumeRole` permission\. 

The following is an example of a trust policy in an IAM role\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "events.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

## Authenticating a scheduled query<a name="query-editor-schedule-query-authentication"></a>

When you schedule a query, you use one of the following authentication methods when the query SQL runs\. Each method requires a different combination of input from the Amazon Redshift console\. 

**AWS Secrets Manager**  
With this method, provide a secret value for **secret\-arn** that is stored in AWS Secrets Manager\. This secret contains credentials to connect to your database\. The secret must be tagged with the key `RedshiftDataFullAccess`\.   
For more information about the minimum permissions, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html) in the *AWS Secrets Manager User Guide*\. 

**Temporary credentials**  
With this method, provide your **database** and **db\-user** values\.   
The `AmazonRedshiftDataFullAccess` policy allows the database user named `redshift_data_api_user` permission for `redshift:GetClusterCredentials`\. If you want to use a different database user to run the SQL statement, then add a policy to the IAM role to allow `redshift:GetClusterCredentials`\. The following example policy allows database users `awsuser` and `myuser`\.   

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "UseTemporaryCredentialsForAllDbUsers",
            "Effect": "Allow",
            "Action": "redshift:GetClusterCredentials",
            "Resource": [
                "arn:aws:redshift:*:*:dbuser:*/awsuser",
                "arn:aws:redshift:*:*:dbuser:*/myuser"
            ]
        }
    ]
}
```

## Create an Amazon EventBridge rule that runs when a query finishes<a name="data-api-eventbridge-finished-notification"></a>

You can create an event rule to send a notification when a query finishes\. For the procedure using the Amazon EventBridge console, see [Creating Amazon EventBridge rules that react to events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-create-rule.html) in the *Amazon EventBridge User Guide*\. For more information about event patterns, see [Amazon EventBridge event patterns](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) in the *Amazon EventBridge User Guide*\. 

For example, the following sample event is sent when a query is `FINISHED`\.

```
{
    "version": "0",
    "id": "6a7e8feb-b491-4cf7-a9f1-bf3703467718",
    "detail-type": "Redshift Data Statement Status Change",
    "source": "aws.redshift-data",
    "account": "123456789012",
    "time": "2020-12-22T17:00:00Z",
    "region": "us-west-1",
    "resources": [
        "arn:aws:redshift:us-east-2:123456789:cluster:t1"
    ],
    "detail": {
        "statementId": "01bdaca2-8967-4e34-ae3f-41d9728d5644",
        "clusterId": "test-dataapi",
        "statementName": "awsome query",
        "state": "FINISHED",
        "pages": 5,
        "expireAt": "2020-12-22T18:43:48Z",
        "principal": "arn:aws:sts::123456789012:assumed-role/any",
        "queryId": 123456
    }
}
```

You can create an event pattern rule to filter the event\.

```
{
    "source": [
        "aws.redshift-data"
    ],
    "detail-type": [
        "Redshift Data Statement Status Change"
    ],
    "detail": {
        "state": [
            "FINISHED"
        ]
    }
}
```