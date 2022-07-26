# Using the Amazon Redshift Data API<a name="data-api"></a>

You can access your Amazon Redshift database using the built\-in Amazon Redshift Data API\. Using this API, you can access Amazon Redshift data with web services–based applications, including AWS Lambda, Amazon SageMaker notebooks, and AWS Cloud9\. For more information on these applications, see [AWS Lambda](https://aws.amazon.com/lambda/),  [Amazon SageMaker](https://aws.amazon.com/sagemaker/), and [AWS Cloud9](https://aws.amazon.com/cloud9/)\. 

The Data API doesn't require a persistent connection to the cluster\. Instead, it provides a secure HTTP endpoint and integration with AWS SDKs\. You can use the endpoint to run SQL statements without managing connections\. Calls to the Data API are asynchronous\. 

The Data API uses either credentials stored in AWS Secrets Manager or temporary database credentials\. You don't need to pass passwords in the API calls with either authorization method\.  For more information about AWS Secrets Manager, see [What Is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager User Guide*\.

For more information about the Data API operations, see the [Amazon Redshift Data API Reference](https://docs.aws.amazon.com/redshift-data/latest/APIReference/)\.

## Working with the Amazon Redshift Data API<a name="data-api-workflow"></a>

Before you use the Amazon Redshift Data API, review the following steps: 

1. Determine if you, as the caller of the Data API, are authorized\. For more information about authorization, see [Authorizing access to the Amazon Redshift Data API](#data-api-access)\.

1. Determine if you plan to call the Data API with authentication credentials from Secrets Manager or temporary credentials\. For more information, see [Choosing authentication credentials when calling the Amazon Redshift Data API](#data-api-calling-considerations-authentication)\.

1. Set up a secret if you use Secrets Manager for authentication credentials\. For more information, see [Storing database credentials in AWS Secrets Manager](#data-api-secrets)\.

1. Review the considerations and limitations when calling the Data API\. For more information, see [Considerations when calling the Amazon Redshift Data API](#data-api-calling-considerations)\.

1. Call the Data API from the AWS Command Line Interface \(AWS CLI\), from your own code, or using the query editor in the Amazon Redshift console\. For examples of calling from the AWS CLI, see [Calling the Data API with the AWS CLI](#data-api-calling-cli)\.

## Authorizing access to the Amazon Redshift Data API<a name="data-api-access"></a>

To access the Data API, a user must be authorized\. You can authorize a user to access the Data API by adding a managed policy, which is a predefined AWS Identity and Access Management \(IAM\) policy, to that user\. To see the permissions allowed and denied by managed policies, see the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\)\. 

Amazon Redshift provides the `AmazonRedshiftDataFullAccess` managed policy\. This policy provides full access to Amazon Redshift Data API operations\. This policy also allows scoped access to specific Amazon Redshift, AWS Secrets Manager, and IAM API operations needed to authenticate and access an Amazon Redshift cluster\. 

You can also create your own IAM policy that allows access to specific resources\. To create your policy, use the `AmazonRedshiftDataFullAccess` policy as your starting template\. After you create your policy, add it to each user that requires access to the Data API\.

Consider the following requirements of the IAM policy associated with the IAM user:
+ If you use AWS Secrets Manager to authenticate, confirm the policy allows use of the `secretsmanager:GetSecretValue` action to retrieve the secret tagged with the key `RedshiftDataFullAccess`\.
+ If you use temporary credentials to authenticate to a cluster, confirm the policy allows the use of the `redshift:GetClusterCredentials` action to the database user name `redshift_data_api_user` for any database in the cluster\. This user name must have already been created in your database\.
+ If you use temporary credentials to authenticate to a serverless workgroup, confirm the policy allows the use of the `redshift-serverless:GetCredentials` action to retrieve the workgroup tagged with the key `RedshiftDataFullAccess`\. The database user is mapped 1:1 to the source AWS Identity and Access Management \(IAM\) identity\. For example, IAM user foo is mapped to database user `IAM:foo`, and IAM role bar is mapped to `IAMR:bar`\.  For more information about IAM identities, see [IAM Identities \(users, user groups, and roles\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) in the IAM User Guide\.

To run a query on a cluster that is owned by another account, the owning account must provide an IAM role that the Data API can assume in the calling account\. For example, suppose Account B owns a cluster that Account A needs to access\. Account B can attach the AWS\-managed policy `AmazonRedshiftDataFullAccess` to Account B's IAM role\. Then Account B trusts Account A using a trust policy such as the following:``

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": [
            "arn:aws:iam::accountID-of-account-A:role/someRoleA"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

Finally, the Account A IAM role needs to be able to assume the Account B IAM role\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "sts:AssumeRole",
    "Resource": "arn:aws:iam::accountID-of-account-B:role/someRoleB"
  }
}
```

The following links provide more information about AWS Identity and Access Management in the *IAM User Guide*\.
+ For information about creating an IAM roles, see [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html)\. 
+ For information about creating an IAM policy, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.
+ For information about adding an IAM policy to a user, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html)\. 

## Storing database credentials in AWS Secrets Manager<a name="data-api-secrets"></a>

When you call the Data API, you can pass credentials for the cluster or serverless workgroup by using a secret in AWS Secrets Manager\. To pass credentials in this way, you specify the name of the secret or the Amazon Resource Name \(ARN\) of the secret\. 

To store credentials with Secrets Manager, you need `SecretManagerReadWrite` managed policy permission\. For more information about the minimum permissions, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html) in the *AWS Secrets Manager User Guide*\. 

**To store your credentials in a secret for an Amazon Redshift cluster**

1. Use the AWS Secrets Manager console to create a secret that contains credentials for your cluster:
   + When you choose **Store a new secret**, choose **Credentials for Redshift cluster**\. 
   + Store your values for **User name** \(database user\), **Password**, and **DB cluster **\(cluster identifier\) in your secret\. 
   + Tag the secret with the key `RedshiftDataFullAccess`\. The AWS\-managed policy `AmazonRedshiftDataFullAccess` only allows the action `secretsmanager:GetSecretValue` for secrets tagged with the key `RedshiftDataFullAccess`\. 

   For instructions, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

   Note the name and ARN of the secret\. You can use these in calls to the Data API\.

**To store your credentials in a secret for a serverless workgroup**

1. Use AWS Secrets Manager AWS CLI commands to store a secret that contains credentials for your serverless workgroup:
   + Create your secret in a file, for example a JSON file named `mycreds.json`\. Provide the values for **User name** \(database user\) and **Password** in the file\.

     ```
     {
           "username": "myusername",
           "password": "mypassword"
     }
     ```
   + Store your values in your secret and tag the secret with the key `RedshiftDataFullAccess`\.

     ```
     aws secretsmanager create-secret --name MyRedshiftSecret  --tags Key="RedshiftDataFullAccess",Value="serverless" --secret-string file://mycreds.json
     ```

     The following shows the output\.

     ```
     {
         "ARN": "arn:aws:secretsmanager:region:accountId:secret:MyRedshiftSecret-mvLHxf",
         "Name": "MyRedshiftSecret",
         "VersionId": "a1603925-e8ea-4739-9ae9-e509eEXAMPLE"
     }
     ```

   For more information, see [Creating a Basic Secret with AWS CLI](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html#proc-create-api) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

   Note the name and ARN of the secret\. You can use these in calls to the Data API\.

## Creating an Amazon VPC endpoint \(AWS PrivateLink\) for the Data API<a name="data-api-vpc-endpoint"></a>

Amazon Virtual Private Cloud \(Amazon VPC\) enables you to launch AWS resources, such as Amazon Redshift clusters and applications, into a virtual private cloud \(VPC\)\. AWS PrivateLink provides private connectivity between virtual private clouds \(VPCs\) and AWS services securely on the Amazon network\. Using AWS PrivateLink, you can create VPC endpoints, which you can use connect to services across different accounts and VPCs based on Amazon VPC\. For more information about AWS PrivateLink, see [VPC Endpoint Services \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html) in the *Amazon Virtual Private Cloud User Guide*\.

You can call the Data API with Amazon VPC endpoints\. Using an Amazon VPC endpoint keeps traffic between applications in your Amazon VPC and the Data API in the AWS network, without using public IP addresses\. Amazon VPC endpoints can help you meet compliance and regulatory requirements related to limiting public internet connectivity\. For example, if you use an Amazon VPC endpoint, you can keep traffic between an application running on an Amazon EC2 instance and the Data API in the VPCs that contain them\.

After you create the Amazon VPC endpoint, you can start using it without making any code or configuration changes in your application\.

**To create an Amazon VPC endpoint for the Data API**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Choose **Endpoints**, and then choose **Create Endpoint**\.

1. On the **Create Endpoint** page, for **Service category**, choose **AWS services**\. For **Service Name**, choose **redshift\-data** \(`com.amazonaws.region.redshift-data`\)\.

1. For **VPC**, choose the VPC to create the endpoint in\.

   Choose the VPC that contains the application that makes Data API calls\.

1. For **Subnets**, choose the subnet for each Availability Zone \(AZ\) used by the AWS service that is running your application\.

   To create an Amazon VPC endpoint, specify the private IP address range in which the endpoint is accessible\. To do this, choose the subnet for each Availability Zone\. Doing so restricts the VPC endpoint to the private IP address range specific to each Availability Zone and also creates an Amazon VPC endpoint in each Availability Zone\.

1. For **Enable DNS name**, select **Enable for this endpoint**\.

   Private DNS resolves the standard Data API DNS hostname \(`https://redshift-data.region.amazonaws.com`\) to the private IP addresses associated with the DNS hostname specific to your Amazon VPC endpoint\. As a result, you can access the Data API VPC endpoint using the AWS CLI or AWS SDKs without making any code or configuration changes to update the Data API endpoint URL\.

1. For **Security group**, choose a security group to associate with the Amazon VPC endpoint\.

   Choose the security group that allows access to the AWS service that is running your application\. For example, if an Amazon EC2 instance is running your application, choose the security group that allows access to the Amazon EC2 instance\. The security group enables you to control the traffic to the Amazon VPC endpoint from resources in your VPC\.

1. Choose **Create endpoint**\.

After the endpoint is created, choose the link in the AWS Management Console to view the endpoint details\.

The endpoint **Details** tab shows the DNS hostnames that were generated while creating the Amazon VPC endpoint\.

You can use the standard endpoint \(`redshift-data.region.amazonaws.com`\) or one of the VPC\-specific endpoints to call the Data API within the Amazon VPC\. The standard Data API endpoint automatically routes to the Amazon VPC endpoint\. This routing occurs because the Private DNS hostname was enabled when the Amazon VPC endpoint was created\.

When you use an Amazon VPC endpoint in a Data API call, all traffic between your application and the Data API remains in the Amazon VPCs that contain them\. You can use an Amazon VPC endpoint for any type of Data API call\. For information about calling the Data API, see [Considerations when calling the Amazon Redshift Data API](#data-api-calling-considerations)\.

## Considerations when calling the Amazon Redshift Data API<a name="data-api-calling-considerations"></a>

Consider the following when calling the Data API:
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
+ By default, users with the same IAM role or IAM user as the runner of an `ExecuteStatement` or `BatchExecuteStatement` API operation can act on the same statement with `CancelStatement`, `DescribeStatement`, `GetStatementResult`, and `ListStatements` API operations\. To act on the same SQL statement from another IAM user, the user must be able to assume the IAM role of the user who ran the SQL statement\. For more information about how to assume a role, see [Authorizing access to the Amazon Redshift Data API](#data-api-access)\. 
+ The SQL statements in the `Sqls` parameter of `BatchExecuteStatement` API operation are run as a single transaction\. 
+ For a list of AWS Regions where the Data API is available, see [Redshift Data API Endpoints](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\. 

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
+ The output of the `describe statement` operation returns the query parameters of an SQL statement\.
+ Only the `execute-statement` operation supports SQL statements with parameters\.

## Calling the Data API<a name="data-api-calling"></a>

You can call the Data API or the AWS CLI to run SQL statements on your cluster or serverless workgroup\. The primary operations to run SQL statements are [https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html](https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_ExecuteStatement.html) and [https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_BatchExecuteStatement.html](https://docs.aws.amazon.com/redshift-data/latest/APIReference/API_BatchExecuteStatement.html)\. The Data API supports the programming languages that are supported by the AWS SDK\. For more information on these, see [Tools to Build on AWS](https://aws.amazon.com/tools/)\.

To see code examples of calling the Data API, see [Getting Started with Redshift Data API](https://github.com/aws-samples/getting-started-with-amazon-redshift-data-api#getting-started-with-redshift-data-api) in *GitHub*\. This repository has examples of using AWS Lambda to access Amazon Redshift data from Amazon EC2, AWS Glue Data Catalog, and Amazon SageMaker\. Example programing languages include Python, Go, Java, and Javascript\.

### Calling the Data API with the AWS CLI<a name="data-api-calling-cli"></a>

You can call the Data API using the AWS CLI\.

The following examples use the AWS CLI to call the Data API\. To run the examples, edit the parameter values to match your environment\. In many of the examples a `cluster-identifier` is provided to run against a cluster\. When you run against a serverless workgroup, you provide a `workgroup-name` instead\. These examples demonstrate a few of the Data API operations\. For more information, see the *AWS CLI Command Reference*\.   

Commands in the following examples have been split and formatted for readability\.

#### To run an SQL statement<a name="data-api-calling-cli-execute-statement"></a>

To run an SQL statement, use the `aws redshift-data execute-statement` AWS CLI command\.

The following AWS CLI command runs an SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

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

The following AWS CLI command runs an SQL statement against a serverless workgroup and returns an identifier to fetch the results\. This example uses the temporary credentials authentication method\.

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

#### To run an SQL statement with parameters<a name="data-api-calling-cli-execute-statement-parameters"></a>

To run an SQL statement, use the `aws redshift-data execute-statement` AWS CLI command\.

The following AWS CLI command runs an SQL statement against a cluster and returns an identifier to fetch the results\. This example uses the AWS Secrets Manager authentication method\. The SQL text has named parameters `colname` and `distance`\. In this case, the column name in the table is `ratecode` and the distance used in the predicate is `5`\. Values for named parameters for the SQL statement are specified in the `parameters` option\.

```
aws redshift-data execute-statement 
    --region us-west-2 
    --secret arn:aws:secretsmanager:us-west-2:123456789012:secret:myuser-secret-hKgPWn 
    --cluster-identifier mycluster-test 
    --sql "SELECT :colname, COUNT(*) FROM demo_table WHERE trip_distance > :distance" 
    --parameters "[{\"name\": \"colname\", \"value\": \"ratecode\"}, \ {\"name\": \"distance\", \"value\": \"5\"}]"
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

#### To run multiple SQL statements<a name="data-api-calling-cli-batch-execute-statement"></a>

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

#### To list metadata about SQL statements<a name="data-api-calling-cli-list-statements"></a>

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

#### To fetch the results of an SQL statement<a name="data-api-calling-cli-get-statement-result"></a>

To fetch the result from an SQL statement that ran, use the `redshift-data get-statement-result` AWS CLI command\. You can provide an `Id` that you receive in response to `execute-statement` or `batch-execute-statement`\. The `Id` value for an SQL statement run by `batch-execute-statement` can be retrieved in the result of `describe-statement` and is suffixed by a colon and sequence number such as `b2906c76-fa6e-4cdf-8c5f-4de1ff9b7652:2`\. If you run multiple SQL statements with `batch-execute-statement`, each SQL statement has an `Id` value as shown in `describe-statement`\.  Authorization to run this command is based on the caller's IAM permissions\. 

The following statement returns the result of an SQL statement run by `execute-statement`\.

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

#### To describe a table<a name="data-api-calling-cli-describe-table"></a>

To get metadata that describes a table, use the `aws redshift-data describe-table` AWS CLI command\.

The following AWS CLI command runs an SQL statement against a cluster and returns metadata that describes a table\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster that describes a table\. This example uses the temporary credentials authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list databases\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list databases\. This example uses the temporary credentials authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list schemas in a database\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list schemas in a database\. This example uses the temporary credentials authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list tables in a database\. This example uses the AWS Secrets Manager authentication method\.

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

The following AWS CLI command runs an SQL statement against a cluster to list tables in a database\. This example uses the temporary credentials authentication method\.

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

## Scheduling Amazon Redshift Data API operations with Amazon EventBridge<a name="data-api-calling-event-bridge"></a>

Amazon EventBridge helps you to respond to state changes in your AWS resources\. When your resources change state, they automatically send events into an event stream\. Events are sent to the account that contains the Amazon Redshift database\. You can create rules that match selected events in the stream and route them to targets to take action\. You can also use rules to take action on a predetermined schedule\. For more information, see the [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. 

To schedule Data API operations with EventBridge, the associated IAM role must trust the principal for CloudWatch Events \(events\.amazonaws\.com\)\. This role should have the equivalent of the managed policy `AmazonEventBridgeFullAccess` attached\. It should also have `AmazonRedshiftDataFullAccess` policy permissions that are managed by the Data API\. You can create an IAM role with these permissions on the IAM console\. When creating a role on the IAM console, choose the AWS service trusted entity for CloudWatch Events\. For more information about creating an IAM role, see [Creating a Role for an AWS Service \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html#roles-creatingrole-service-console) in the *IAM User Guide*\.

The following example uses the AWS CLI to create an EventBridge rule that is used to run an SQL statement\. 

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