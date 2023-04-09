# Calling the Data API<a name="data-api-calling"></a>

You can call the Data API or the AWS CLI to run SQL statements on your cluster or serverless workgroup\. The primary operations to run SQL statements are [https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html](https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html) and [https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_BatchExecuteStatement.html](https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_BatchExecuteStatement.html) in the *Amazon Redshift Data API Reference*\. The Data API supports the programming languages that are supported by the AWS SDK\. For more information on these, see [Tools to Build on AWS](https://aws.amazon.com/tools/)\.

To see code examples of calling the Data API, see [Getting Started with Redshift Data API](https://github.com/aws-samples/getting-started-with-amazon-redshift-data-api#getting-started-with-redshift-data-api) in *GitHub*\. This repository has examples of using AWS Lambda to access Amazon Redshift data from Amazon EC2, AWS Glue Data Catalog, and Amazon SageMaker\. Example programming languages include Python, Go, Java, and Javascript\.

You can call the Data API using the AWS CLI\.

The following examples use the AWS CLI to call the Data API\. To run the examples, edit the parameter values to match your environment\. In many of the examples a `cluster-identifier` is provided to run against a cluster\. When you run against a serverless workgroup, you provide a `workgroup-name` instead\. These examples demonstrate a few of the Data API operations\. For more information, see the *AWS CLI Command Reference*\.   

Commands in the following examples have been split and formatted for readability\.

## To run a SQL statement<a name="data-api-calling-cli-execute-statement"></a>

To run a SQL statement, use the `aws redshift-data execute-statement` AWS CLI command\.

The following AWS CLI command runs a SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --sql "select * from stl_query limit 1" 
    --database dev
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598323175.823,
    "Database": "dev",
    "Id": "c016234e-5c6c-4bc5-bb16-2c5b8ff61814",
    "SecretArn": "arn:aws:secretsmanager:us-west-2:123456789012:secret:yanruiz-secret-hKgPWn"
}
```

The following AWS CLI command runs a SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --db-user myuser 
    --cluster-identifier mycluster-test 
    --database dev 
    --sql "select * from stl_query limit 1"
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598306924.632,
    "Database": "dev",
    "DbUser": "myuser",
    "Id": "d9b6c0c9-0747-4bf4-b142-e8883122f766"
}
```

The following AWS CLI command runs a SQL statement against a serverless workgroup and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

```
aws redshift-data execute-statement 
    --database dev 
    --workgroup-name myworkgroup 
    --sql "select 1;"
```

The following is an example of the response\.

```
{
 "CreatedAt": "2022-02-11T06:25:28.748000+00:00",
 "Database": "dev",
 "DbUser": "IAMR:RoleName",
 "Id": "89dd91f5-2d43-43d3-8461-f33aa093c41e",
 "WorkgroupName": "myworkgroup"
}
```

The following AWS CLI command runs a SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method and an idempotency token\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --sql "select * from stl_query limit 1" 
    --database dev 
    --client-token b855dced-259b-444c-bc7b-d3e8e33f94g1
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598323175.823,
    "Database": "dev",
    "Id": "c016234e-5c6c-4bc5-bb16-2c5b8ff61814",
    "SecretArn": "arn:aws:secretsmanager:us-west-2:123456789012:secret:yanruiz-secret-hKgPWn"
}
```

## To run a SQL statement with parameters<a name="data-api-calling-cli-execute-statement-parameters"></a>

To run a SQL statement, use the `aws redshift-data execute-statement` AWS CLI command\.

  The following AWS CLI command runs a SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method\. The SQL text has named parameter `distance`\. In this case, the distance used in the predicate is `5`\. In a SELECT statement, named parameters for column names can only be used in the predicate\. Values for named parameters for the SQL statement are specified in the `parameters` option\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --sql "SELECT ratecode FROM demo_table WHERE trip_distance > :distance"  
    --parameters "[{\"name\": \"distance\", \"value\": \"5\"}]"
    --database dev
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598323175.823,
    "Database": "dev",
    "Id": "c016234e-5c6c-4bc5-bb16-2c5b8ff61814",
    "SecretArn": "arn:aws:secretsmanager:us-west-2:123456789012:secret:yanruiz-secret-hKgPWn"
}
```

