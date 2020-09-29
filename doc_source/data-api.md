# Using the Amazon Redshift Data API<a name="data-api"></a>

You can access your Amazon Redshift database using the built\-in Amazon Redshift Data API\. Using this API, you can access Amazon Redshift data with web services–based applications, including AWS Lambda, AWS AppSync, Amazon SageMaker notebooks, and AWS Cloud9\. For more information on these applications, see [AWS Lambda](https://aws.amazon.com/lambda/), [AWS AppSync](https://aws.amazon.com/appsync/), [Amazon SageMaker](https://aws.amazon.com/sagemaker/), and [AWS Cloud9](https://aws.amazon.com/cloud9/)\. 

The Data API doesn't require a persistent connection to the cluster\. Instead, it provides a secure HTTP endpoint and integration with AWS SDKs\. You can use the endpoint to run SQL statements without managing connections\. Calls to the Data API are asynchronous\. 

The Data API uses either credentials stored in AWS Secrets Manager or temporary database credentials\. You don't need to pass passwords in the API calls with either authorization method\.  For more information about AWS Secrets Manager, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

For more information about the Data API operations, see [https://docs.aws.amazon.com/redshift-data/latest/APIReference/](https://docs.aws.amazon.com/redshift-data/latest/APIReference/)\.

## Working with the Amazon Redshift Data API<a name="data-api-workflow"></a>

Before you use the Amazon Redshift Data API, review the following steps: 

1. Determine if you, as the caller of the Data API, are authorized\. For more information about authorization, see [Authorizing access to the Amazon Redshift Data API](#data-api-access)\.

1. Determine if you plan to call the Data API with authentication credentials from Secrets Manager or temporary credentials\. For more information, see [Choosing authentication credentials when calling the Amazon Redshift Data API](#data-api-calling-considerations-authentication)\.

1. Set up a secret if you use Secrets Manager for authentication credentials\. For more information, see [Storing database credentials in AWS Secrets Manager](#data-api-secrets)\.

1. Review the considerations and limitations when calling the Data API\. For more information, see [Considerations when calling the Amazon Redshift Data API](#data-api-calling-considerations)\.

1. Call the Data API from the AWS Command Line Interface \(AWS CLI\), from your own code, or using the query editor in the Amazon Redshift console\. For examples of calling from the AWS CLI, see [Calling the Data API with the AWS CLI](#data-api-calling-cli)\.

## Authorizing access to the Amazon Redshift Data API<a name="data-api-access"></a>

To access the Data API, a user must be authorized\. You can authorize a user to access the Data API by adding a managed policy, which is a predefined AWS Identity and Access Management \(IAM\) policy, to that user\. To see the permissions allowed and denied by managed policies, see the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\. 

Amazon Redshift provides the `AmazonRedshiftDataFullAccess` managed policy\. This policy provides full access to Amazon Redshift Data API operations\. This policy also allows scoped access to specific Amazon Redshift, AWS Secrets Manager, and IAM API operations needed to authenticate and access an Amazon Redshift cluster\. If you use AWS Secrets Manager to authenticate, the policy allows use of the `secretsmanager:GetSecretValue` action to retrieve the secret tagged with the key `RedshiftDataFullAccess`\. If you use temporary credentials to authenticate, the policy allows use of the `redshift:GetClusterCredentials` action to the database user name `redshift_data_api_user` for any database in the cluster\. This user name must have already been created in your database\. 

You can also create your own IAM policy that allows access to specific resources\. To create your policy, use the `AmazonRedshiftDataFullAccess` policy as your starting template\. After you create your policy, add it to each user that requires access to the Data API\.

For information about creating an IAM policy, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about adding an IAM policy to a user, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

## Storing database credentials in AWS Secrets Manager<a name="data-api-secrets"></a>

When you call the Data API, you can pass credentials for the cluster by using a secret in AWS Secrets Manager\. To pass credentials in this way, you specify the name of the secret or the Amazon Resource Name \(ARN\) of the secret\. 

To store credentials with Secrets Manager, you need `SecretManagerReadWrite` managed policy permission\. For more information about the minimum permissions, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html) in the *AWS Secrets Manager User Guide*\. 

**To store your credentials in a secret for an Amazon Redshift cluster**

1. Use AWS Secrets Manager to create a secret that contains credentials for your cluster:
   + When you choose **Store a new secret**, choose **Credentials for Redshift cluster**\. 
   + Store your values for **User name** \(database user\), **Password**, and **DB cluster **\(cluster identifier\) in your secret\. 
   + Tag the secret with the key `RedshiftDataFullAccess`\. The AWS\-managed policy `AmazonRedshiftDataFullAccess` only allows the action `secretsmanager:GetSecretValue` for secrets tagged with the key `RedshiftDataFullAccess`\. 

   For instructions, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

   Note the name and ARN of the secret\. You can use these in calls to the Data API\.

## Considerations when calling the Amazon Redshift Data API<a name="data-api-calling-considerations"></a>

Consider the following when calling the Data API:
+ The maximum duration of a query is 24 hours\. 
+ The maximum query result size is 100 MB\. If a call returns more than 100 MB of response data, the call is ended\. 
+ The maximum retention time for query results is 24 hours\. 
+ The maximum query statement size is 100 KB\. 
+ The Data API is available to query single\-node and multiple\-node clusters of the following node types:
  + dc2\.large
  + dc2\.8xlarge
  + ds2\.xlarge
  + ds2\.8xlarge
  + ra3\.4xlarge
  + ra3\.16xlarge
+ The cluster must be in a virtual private cloud \(VPC\) based on the Amazon VPC service\. 

### Choosing authentication credentials when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-authentication"></a>

When you call the Data API, you use one of the following authentication methods for some API operations\. Each method requires a different combination of parameters\. 

**AWS Secrets Manager**  
With this method, provide the `secret-arn` secret value that is stored in AWS Secrets Manager\. The specified secret contains credentials to connect to your database\. You also supply a value for `cluster-identifier` that matches the cluster identifier in the secret\. 

**Temporary credentials**  
With this method, provide your `cluster-identifier`, `database`, and `db-user` values\.

With either method, you can also supply a `region` value that specifies the AWS Region where your cluster is located\. 

### Mapping JDBC data types when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-jdbc"></a>

 The following table maps Java Database Connectivity \(JDBC\) data types to the data types you specify in Data API calls\.


****  

|  JDBC data type  |  Data API data type  | 
| --- | --- | 
|  `INTEGER, TINYINT, SMALLINT, BIGINT`  |  `LONG`  | 
|  `FLOAT, REAL, DOUBLE`  |  `DOUBLE`  | 
|  `DECIMAL`  |  `STRING`  | 
|  `BOOLEAN, BIT`  |  `BOOLEAN`  | 
|  `BLOB, BINARY, LONGVARBINARY, VARBINARY`  |  `BLOB`  | 
|  `CLOB`  |  `STRING`  | 
|  Other types \(including types related to date and time\)  |  `STRING`  | 

For some specific types, such as `DECIMAL` or `TIME`, a hint might be required\. This hint instructs the Data API that the `String` value should be passed to the database as a different type\. You can do this by including values in `typeHint` in the `SqlParameter` data type\. The possible values for `typeHint` are the following:
+ `DECIMAL` – The corresponding `String` parameter value is sent as an object of `DECIMAL` type to the database\.
+ `TIMESTAMP` – The corresponding `String` parameter value is sent as an object of `TIMESTAMP` type to the database\. The accepted format is `YYYY-MM-DD HH:MM:SS[.FFF]`\.
+ `TIME` – The corresponding `String` parameter value is sent as an object of `TIME` type to the database\. The accepted format is `HH:MM:SS[.FFF]`\.
+ `DATE` – The corresponding `String` parameter value is sent as an object of `DATE` type to the database\. The accepted format is `YYYY-MM-DD`\.

**Note**  
Currently, the Data API doesn't support arrays of universal unique identifiers \(UUIDs\)\.

## Calling the Data API<a name="data-api-calling"></a>

You can call the Data API or the AWS CLI to run SQL statements on your cluster\. The primary operation to run an SQL statement is [https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html](https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html)\. The Data API supports the programming languages that are supported by the AWS SDK\. For more information on these, see [Tools to Build on AWS](https://aws.amazon.com/tools/)\.

### Calling the Data API with the AWS CLI<a name="data-api-calling-cli"></a>

You can call the Data API using the AWS CLI\.

The following examples use the AWS CLI to call the Data API\. To run the examples, edit the parameter values to match your environment\. These examples demonstrate a few of the Data API operations\. For more information, see the *AWS CLI Command Reference*\.   

#### To run an SQL statement<a name="data-api-calling-cli-execute-statment"></a>

To run an SQL statement, use the `aws redshift-data execute-statement` AWS CLI command\.

The following AWS CLI command runs an SQL statement and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

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

#### To list metadata about SQL statements<a name="data-api-calling-cli-list-statments"></a>

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
        },
        {
            "CreatedAt": 1598306836.273,
            "Id": "1d765bf4-b124-4ee5-b384-9e3e0417707b",
            "QueryString": "select 1",
            "Status": "CANCELLED",
            "UpdatedAt": 1598306836.944
        }
    ]
}
```

#### To describe metadata about an SQL statement<a name="data-api-calling-cli-describe-statement"></a>

To get descriptions of metadata for an SQL statement, use the `aws redshift-data describe-statement` AWS CLI command\. Authorization to run this command is based on the caller's IAM permissions\. 

The following AWS CLI command describes an SQL statement\. 

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

#### To fetch the results of an SQL statement<a name="data-api-calling-cli-get-statement-result"></a>

To fetch the result from an SQL statement that ran, use the `redshift-data get-statement-result` AWS CLI command\. Authorization to run this command is based on the caller's IAM permissions\. 

```
aws redshift-data get-statement-result 
    --id d9b6c0c9-0747-4bf4-b142-e8883122f766 
    --region us-west-2
```

The following is an example of the response\.

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
                "stringValue": "health                                                                                                                                                                                                                                                                                                                          "
            },
            {
                "longValue": 1023
            },
            {
                "longValue": 15279
            },
            {
                "stringValue": "dev                             "
            },
            {
                "stringValue": "select system_status from stv_gui_status;                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       "
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

#### To describe a table<a name="data-api-calling-cli-describe-table"></a>

To get metadata that describes a table, use the `aws redshift-data describe-table` AWS CLI command\.

The following AWS CLI command runs an SQL statement and returns metadata that describes a table\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement that describes a table\. This example uses the temporary credentials authentication method\.

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

#### To list the databases in a cluster<a name="data-api-calling-cli-list-databases"></a>

To list the databases in a cluster, use the `aws redshift-data list-databases` AWS CLI command\.

The following AWS CLI command runs an SQL statement to list databases\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement to list databases\. This example uses the temporary credentials authentication method\.

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

#### To list the schemas in a database<a name="data-api-calling-cli-list-schemas"></a>

To list the schemas in a database, use the `aws redshift-data list-schemas` AWS CLI command\.

The following AWS CLI command runs an SQL statement to list schemas in a database\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement to list schemas in a database\. This example uses the temporary credentials authentication method\.

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

#### To list the tables in a database<a name="data-api-calling-cli-list-tables"></a>

To list the tables in a database, use the `aws redshift-data list-tables` AWS CLI command\.

The following AWS CLI command runs an SQL statement to list tables in a database\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement to list tables in a database\. This example uses the temporary credentials authentication method\.

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

## Troubleshooting issues for Amazon Redshift Data API<a name="data-api-troubleshooting"></a>

Use the following sections, titled with common error messages, to help troubleshoot problems that you have with the Data API\. 

**Topics**
+ [Packet for query is too large](#data-api-troubleshooting-packet-too-large)
+ [Database response exceeded size limit](#data-api-troubleshooting-response-size-too-large)

### Packet for query is too large<a name="data-api-troubleshooting-packet-too-large"></a>

If you see an error indicating that the packet for a query is too large, generally the result set returned for a row is too large\. The Data API size limit is 64 KB per row in the result set returned by the database\.

To solve this issue, make sure that each row in a result set is 64 KB or less\.

### Database response exceeded size limit<a name="data-api-troubleshooting-response-size-too-large"></a>

If you see an error indicating that the database response has exceeded the size limit, generally the size of the result set returned by the database was too large\. The Data API limit is 100 MB in the result set returned by the database\.

To solve this issue, make sure that calls to the Data API return 100 MB of data or less\. If you need to return more than 100 MB, you can run multiple statement calls with the `LIMIT` clause in your query\.