# Scheduling Amazon Redshift Data API operations with Amazon EventBridge<a name="data-api-calling-event-bridge"></a>

You can create rules that match selected events and route them to targets to take action\. You can also use rules to take action on a predetermined schedule\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. 

To schedule Data API operations with EventBridge, the associated IAM role must trust the principal for CloudWatch Events \(events\.amazonaws\.com\)\. This role should have the equivalent of the managed policy `AmazonEventBridgeFullAccess` attached\. It should also have `AmazonRedshiftDataFullAccess` policy permissions that are managed by the Data API\. You can create an IAM role with these permissions on the IAM console\. When creating a role on the IAM console, choose the AWS service trusted entity for CloudWatch Events\. For more information about creating an IAM role, see [Creating a Role for an AWS Service \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\.

The following examples show variations of creating EventBridge rules with a single or multiple SQL statements and with an Amazon Redshift cluster or an Amazon Redshift Serverless workgroup as the data warehouse\.

## Calling with a single SQL statement and cluster<a name="data-api-calling-event-bridge-sql-cluster"></a>

The following example uses the AWS CLI to create an EventBridge rule that is used to run a SQL statement against an Amazon Redshift cluster\.

```
aws events put-rule 
--name test-redshift-cluster-data 
--schedule-expression "rate(1 minute)"
```

Then an EventBridge target is created to run on the schedule specified in the rule\. 

```
aws events put-targets 
--cli-input-json file://data.json
```

The input data\.json file is as follows\. The `Sql` JSON key indicates there is a single SQL statement\. The `Arn` JSON value contains a cluster identifier\. 

```
{
    "Rule": "test-redshift-cluster-data",
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

## Calling with a single SQL statement and workgroup<a name="data-api-calling-event-bridge-sql-workgroup"></a>

The following example uses the AWS CLI to create an EventBridge rule that is used to run a SQL statement against an Amazon Redshift Serverless workgroup\.

```
aws events put-rule 
--name  test-redshift-serverless-workgroup-data 
--schedule-expression "rate(1 minute)"
```

Then an EventBridge target is created to run on the schedule specified in the rule\. 

```
aws events put-targets 
--cli-input-json file://data.json
```

The input data\.json file is as follows\. The `Sql` JSON key indicates there is a single SQL statement\. The `Arn` JSON value contains a workgroup name\. 

```
{
    "Rule": "test-redshift-serverless-workgroup-data", 
    "EventBusName": "default", 
    "Targets": [ 
        {
            "Id": "2",
            "Arn": "arn:aws:redshift-serverless:us-east-1:123456789012:workgroup/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
            "RoleArn": "arn:aws:iam::123456789012:role/Administrator", 
            "RedshiftDataParameters": {
                "Database": "dev",
                "Sql": "select 1;",
                "StatementName": "test-scheduler-statement", 
                "WithEvent": true 
            } 
        } 
    ] 
}
```

## Calling with multiple SQL statements and cluster<a name="data-api-calling-event-bridge-sqls-cluster"></a>

The following example uses the AWS CLI to create an EventBridge rule that is used to run multiple SQL statements against an Amazon Redshift cluster\.

```
aws events put-rule 
--name  test-redshift-cluster-data 
--schedule-expression "rate(1 minute)"
```

Then an EventBridge target is created to run on the schedule specified in the rule\. 

```
aws events put-targets 
--cli-input-json file://data.json
```

The input data\.json file is as follows\. The `Sqls` JSON key indicates there are multiple SQL statements\. The `Arn` JSON value contains a cluster identifier\. 

```
{
    "Rule": "test-redshift-cluster-data", 
    "EventBusName": "default", 
    "Targets": [ 
        {
            "Id": "2",
            "Arn": "arn:aws:redshift:us-east-1:123456789012:cluster:mycluster",
            "RoleArn": "arn:aws:iam::123456789012:role/Administrator", 
            "RedshiftDataParameters": {
                "Database": "dev",
                "Sqls": ["select 1;", "select 2;", "select 3;"],
                "StatementName": "test-scheduler-statement", 
                "WithEvent": true 
            } 
        } 
    ] 
}
```

## Calling with multiple SQL statements and workgroup<a name="data-api-calling-event-bridge-sqls-workgroup"></a>

The following example uses the AWS CLI to create an EventBridge rule that is used to run multiple SQL statements against an Amazon Redshift Serverless workgroup\.

```
aws events put-rule 
--name  test-redshift-serverless-workgroup-data 
--schedule-expression "rate(1 minute)"
```

Then an EventBridge target is created to run on the schedule specified in the rule\. 

```
aws events put-targets 
--cli-input-json file://data.json
```

The input data\.json file is as follows\. The `Sqls` JSON key indicates there are multiple SQL statements\. The `Arn` JSON value contains a workgroup name\. 

```
{
    "Rule": "test-redshift-serverless-workgroup-data", 
    "EventBusName": "default", 
    "Targets": [ 
        {
            "Id": "2",
            "Arn": "arn:aws:redshift-serverless:us-east-1:123456789012:workgroup/a1b2c3d4-5678-90ab-cdef-EXAMPLE11111",
            "RoleArn": "arn:aws:iam::123456789012:role/Administrator", 
            "RedshiftDataParameters": {
                "Database": "dev",
                "Sqls": ["select 1;", "select 2;", "select 3;"],
                "StatementName": "test-scheduler-statement", 
                "WithEvent": true 
            } 
        } 
    ] 
}
```