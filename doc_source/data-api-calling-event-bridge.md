# Scheduling Amazon Redshift Data API operations with Amazon EventBridge<a name="data-api-calling-event-bridge"></a>

Amazon EventBridge helps you to respond to state changes in your AWS resources\. When your resources change state, they automatically send events into an event stream\. Events are sent to the account that contains the Amazon Redshift database\. You can create rules that match selected events in the stream and route them to targets to take action\. You can also use rules to take action on a predetermined schedule\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. 

To schedule Data API operations with EventBridge, the associated IAM role must trust the principal for CloudWatch Events \(events\.amazonaws\.com\)\. This role should have the equivalent of the managed policy `AmazonEventBridgeFullAccess` attached\. It should also have `AmazonRedshiftDataFullAccess` policy permissions that are managed by the Data API\. You can create an IAM role with these permissions on the IAM console\. When creating a role on the IAM console, choose the AWS service trusted entity for CloudWatch Events\. For more information about creating an IAM role, see [Creating a Role for an AWS Service \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\.

The following example uses the AWS CLI to create an EventBridge rule that is used to run a SQL statement\. 

```
aws events put-rule 
--name test-redshift-data 
--schedule-expression "rate(1 minute)"
```

Then an EventBridge target is created to run on the schedule specified in the rule\. 

```
aws events put-targets 
--cli-input-json file://data.json
```

The input data\.json file is as follows\. 

```
{
    "Rule": "test-redshift-data",
    "EventBusName": "default",
    "Targets": [
        {
            "Id": "2",
            "Arn": "arn:aws:redshift:us-east-1:123456789012:cluster:mycluster",
            "RoleArn": "arn:aws:iam::123456789012:role/Administrator",
            "RedshiftDataParameters": {
                "Database": "dev",
                "DbUser": "root",
                "Sql": "select 1;",
                "StatementName": "test-scheduler-statement",
                "WithEvent": true
            }
        }
    ]
}
```