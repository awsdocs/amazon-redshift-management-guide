# Using the Amazon Redshift Data API<a name="data-api"></a>

You can access your Amazon Redshift database using the built\-in Amazon Redshift Data API\. Using this API, you can access Amazon Redshift data with web services–based applications, including AWS Lambda, Amazon SageMaker notebooks, and AWS Cloud9\. For more information on these applications, see [AWS Lambda](https://aws.amazon.com/lambda/),  [Amazon SageMaker](https://aws.amazon.com/sagemaker/), and [AWS Cloud9](https://aws.amazon.com/cloud9/)\. 

The Data API doesn't require a persistent connection to the cluster\. Instead, it provides a secure HTTP endpoint and integration with AWS SDKs\. You can use the endpoint to run SQL statements without managing connections\. Calls to the Data API are asynchronous\. 

The Data API uses either credentials stored in AWS Secrets Manager or temporary database credentials\. You don't need to pass passwords in the API calls with either authorization method\.  For more information about AWS Secrets Manager, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

For more information about the Data API operations, see the [Amazon Redshift Data API Reference](https://docs.aws.amazon.com/redshift-data/latest/APIReference/)\.

## Working with the Amazon Redshift Data API<a name="data-api-workflow"></a>

Before you use the Amazon Redshift Data API, review the following steps: 

1. Determine if you, as the caller of the Data API, are authorized\. For more information about authorization, see [Authorizing access to the Amazon Redshift Data API](data-api-access.md)\.

1. Determine if you plan to call the Data API with authentication credentials from Secrets Manager or temporary credentials\. For more information, see [Choosing authentication credentials when calling the Amazon Redshift Data API](#data-api-calling-considerations-authentication)\.

1. Set up a secret if you use Secrets Manager for authentication credentials\. For more information, see [Storing database credentials in AWS Secrets Manager](data-api-access.md#data-api-secrets)\.

1. Review the considerations and limitations when calling the Data API\. For more information, see [Considerations when calling the Amazon Redshift Data API](#data-api-calling-considerations)\.

1. Call the Data API from the AWS Command Line Interface \(AWS CLI\), from your own code, or using the query editor in the Amazon Redshift console\. For examples of calling from the AWS CLI, see [Calling the Data API](data-api-calling.md)\.

## Considerations when calling the Amazon Redshift Data API<a name="data-api-calling-considerations"></a>

Consider the following when calling the Data API:
+ For a list of AWS Regions where the Data API is available, see the endpoints listed for [Redshift Data API](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\. 
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
+ By default, users with the same IAM role or IAM user as the runner of an `ExecuteStatement` or `BatchExecuteStatement` API operation can act on the same statement with `CancelStatement`, `DescribeStatement`, `GetStatementResult`, and `ListStatements` API operations\. To act on the same SQL statement from another IAM user, the user must be able to assume the IAM role of the user who ran the SQL statement\. For more information about how to assume a role, see [Authorizing access to the Amazon Redshift Data API](data-api-access.md)\. 
+ The SQL statements in the `Sqls` parameter of `BatchExecuteStatement` API operation are run as a single transaction\. They run serially in the order of the array\. Subsequent SQL statements don't start until the previous statement in the array completes\. If any SQL statement fails, then because they are run as one transaction, all work is rolled back\.

### Choosing authentication credentials when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-authentication"></a>

When you call the Data API, you use one of the following authentication methods for some API operations\. Each method requires a different combination of parameters\. 

**AWS Secrets Manager**  
With this method, provide the `secret-arn` secret value that is stored in AWS Secrets Manager\. The specified secret contains credentials to connect to the `database` you specify\. When you are connecting to a cluster, you also supply the database name, and the cluster identifier that matches the cluster in the secret\. When you are connecting to a serverless workgroup, you also supply the database name\. 

**Temporary credentials**  
With this method, when connecting to a cluster, specify the cluster identifier, the database name, and the database user name\. Also, permission to call the `redshift:GetClusterCredentials` operation is required\. When connecting to a serverless workgroup, specify the database name\.

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

String values are passed to the Amazon Redshift database and implicitly converted into a database data type\.

**Note**  
Currently, the Data API doesn't support arrays of universal unique identifiers \(UUIDs\)\.

### Running SQL statements with parameters when calling the Amazon Redshift Data API<a name="data-api-calling-considerations-parameters"></a>

You can control the SQL text submitted to the database engine by calling the Data API operation using parameters for parts of the SQL statement\. Named parameters provide a flexible way to pass in parameters without hardcoding them in the SQL text\. They help you reuse SQL text and avoid SQL injection problems\.

The following example shows the named parameters of a `parameters` field of an `execute statement` AWS CLI command\.

```
--parameters "[{\"name\": \"id\", \"value\": \"1\"},{\"name\": \"address\", \"value\": \"Seattle\"}]"
```

Consider the following when using named parameters:
+ The named parameters can be in any order and parameters can be used more than one time in the SQL text\. The parameters option shown in the previous example, the values `1` and `Seattle` are inserted into the table columns `id` and `address`\. In the SQL text, you specify the named parameters as follows:

  ```
  --sql "insert into mytable values (:id, :address)"
  ```
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
+ The output of the `describe statement` operation returns the query parameters of a SQL statement\.
+ Only the `execute-statement` operation supports SQL statements with parameters\.