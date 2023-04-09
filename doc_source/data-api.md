# Using the Amazon Redshift Data API<a name="data-api"></a>

You can access your Amazon Redshift database using the built\-in Amazon Redshift Data API\. Using this API, you can access Amazon Redshift data with web services–based applications, including AWS Lambda, Amazon SageMaker notebooks, and AWS Cloud9\. For more information on these applications, see [AWS Lambda](https://aws.amazon.com/lambda/),  [Amazon SageMaker](https://aws.amazon.com/sagemaker/), and [AWS Cloud9](https://aws.amazon.com/cloud9/)\. 

The Data API doesn't require a persistent connection to your database\. Instead, it provides a secure HTTP endpoint and integration with AWS SDKs\. You can use the endpoint to run SQL statements without managing connections\. Calls to the Data API are asynchronous\. 

The Data API uses either credentials stored in AWS Secrets Manager or temporary database credentials\. You don't need to pass passwords in the API calls with either authorization method\.  For more information about AWS Secrets Manager, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

For more information about the Data API operations, see the [Amazon Redshift Data API Reference](https://docs.aws.amazon.com/redshift-data/latest/APIReference/)\.

## Working with the Amazon Redshift Data API<a name="data-api-workflow"></a>

Before you use the Amazon Redshift Data API, review the following steps: 

1. Determine if you, as the caller of the Data API, are authorized\. For more information about authorization, see [Authorizing access to the Amazon Redshift Data API](data-api-access.md)\.

1. Determine if you plan to call the Data API with authentication credentials from Secrets Manager or temporary credentials\. For more information, see [Choosing database authentication credentials when calling the Amazon Redshift Data API](#data-api-calling-considerations-authentication)\.

1. Set up a secret if you use Secrets Manager for authentication credentials\. For more information, see [Storing database credentials in AWS Secrets Manager](data-api-access.md#data-api-secrets)\.

1. Review the considerations and limitations when calling the Data API\. For more information, see [Considerations when calling the Amazon Redshift Data API](#data-api-calling-considerations)\.

1. Call the Data API from the AWS Command Line Interface \(AWS CLI\), from your own code, or using the query editor in the Amazon Redshift console\. For examples of calling from the AWS CLI, see [Calling the Data API](data-api-calling.md)\.

## Considerations when calling the Amazon Redshift Data API<a name="data-api-calling-considerations"></a>

Consider the following when calling the Data API:
+ The Amazon Redshift Data API can access databases in Amazon Redshift provisioned clusters and Redshift Serverless workgroups\. For a list of AWS Regions where the Redshift Data API is available, see the endpoints listed for [Redshift Data API](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\. 
+ The maximum duration of a query is 24 hours\. 
+ The maximum number of active queries \(`STARTED` and `SUBMITTED` queries\) per Amazon Redshift cluster is 200\. 
+ The maximum query result size is 100 MB\. If a call returns more than 100 MB of response data, the call is ended\. 
+ The maximum retention time for query results is 24 hours\. 
+ The maximum query statement size is 100 KB\. 
+ The Data API is available to query single\-node and multiple\-node clusters of the following node types:
  + dc2\.large
  + dc2\.8xlarge
  + ds2\.xlarge
  + ds2\.8xlarge
  + ra3\.xlplus
  + ra3\.4xlarge
  + ra3\.16xlarge
+ The cluster must be in a virtual private cloud \(VPC\) based on the Amazon VPC service\. 
+ By default, users with the same IAM role or IAM permissions as the runner of an `ExecuteStatement` or `BatchExecuteStatement` API operation can act on the same statement with `CancelStatement`, `DescribeStatement`, `GetStatementResult`, and `ListStatements` API operations\. To act on the same SQL statement from another user, the user must be able to assume the IAM role of the user who ran the SQL statement\. For more information about how to assume a role, see [Authorizing access to the Amazon Redshift Data API](data-api-access.md)\. 
+ The SQL statements in the `Sqls` parameter of `BatchExecuteStatement` API operation are run as a single transaction\. They run serially in the order of the array\. Subsequent SQL statements don't start until the previous statement in the array completes\. If any SQL statement fails, then because they are run as one transaction, all work is rolled back\.
+ The maximum retention time for a client token used in `ExecuteStatement` or `BatchExecuteStatement` API operation is 8 hours\.

### Choosing database authentication credentials when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-authentication"></a>

When you call the Data API, you use one of the following authentication methods for some API operations\. Each method requires a different combination of parameters\. 

**AWS Secrets Manager**  
With this method, provide the `secret-arn` of a secret stored in AWS Secrets Manager which has `username` and `password`\. The specified secret contains credentials to connect to the `database` you specify\. When you are connecting to a cluster, you also supply the database name, If you provide a cluster identifier \(`dbClusterIdentifier`\), it must match the cluster identifier stored in the secret\. When you are connecting to a serverless workgroup, you also supply the database name\. 

**Temporary credentials**  
With this method, choose one of the following options:  
+ When connecting to a serverless workgroup, specify the workgroup name and database name\. The database user name is derived from the IAM identity\. For example, `arn:iam::123456789012:user:foo` has the database user name `IAM:foo`\. Also, permission to call the `redshift-serverless:GetCredentials` operation is required\.
+ When connecting to a cluster as an IAM identity, specify the cluster identifier and the database name\. The database user name is derived from the IAM identity\. For example, `arn:iam::123456789012:user:foo` has the database user name `IAM:foo`\. Also, permission to call the `redshift:GetClusterCredentialsWithIAM` operation is required\.
+ When connecting to a cluster as a database user, specify the cluster identifier, the database name, and the database user name\. Also, permission to call the `redshift:GetClusterCredentials` operation is required\.

With these methods, you can also supply a `region` value that specifies the AWS Region where your data is located\. 

### Mapping JDBC data types when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-jdbc"></a>

 The following table maps Java Database Connectivity \(JDBC\) data types to the data types you specify in Data API calls\.


****  

|  JDBC data type  |  Data API data type  | 
| --- | --- | 
|   `INTEGER, SMALLINT, BIGINT`  |  `LONG`  | 
|  `FLOAT, REAL, DOUBLE`  |  `DOUBLE`  | 
|  `DECIMAL`  |  `STRING`  | 
|  `BOOLEAN, BIT`  |  `BOOLEAN`  | 
|  `BLOB, BINARY, LONGVARBINARY, VARBINARY`  |  `BLOB`  | 
|  `VARBINARY`  |  `STRING`  | 
|  `CLOB`  |  `STRING`  | 
|  Other types \(including types related to date and time\)  |  `STRING`  | 

String values are passed to the Amazon Redshift database and implicitly converted into a database data type\.

**Note**  
Currently, the Data API doesn't support arrays of universal unique identifiers \(UUIDs\)\.

### Running SQL statements with parameters when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-parameters"></a>

You can control the SQL text submitted to the database engine by calling the Data API operation using parameters for parts of the SQL statement\. Named parameters provide a flexible way to pass in parameters without hardcoding them in the SQL text\. They help you reuse SQL text and avoid SQL injection problems\.

The following example shows the named parameters of a `parameters` field of an `execute-statement` AWS CLI command\.

```
--parameters "[{\"name\": \"id\", \"value\": \"1\"},{\"name\": \"address\", \"value\": \"Seattle\"}]"
```

Consider the following when using named parameters:
+ Named parameters can only be used to replace values in SQL statements\.
  + You can replace the values in an INSERT statement, such as `INSERT INTO mytable VALUES(:val1)`\.

    The named parameters can be in any order and parameters can be used more than one time in the SQL text\. The parameters option shown in a previous example, the values `1` and `Seattle` are inserted into the table columns `id` and `address`\. In the SQL text, you specify the named parameters as follows:

    ```
    --sql "insert into mytable values (:id, :address)"
    ```
  + You can replace the values in a conditions clause, such as `WHERE attr >= :val1`, `WHERE attr BETWEEN :val1 AND :val2`, and `HAVING COUNT(attr) > :val`\.
  + You can't replace column names in an SQL statement, such as `SELECT column-name`, `ORDER BY column-name`, or `GROUP BY column-name`\.

    For example, the following SELECT statement fails with invalid syntax\.

    ```
    --sql "SELECT :colname, FROM event" --parameters "[{\"name\": \"colname\", \"value\": \"eventname\"}]"
    ```

    If you describe \(`describe-statement` operation\) the statement with the syntax error, the `QueryString` returned does not substitute the column name for the parameter \(`"QueryString": "SELECT :colname, FROM event"`\), and an error is reported \(ERROR: syntax error at or near \\"FROM\\"\\n Position: 12\)\.
  + You can't replace column names in an aggregate function, such as `COUNT(column-name)`, `AVG(column-name)`, or `SUM(column-name)`\.
  + You can't replace column names in a JOIN clause\.
+ When the SQL runs, data is implicitly cast to a data type\. For more information about data type casting, see [Data types](https://docs.aws.amazon.com/redshift/latest/dg/c_Supported_data_types.html) in the *Amazon Redshift Database Developer Guide*\. 
+ You can't set a value to NULL\. The Data API interprets it as the literal string `NULL`\. The following example replaces `id` with the literal string `null`\. Not the SQL NULL value\. 

  ```
  --parameters "[{\"name\": \"id\", \"value\": \"null\"}]"
  ```
+ You can't set a zero length value\. The Data API SQL statement fails\. The following example trys to set `id` with a zero length value and results in a failure of the SQL statement\. 

  ```
  --parameters "[{\"name\": \"id\", \"value\": \"\"}]"
  ```
+ You can't set a table name in the SQL statement with a parameter\. The Data API follows the rule of the JDBC `PreparedStatement`\. 
+ The output of the `describe-statement` operation returns the query parameters of a SQL statement\.
+ Only the `execute-statement` operation supports SQL statements with parameters\.

## Running SQL statements with an idempotency token when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-idempotency"></a>

When you make a mutating API request, the request typically returns a result before the operation's asynchronous workflows have completed\. Operations might also time out or encounter other server issues before they complete, even though the request has already returned a result\. This could make it difficult to determine whether the request succeeded or not, and could lead to multiple retries to ensure that the operation completes successfully\. However, if the original request and the subsequent retries are successful, the operation is completed multiple times\. This means that you might update more resources than you intended\.

*Idempotency* ensures that an API request completes no more than one time\. With an idempotent request, if the original request completes successfully, any subsequent retries complete successfully without performing any further actions\. The Data API `ExecuteStatement` and `BatchExecuteStatement` operations have an optional `ClientToken` idempotent parameter\. The `ClientToken` expires after 8 hours\.

**Important**  
If you call `ExecuteStatement` and `BatchExecuteStatement` operations from an AWS SDK, it automatically generates a client token to use on retry\. In this case, we don't recommend using the `client-token` parameter with `ExecuteStatement` and `BatchExecuteStatement` operations\. View the CloudTrail log to see the `ClientToken`\. For a CloudTrail log example, see [Amazon Redshift Data API examples](logging-with-cloudtrail.md#data-api-cloudtrail)\.

The following `execute-statement` AWS CLI command illustrates the optional `client-token` parameter for idempotency\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --sql "select * from stl_query limit 1" 
    --database dev 
    --client-token b855dced-259b-444c-bc7b-d3e8e33f94g1
```

The following table shows some common responses that you might get for idempotent API requests, and provides retry recommendations\.


| Response | Recommendation | Comments | 
| --- | --- | --- | 
|  200 \(OK\)  |  Do not retry  |  The original request completed successfully\. Any subsequent retries return successfully\.  | 
|  400\-series response codes   |  Do not retry  |  There is a problem with the request, from among the following:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/data-api.html) If the request involves a resource that is in the process of changing states, retrying the request could possibly succeed\.  | 
|  500\-series response codes   |  Retry  |  The error is caused by an AWS server\-side issue and is generally transient\. Repeat the request with an appropriate backoff strategy\.  | 

For information about Amazon Redshift response codes, see [Common Errors](https://docs.aws.amazon.com/redshift/latest/APIReference/CommonErrors.html) in the *Amazon Redshift API Reference*\.