The following example uses the `EVENT` table from the sample database\. For more information, see [EVENT table](https://docs.aws.amazon.com/redshift/latest/dg/r_eventtable.html) in the *Amazon Redshift Database Developer Guide*\. 

If you don't already have the `EVENT` table in your database, you can create one using the Data API as follows:

```
aws redshift-data execute-statement 
--database dev
--cluster-id my-test-cluster
--db-user awsuser
--sql "create table event( eventid integer not null distkey, 
                           venueid smallint not null, 
                           catid smallint not null, 
                           dateid smallint not null sortkey, 
                           eventname varchar(200), 
                           starttime timestamp)"
```

The following command inserts one row into the `EVENT` table\. 

```
aws redshift-data execute-statement 
--database dev
--cluster-id my-test-cluster
--db-user awsuser 
--sql "insert into event values(:eventid, :venueid::smallint, :catid, :dateid, :eventname, :starttime)" 
--parameters "[{\"name\": \"eventid\", \"value\": \"1\"}, {\"name\": \"venueid\", \"value\": \"1\"}, 
               {\"name\": \"catid\", \"value\": \"1\"}, 
               {\"name\": \"dateid\", \"value\": \"1\"}, 
               {\"name\": \"eventname\", \"value\": \"event 1\"}, 
               {\"name\": \"starttime\", \"value\": \"2022-02-22\"}]"
```

The following command inserts a second row into the `EVENT` table\. This example demonstrates the following: 
+ The parameter named `id` is used four times in the SQL text\.
+ Implicit type conversion is applied automatically when inserting parameter `starttime`\.
+ The `venueid` column is type cast to SMALLINT data type\.
+ Character strings that represent the DATE data type are implicitly converted into the TIMESTAMP data type\.
+ Comments can be used within SQL text\.

```
aws redshift-data execute-statement 
--database dev
--cluster-id my-test-cluster
--db-user awsuser 
--sql "insert into event values(:id, :id::smallint, :id, :id, :eventname, :starttime) /*this is comment, and it won't apply parameterization for :id, :eventname or :starttime here*/" 
--parameters "[{\"name\": \"eventname\", \"value\": \"event 2\"}, 
               {\"name\": \"starttime\", \"value\": \"2022-02-22\"}, 
               {\"name\": \"id\", \"value\": \"2\"}]"
```

The following shows the two inserted rows:

```
 eventid | venueid | catid | dateid | eventname |      starttime
---------+---------+-------+--------+-----------+---------------------
       1 |       1 |     1 |      1 | event 1   | 2022-02-22 00:00:00
       2 |       2 |     2 |      2 | event 2   | 2022-02-22 00:00:00
```

The following command uses a named parameter in a WHERE clause to retrieve the row where `eventid` is `1`\. 

```
aws redshift-data execute-statement 
--database dev
--cluster-id my-test-cluster
--db-user awsuser 
--sql "select * from event where eventid=:id"
--parameters "[{\"name\": \"id\", \"value\": \"1\"}]"
```

Run the following command to get the SQL results of the previous SQL statement:

```
aws redshift-data get-statement-result --id 7529ad05-b905-4d71-9ec6-8b333836eb5a        
```

Provides the following results:

```
{
    "Records": [
        [
            {
                "longValue": 1
            },
            {
                "longValue": 1
            },
            {
                "longValue": 1
            },
            {
                "longValue": 1
            },
            {
                "stringValue": "event 1"
            },
            {
                "stringValue": "2022-02-22 00:00:00.0"
            }
        ]
    ],
    "ColumnMetadata": [
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "eventid",
            "length": 0,
            "name": "eventid",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "venueid",
            "length": 0,
            "name": "venueid",
            "nullable": 0,
            "precision": 5,
            "scale": 0,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "int2"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "catid",
            "length": 0,
            "name": "catid",
            "nullable": 0,
            "precision": 5,
            "scale": 0,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "int2"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "dateid",
            "length": 0,
            "name": "dateid",
            "nullable": 0,
            "precision": 5,
            "scale": 0,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "int2"
        },
        {
            "isCaseSensitive": true,
            "isCurrency": false,
            "isSigned": false,
            "label": "eventname",
            "length": 0,
            "name": "eventname",
            "nullable": 1,
            "precision": 200,
            "scale": 0,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "varchar"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "label": "starttime",
            "length": 0,
            "name": "starttime",
            "nullable": 1,
            "precision": 29,
            "scale": 6,
            "schemaName": "public",
            "tableName": "event",
            "typeName": "timestamp"
        }
    ],
    "TotalNumRows": 1
}
```

## To run multiple SQL statements<a name="data-api-calling-cli-batch-execute-statement"></a>

To run multiple SQL statements with one command, use the `aws redshift-data batch-execute-statement` AWS CLI command\.

The following AWS CLI command runs three SQL statements against a cluster and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

```
aws redshift-data batch-execute-statement 
    --region us-west-2 
    --db-user myuser 
    --cluster-identifier mycluster-test 
    --database dev 
    --sqls "set timezone to BST" "select * from mytable" "select * from another_table"
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598306924.632,
    "Database": "dev",
    "DbUser": "myuser",
    "Id": "d9b6c0c9-0747-4bf4-b142-e8883122f766"
}
```

## To list metadata about SQL statements<a name="data-api-calling-cli-list-statements"></a>

To list metadata about SQL statements, use the `aws redshift-data list-statements` AWS CLI command\. Authorization to run this command is based on the caller's IAM permissions\.

The following AWS CLI command lists SQL statements that ran\.

```
aws redshift-data list-statements 
    --region us-west-2 
    --status ALL
```

The following is an example of the response\.

```
{
    "Statements": [
        {
            "CreatedAt": 1598306924.632,
            "Id": "d9b6c0c9-0747-4bf4-b142-e8883122f766",
            "QueryString": "select * from stl_query limit 1",
            "Status": "FINISHED",
            "UpdatedAt": 1598306926.667
        },
        {
            "CreatedAt": 1598311717.437,
            "Id": "e0ebd578-58b3-46cc-8e52-8163fd7e01aa",
            "QueryString": "select * from stl_query limit 1",
            "Status": "FAILED",
            "UpdatedAt": 1598311719.008
        },
        {
            "CreatedAt": 1598313683.65,
            "Id": "c361d4f7-8c53-4343-8c45-6b2b1166330c",
            "QueryString": "select * from stl_query limit 1",
            "Status": "ABORTED",
            "UpdatedAt": 1598313685.495
        },
        {
            "CreatedAt": 1598306653.333,
            "Id": "a512b7bd-98c7-45d5-985b-a715f3cfde7f",
            "QueryString": "select 1",
            "Status": "FINISHED",
            "UpdatedAt": 1598306653.992
        }
    ]
}
```

## To describe metadata about a SQL statement<a name="data-api-calling-cli-describe-statement"></a>

To get descriptions of metadata for a SQL statement, use the `aws redshift-data describe-statement` AWS CLI command\. Authorization to run this command is based on the caller's IAM permissions\. 

The following AWS CLI command describes a SQL statement\. 

```
aws redshift-data describe-statement 
    --id d9b6c0c9-0747-4bf4-b142-e8883122f766 
    --region us-west-2
```

The following is an example of the response\.

```
{
    "ClusterIdentifier": "mycluster-test",
    "CreatedAt": 1598306924.632,
    "Duration": 1095981511,
    "Id": "d9b6c0c9-0747-4bf4-b142-e8883122f766",
    "QueryString": "select * from stl_query limit 1",
    "RedshiftPid": 20859,
    "RedshiftQueryId": 48879,
    "ResultRows": 1,
    "ResultSize": 4489,
    "Status": "FINISHED",
    "UpdatedAt": 1598306926.667
}
```

The following is an example of a `describe-statement` response after running a `batch-execute-statement` command with multiple SQL statements\.

```
{
    "ClusterIdentifier": "mayo",
    "CreatedAt": 1623979777.126,
    "Duration": 6591877,
    "HasResultSet": true,
    "Id": "b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652",
    "RedshiftPid": 31459,
    "RedshiftQueryId": 0,
    "ResultRows": 2,
    "ResultSize": 22,
    "Status": "FINISHED",
    "SubStatements": [
        {
            "CreatedAt": 1623979777.274,
            "Duration": 3396637,
            "HasResultSet": true,
            "Id": "b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652:1",
            "QueryString": "select 1;",
            "RedshiftQueryId": -1,
            "ResultRows": 1,
            "ResultSize": 11,
            "Status": "FINISHED",
            "UpdatedAt": 1623979777.903
        },
        {
            "CreatedAt": 1623979777.274,
            "Duration": 3195240,
            "HasResultSet": true,
            "Id": "b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652:2",
            "QueryString": "select 2;",
            "RedshiftQueryId": -1,
            "ResultRows": 1,
            "ResultSize": 11,
            "Status": "FINISHED",
            "UpdatedAt": 1623979778.076
        }
    ],
    "UpdatedAt": 1623979778.183
}
```

## To fetch the results of a SQL statement<a name="data-api-calling-cli-get-statement-result"></a>

To fetch the result from a SQL statement that ran, use the `redshift-data get-statement-result` AWS CLI command\. You can provide an `Id` that you receive in response to `execute-statement` or `batch-execute-statement`\. The `Id` value for a SQL statement run by `batch-execute-statement` can be retrieved in the result of `describe-statement` and is suffixed by a colon and sequence number such as `b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652:2`\. If you run multiple SQL statements with `batch-execute-statement`, each SQL statement has an `Id` value as shown in `describe-statement`\.  Authorization to run this command is based on the caller's IAM permissions\. 

The following statement returns the result of a SQL statement run by `execute-statement`\.

```
aws redshift-data get-statement-result 
    --id d9b6c0c9-0747-4bf4-b142-e8883122f766 
    --region us-west-2
```

The following statement returns the result of the second SQL statement run by `batch-execute-statement`\.

```
aws redshift-data get-statement-result 
    --id b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652:2 
    --region us-west-2
```

The following is an example of the response to a call to `get-statement-result`\.

```
{
    "ColumnMetadata": [
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "userid",
            "length": 0,
            "name": "userid",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "query",
            "length": 0,
            "name": "query",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": true,
            "isCurrency": false,
            "isSigned": false,
            "label": "label",
            "length": 0,
            "name": "label",
            "nullable": 0,
            "precision": 320,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "bpchar"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "xid",
            "length": 0,
            "name": "xid",
            "nullable": 0,
            "precision": 19,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int8"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "pid",
            "length": 0,
            "name": "pid",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": true,
            "isCurrency": false,
            "isSigned": false,
            "label": "database",
            "length": 0,
            "name": "database",
            "nullable": 0,
            "precision": 32,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "bpchar"
        },
        {
            "isCaseSensitive": true,
            "isCurrency": false,
            "isSigned": false,
            "label": "querytxt",
            "length": 0,
            "name": "querytxt",
            "nullable": 0,
            "precision": 4000,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "bpchar"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "label": "starttime",
            "length": 0,
            "name": "starttime",
            "nullable": 0,
            "precision": 29,
            "scale": 6,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "timestamp"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "label": "endtime",
            "length": 0,
            "name": "endtime",
            "nullable": 0,
            "precision": 29,
            "scale": 6,
            "schemaName": "",
            "tableName": "stll_query",
            "type": 93,
            "typeName": "timestamp"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "aborted",
            "length": 0,
            "name": "aborted",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "insert_pristine",
            "length": 0,
            "name": "insert_pristine",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "concurrency_scaling_status",
            "length": 0,
            "name": "concurrency_scaling_status",
            "nullable": 0,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "stll_query",
            "typeName": "int4"
        }
    ],
    "Records": [
        [
            {
                "longValue": 1
            },
            {
                "longValue": 3
            },
            {
                "stringValue": "health"
            },
            {
                "longValue": 1023
            },
            {
                "longValue": 15279
            },
            {
                "stringValue": "dev"
            },
            {
                "stringValue": "select system_status from stv_gui_status;"
            },
            {
                "stringValue": "2020-08-21 17:33:51.88712"
            },
            {
                "stringValue": "2020-08-21 17:33:52.974306"
            },
            {
                "longValue": 0
            },
            {
                "longValue": 0
            },
            {
                "longValue": 6
            }
        ]
    ],
    "TotalNumRows": 1
}
```

## To describe a table<a name="data-api-calling-cli-describe-table"></a>

To get metadata that describes a table, use the `aws redshift-data describe-table` AWS CLI command\.

The following AWS CLI command runs a SQL statement against a cluster and returns metadata that describes a table\. This example uses the AWS Secrets Manager authentication method\.

```
aws redshift-data describe-table 
    --region us-west-2  
    --cluster-identifier mycluster-test 
    --database dev 
    --schema information_schema 
    --table sql_features 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn
```

The following is an example of the response\.

```
{
    "ColumnList": [
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "feature_id",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "feature_name",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        }     
    ]
}
```

The following AWS CLI command runs a SQL statement against a cluster that describes a table\. This example uses the temporary credentials authentication method\.

```
aws redshift-data describe-table 
    --region us-west-2 
    --db-user myuser 
    --cluster-identifier mycluster-test 
    --database dev 
    --schema information_schema 
    --table sql_features
```

The following is an example of the response\.

```
{
    "ColumnList": [
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "feature_id",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "feature_name",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "sub_feature_id",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "sub_feature_name",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "is_supported",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "is_verified_by",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        },
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": false,
            "length": 2147483647,
            "name": "comments",
            "nullable": 1,
            "precision": 2147483647,
            "scale": 0,
            "schemaName": "information_schema",
            "tableName": "sql_features",
            "typeName": "character_data"
        }
    ]
}
```

## To list the databases in a cluster<a name="data-api-calling-cli-list-databases"></a>

To list the databases in a cluster, use the `aws redshift-data list-databases` AWS CLI command\.

The following AWS CLI command runs a SQL statement against a cluster to list databases\. This example uses the AWS Secrets Manager authentication method\.

```
aws redshift-data list-databases 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --database dev
```

The following is an example of the response\.

```
{
    "Databases": [
        "dev"
    ]
}
```

The following AWS CLI command runs a SQL statement against a cluster to list databases\. This example uses the temporary credentials authentication method\.

```
aws redshift-data list-databases 
    --region us-west-2 
    --db-user myuser 
    --cluster-identifier mycluster-test 
    --database dev
```

The following is an example of the response\.

```
{
    "Databases": [
        "dev"
    ]
}
```

## To list the schemas in a database<a name="data-api-calling-cli-list-schemas"></a>

To list the schemas in a database, use the `aws redshift-data list-schemas` AWS CLI command\.

The following AWS CLI command runs a SQL statement against a cluster to list schemas in a database\. This example uses the AWS Secrets Manager authentication method\.

```
aws redshift-data list-schemas 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --database dev
```

The following is an example of the response\.

```
{
    "Schemas": [
        "information_schema",
        "pg_catalog",
        "pg_internal",
        "public"
    ]
}
```

The following AWS CLI command runs a SQL statement against a cluster to list schemas in a database\. This example uses the temporary credentials authentication method\.

```
aws redshift-data list-schemas 
    --region us-west-2 
    --db-user mysuser 
    --cluster-identifier mycluster-test 
    --database dev
```

The following is an example of the response\.

```
{
    "Schemas": [
        "information_schema",
        "pg_catalog",
        "pg_internal",
        "public"
    ]
}
```

## To list the tables in a database<a name="data-api-calling-cli-list-tables"></a>

To list the tables in a database, use the `aws redshift-data list-tables` AWS CLI command\.

The following AWS CLI command runs a SQL statement against a cluster to list tables in a database\. This example uses the AWS Secrets Manager authentication method\.

```
aws redshift-data list-tables 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --database dev 
    --schema information_schema
```

The following is an example of the response\.

```
{
    "Tables": [
        {
            "name": "sql_features",
            "schema": "information_schema",
            "type": "SYSTEM TABLE"
        },
        {
            "name": "sql_implementation_info",
            "schema": "information_schema",
            "type": "SYSTEM TABLE"
        }
}
```

The following AWS CLI command runs a SQL statement against a cluster to list tables in a database\. This example uses the temporary credentials authentication method\.

```
aws redshift-data list-tables 
     --region us-west-2 
     --db-user myuser 
     --cluster-identifier mycluster-test 
     --database dev 
     --schema information_schema
```

The following is an example of the response\.

```
{
    "Tables": [
        {
            "name": "sql_features",
            "schema": "information_schema",
            "type": "SYSTEM TABLE"
        },
        {
            "name": "sql_implementation_info",
            "schema": "information_schema",
            "type": "SYSTEM TABLE"
        }
    ]
}
```