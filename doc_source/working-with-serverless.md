# Working with Amazon Redshift Serverless \(preview\)<a name="working-with-serverless"></a>


|  | 
| --- |
| This is prerelease documentation for Amazon Redshift Serverless, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only in test environments, and not in production environments\. For preview terms and conditions, see Beta Service Participation in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

If you have any questions about this preview, contact the Amazon Redshift Serverless team by email at `redshift-preview-serverless@amazon.com`\. For service issues, contact AWS Support\.

Amazon Redshift Serverless automatically provisions data warehouse capacity and intelligently scales the underlying resources\. The serverless endpoint adjusts capacity in seconds to deliver consistently high performance and simplified operations for even the most demanding and volatile workloads\. 

With the serverless endpoint, you can benefit from the following features:
+ Access and analyze data without the need to set up, tune, and manage Amazon Redshift provisioned clusters\.
+ Use the superior Amazon Redshift SQL capabilities, industry\-leading performance, and lake house architecture to seamlessly query across a data warehouse, a data lake, and operational data sources\.
+ Deliver consistently high performance and simplified operations for even the most demanding and volatile workloads with intelligent and automatic scaling in seconds\.
+ Pay only when the data warehouse is in use\.

With the serverless endpoint, you use a console interface to reach a serverless data warehouse\. Through the data warehouse, you can access your Amazon Redshift managed storage and your Amazon S3 data lake\.

In the following sections, you can learn the basics of Amazon Redshift Serverless\. 

## Getting started with Amazon Redshift Serverless<a name="serverless-overview"></a>

To use the Amazon Redshift console, you need IAM permission as described in [Using identity\-based policies \(IAM policies\) for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html) in the *Amazon Redshift Cluster Management Guide*\. In addition, to use Amazon Redshift Serverless, attach a policy similar to the following policy to your IAM role or IAM user\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "redshift-serverless:*",
            "Resource": "*"
        }
    ]
}
```

To get started, open the AWS Management Console, choose the Amazon Redshift console, and then choose **Try Amazon Redshift Serverless**\.  

If you have the correct AWS Identity and Access Management \(IAM\) permissions, the first time you access the serverless endpoint console you view the **Get started with Amazon Redshift Serverless** page\. Your organization might be eligible for **Serverless credits** for your serverless endpoint\. For more information, see [Amazon Redshift free trial](http://aws.amazon.com/redshift/free-trial/)\.

Here is where you can **Use default settings** or **Customize settings** to create your serverless endpoint and a database\. When you customize, you see the following settings for your environment:
+ **Database name** – The name of the initial \(default\) database to create in the serverless endpoint environment\. This database is owned by your account and created in the current AWS Region\. The name is `dev` and you can't change it\.
+ **Admin user credentials** – The user name and password of the administrator of the initial database\. This user has ownership permissions for the database\. For more information, see [Identity and access management in Amazon Redshift Serverless](#serverless-iam)\.
+ **Virtual private cloud \(VPC\)** – The name of the VPC where the database is created\. 
+ **VPC security groups** – These security groups define which subnets and IP ranges can be used in the VPC\. 
+ **Subnet** – The subnets in the VPC that is associated with the specified database\. For current considerations when choosing the subnet, see [Known issues and limitations](#serverless-known-issues)\. 
+ The AWS\-owned KMS key is used by default to encrypt your data\. Instead of using the AWS\-owned KMS key, you can **Customize encryption settings** to choose a **KMS key** you manage – The AWS KMS key is used to encrypt resources in the serverless endpoint\. For information about creating KMS keys, see [Creating keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.
+ **Audit logging** – The audit log types you want to export\. For more information, see [Audit logging for Amazon Redshift Serverless](#serverless-audit-logging)\.
+ **Permissions** – The IAM role you associate with your serverless endpoint must include a trust relationship with `redshift-serverless.amazonaws.com` and `redshift.amazonaws.com`\. For more information about this IAM role, see [Permissions required to use Amazon Redshift Serverless](#serverless-endpoint-iam-role)\. 

Amazon Redshift Serverless initializes the resources for your AWS account in the current AWS Region\. The initialization process can take a few minutes to set up the environment\. The Amazon Redshift query editor v2 is opened in a new tab for you to start using your serverless endpoint\.

## Amazon Redshift Serverless console<a name="serverless-console"></a>

The Amazon Redshift Serverless console navigation menu contains the following pages and links: 
+ **Serverless dashboard** to see a summary of your resources and activity\.
+ **Query editor** link to open the Amazon Redshift query editor v2 to manage and query the data in your serverless endpoint\. The query editor v2 is an SQL client where you can run queries and create and load databases\.
+ **Serverless configuration** to update your serverless endpoint settings\.
+ **Query and database monitoring** to review and analyze your query activity\.
+ **Resource monitoring** to review your capacity and compute usage\.
+ **Datashares** to manage account level data sharing\. This page is where you can manage the datashares available to the serverless endpoint\. For more information, see [Data sharing in Amazon Redshift Serverless](#serverless-datasharing)\.
+ **Provisioned clusters dashboard** to see a summary of your provisioned clusters and open the Amazon Redshift console\.
+ **Documentation** link to open the documentation landing page\.

On the **Serverless dashboard** page, you can view a summary of your resources and graphs of your usage\. 
+ **Resource summary** – This section shows the number of databases and snapshots in your serverless endpoint\. 
+ **Query summary** – This section shows query activity for the last one hour\. 
+ **RPU capacity used** – This section shows capacity used for the last one hour\. 
+ **Datashares** – This section shows the number of datashares in this account or from another account that are available or require authorization\. 

On the **Serverless configuration** page, you can view your serverless endpoint environment settings\. This page displays information about your serverless endpoint settings such as **General information**, **Data backup**, **Data access**, and **Limits**\. 

The **General information** section displays the following: 
+ **Serverless namespace** – Is an identifier of your serverless endpoint\.
+ **Date created** – Is the date and time that your serverless endpoint was created\.
+ **Status** – Your serverless endpoint must be `Available` to query or change some configuration settings\.
+ **Change admin password** – You can change the password of the **Admin user**\. 
+ **Database name** – Is the name of the inital database created with the serverless endpoint\. This name can be used in the connection string when you connect to the serverless endpoint\. 
+ **Serverless credit remaining** – Is the amount of credit remaining for the account\.
+ **Endpoint** – Is the serverless endpoint used for some connections\.
+ **JDBC URL** – Is the connection string to connect from JDBC tools\.
+ **ODBC URL** – is the connection string to connect from ODBC tools\.

On the **Data backup** tab you can work with the following:
+ **Snapshots** – You can create, delete, and manage snapshots of your serverless endpoint data\. Currently, the retention period for snapshots is `indefinitely`\. You can authorize AWS accounts for serverless to restore from a specific snapshot\.
+ **Recovery points** – Displays the recovery points that are automatically created so you can recover from an accidental write or delete within the last 24 hours\. You can create a snapshot from a recovery point if you want to keep a point of recovery for a longer time period\. Currently, the retention period for snapshots is `indefinitely`\. 

On the **Data access** tab you can work with the following:
+ **Network and security** settings – You can view VPC\-related values, the AWS KMS encryption values, and the audit logging values\. Only audit logging values can be updated\.
+ **AWS KMS key** – The AWS KMS key used to encrypt resources in the serverless endpoint\. 
+ **Permissions** – You can manage the IAM roles that Amazon Redshift Serverless can assume to use resources on your behalf\. 
+ **Redshift\-managed VPC endpoints** – You can access your serverless endpoint from another VPC or subnet\. 

On the **Limits** tab you can work with the following:
+ **Base capacity in Redshift processing units \(RPUs\)** settings – You can set the base capacity used to process your workload\. To improve query performance, increase your RPU value\. 
+ **Usage limits** – The maximum compute resources that your serverless endpoint can use in a time period before an action is initiated\. You limit the amount of resource your serverless endpoint uses to run your workload\. Usage is measured in Redshift Processing Units \(RPUs\) seconds\. An RPU second is the number of RPUs used in one second\. You determine an action when a threshold that you set is reached, as follows: 
  + Send an alert\.
  + Log an entry to a system table\.
  + Turn off user queries\.

For more information, see [Automatic capacity scaling using base and maximum parameters](#serverless-scaling-base-max)\.

On the **Datashares** tab you can work with the following:
+ **Datashares created in my namespace** settings – You can create a datashare and share it with other namespaces and AWS accounts\. 
+ **Datashares from other namespaces and AWS accounts** – You can create a database from a datashare from other namespace and AWS accounts\. 

On the **Query and database monitoring** page, you can view graphs of your **Query history** and **Database performance**\. You can filter the data based on several dimensions\.

On the **Query history** tab you see the following graphs \(you can choose between **Query list** and **Resource metrics**\):
+ **Query runtime** – This graph shows which queries are running in the same timeframe\. Choose a bar in the graph to view more query execution details\. 
+ **Queries and loads** – This section lists queries and loads by **Query ID**\. 
+ **RPU capacity used** – This graph shows overall capacity in Redshift processing units \(RPUs\)\. 
+ **Database connections** – This graph shows the number of active database connections\. 

On the **Database performance** tab you see the following graphs:
+ **Queries completed per second** – This graph shows the average number of queries completed per second\. 
+ **Queries duration** – This graph shows the average amount of time to complete a query\. 
+ **Database connections** – This graph shows the number of active database connections\. 
+ **Running queries** – This graph shows the total number of running queries at a given time\. 
+ **Queued queries** – This graph shows the total number of queries queued at a given time\. 
+ **Query run time breakdown** – This graph shows the total time queries spent running by query type\. 

On the **Resource monitoring** page, you can view graphs of your consumed resources\. You can filter the data based on several facets\.
+ **RPU capacity used** – This graph shows the overall capacity in Redshift processing units \(RPUs\)\. 
+ **Compute usage** – This graph shows the accumulative usage of Amazon Redshift Serverless by period for the selected time range\. 

On the **Datashares** page, you can manage datashares **In my account** and **From other accounts**\. For more information about data sharing, see [Data sharing in Amazon Redshift Serverless](#serverless-datasharing)\.

## Permissions required to use Amazon Redshift Serverless<a name="serverless-endpoint-iam-role"></a>

When you use Amazon Redshift Serverless, the IAM role you associate to your serverless endpoint needs a trust relationship with both `redshift.amazonaws.com` and `redshift-serverless.amazonaws.com` to allow Amazon Redshift to assume permissions on your behalf\. 

The following example shows the policy document in JSON format to set up a trust relationship with Amazon Redshift Serverless\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "redshift-serverless.amazonaws.com",
                    "redshift.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

For more information about trust entities, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

## Overview of Amazon Redshift Serverless features<a name="serverless-considerations"></a>

Most of the features supported by an Amazon Redshift provisioned cluster are also supported on a serverless endpoint\. The following lists some of the key Amazon Redshift capabilities you can use with a serverless endpoint\. 
+ Snapshots – you can restore a snapshot of a serverless endpoint or a provisioned cluster to your serverless endpoint\. For more information, see [Working with snapshots and recovery points](#serverless-snapshots-recovery-points)\. 
+ Recovery points – Amazon Redshift Serverless automatically creates a point of recovery every 30 minutes\. These recovery points are kept for 24 hours\. You can use them to restore your serverless endpoint after accidental writes or deletes\. When you restore from a recovery point all the data in the databases of your serverless endpoint is restored to an earlier point in time\. You can also create a snapshot from a recovery point if you need to keep a point of recovery for a longer period\. For more information, see [Working with snapshots and recovery points](#serverless-snapshots-recovery-points)\.
+ Base RPU capacity – You can set a base capacity in Redshift Processing Units \(RPUs\)\. One RPU provides 2 vCPUs and 16 GiBs of memory\. Amazon Redshift uses this measurement to limit the resources \(thus cost\) used for your workload\. You can increase this value to improve query performance\. The default is 128 RPUs\.
+ Usage limits of cross\-Region data sharing – You can limit the of amount data that is transferred from a producer Region to a consumer Region\. Data transfer costs differ by AWS Region\. 
+ User\-defined functions \(UDFs\) – you can run user\-defined functions \(UDFs\) on your serverless endpoint\. For more information, see [Creating user\-defined functions](https://docs.aws.amazon.com/redshift/latest/dg/user-defined-functions.html) in the *Amazon Redshift Database Developer Guide*\.
+ Stored procedures – you can run stored procedures on your serverless endpoint\. For more information, see [Creating stored procedures](https://docs.aws.amazon.com/redshift/latest/dg/stored-procedure-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Materialized views – you can created materialized views on your serverless endpoint\. For more information, see [Creating materialized views](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Spatial functions – you can run spatial functions on your serverless endpoint\. For more information, see [Querying spatial data](https://docs.aws.amazon.com/redshift/latest/dg/geospatial-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Federated queries – you can run queries to join data with Aurora and Amazon RDS databases from with your serverless endpoint\. For more information, see [Querying data with federated queries](https://docs.aws.amazon.com/redshift/latest/dg/federated-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Data lake queries – you can run queries to join data with your Amazon S3 data lake with your serverless endpoint\.   
+ HyperLogLog – you can run HyperLogLog functions on your serverless endpoint\. For more information, see [Using HyperLogLog sketches](https://docs.aws.amazon.com/redshift/latest/dg/hyperloglog-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Querying data across databases – you can query data across databases on your serverless endpoint\. For more information, see [Querying data across databases](https://docs.aws.amazon.com/redshift/latest/dg/cross-database-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Data sharing – you can access datashares on provisioned clusters with your serverless endpoint\. For more information, see [Sharing data across clusters](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Semistructured data querying – you can ingest and store semistructured data with the `SUPER` data type on your serverless endpoint\. For more information, see [Ingesting and querying semistructured data](https://docs.aws.amazon.com/redshift/latest/dg/super-overview.html) in the *Amazon Redshift Database Developer Guide*\.
+ Amazon Redshift machine learning – you can use Amazon Redshift machine learning with your serverless endpoint\. For more information, see [Using machine learning](https://docs.aws.amazon.com/redshift/latest/dg/machine_learning.html) in the *Amazon Redshift Database Developer Guide*\.
+ SQL commands and functions – with a few exceptions \(such as `REBOOT_CLUSTER`\), you can use Amazon Redshift SQL commands and functions on your serverless endpoint\. For more information, see [SQL reference](https://docs.aws.amazon.com/redshift/latest/dg/cm_chap_SQLCommandRef.html) in the *Amazon Redshift Database Developer Guide*\.

Some features supported by an Amazon Redshift provisioned cluster are not supported by a serverless endpoint\. The following lists some of these features: 
+ Amazon Redshift Spectrum
+ Parameter groups
+ Workload management
+ Integration with an AWS Partner
+ Maintenance window and software version tracks

## Known issues and limitations<a name="serverless-known-issues"></a>

The following issues are open: 
+ After a long period of inactivity using Amazon Redshift query editor v2, running a query can show a spinning progress wheel without a query result pane appearing\. Refreshing the web browser can clear this condition\.
+ Canceling multiple queries in the query editor v2 can sometimes result in a spinning progress wheel\. Refreshing the web browser can clear this condition\.
+ A serverless endpoint is only supported in the Availability Zones with these IDs:
  + use1\-az2
  + use1\-az4
  + use1\-az6
  + use2\-az1
  + use2\-az2

  When you configure your serverless endpoint, open **Additional considerations**, and make sure that the subnet IDs provided in **Subnet** contain at least one of the supported Availability Zone IDs\. To see the subnet to Availability Zone ID mapping, go to the VPC console and choose **Subnets** to see the list of subnet IDs with their Availability Zone IDs\. Verify that your subnet is mapped to a supported Availability Zone ID\. To create a subnet, see [Create a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\. 
+ You can only have one serverless endpoint for each AWS account\.
+ Public endpoints aren't supported\.
+ There is no maintenance window with Amazon Redshift Serverless\. Software version updates are automatically applied\. Any ongoing connections are dropped at the point in time when Amazon Redshift switch versions\. Clients need to reestablish connections and Amazon Redshift Serverless works instantly\. 

## Working with Amazon Redshift query editor v2 and the serverless endpoint<a name="serverless-sql-client"></a>

You can manage and query your databases using query editor v2\. The query editor v2 is a full feature web\-based SQL client tool to connect to your Amazon Redshift data\. To get set up to use the Amazon Redshift query editor v2, including what permissions are needed, see [Configuring your AWS account](https://docs.aws.amazon.com/redshift/latest/mgmt/query-editor-v2-getting-started.html) in the *Amazon Redshift Cluster Management Guide*\.

Look for the **Query data** button to query data in your serverless endpoint with query editor v2\. When you invoke query editor v2 from the Amazon Redshift Serverless console, a new browser tab opens with the query editor\. The query editor v2 connects from your client machine to the serverless endpoint environment\. You can use all Amazon Redshift SQL functionality with Amazon Redshift Serverless including semistructured data support, data sharing, machine learning functions, queries to an Amazon S3 data lake, and federated query\. With query editor v2, you can do the following tasks: 
+ Query data in your **Serverless** and provisioned clusters\.
+ Load sample data into the **sample\_data\_dev** database\.
+ Create databases, schemas, tables, and functions\.
+ Save and share queries\.
+ Save charts\.

For information about query editor v2, see [Querying a database using the Amazon Redshift query editor v2](https://docs.aws.amazon.com/redshift/latest/mgmt/query-editor-v2.html) in the *Amazon Redshift Cluster Management Guide*\. 

## Connecting to the serverless endpoint<a name="serverless-connecting"></a>

Amazon Redshift Serverless provides a serverless endpoint for your AWS account\. If you have multiple teams or projects and want to manage costs separately, you can use separate AWS accounts\.

The serverless endpoint connects to the serverless environment in your AWS account in the current AWS Region\. The serverless endpoint runs in a VPC and is not publicly accessible\.

You can connect to a database \(named `dev`\) on the endpoint with the following syntax\.

```
account-number.aws-region.redshift-serverless.amazonaws.com:port/dev
```

For example, the following connection string specifies region us\-east\-1\.

```
123456789012.us-east-1.redshift-serverless.amazonaws.com:5439/dev
```

You can use one of the following methods to connect to your serverless endpoint with your preferred SQL client using the Amazon Redshift\-provided JDBC driver version 2 driver\.

To connect with IAM using JDBC driver version 2\.x, use the following syntax\.

```
jdbc:redshift:iam://redshift-serverless-default:aws-region/dev
```

For example, the following connection string specifies region us\-east\-1\.

```
jdbc:redshift:iam://redshift-serverless-workspace:us-east-1/dev
```

To connect with a user name and password for database authentication using JDBC driver version 2\.0 or later, use the following syntax\.

```
jdbc:redshift://account-number.aws-region.redshift-serverless.amazonaws.com:5439/dev
```

For example, the following connection string specifies account ID 123456789012 in region us\-east\-2\.

```
jdbc:redshift://123456789012.us-east-2.redshift-serverless.amazonaws.com:5439/dev
```

To connect with IAM using the JDBC driver version 2\.1\.0\.3, which is available by request during the preview period, use the following syntax\. 

```
jdbc:redshift:iam://account-number.aws-region.redshift-serverless.amazonaws.com:5439/dev
```

For example, the following connection string specifies account ID 123456789012 in region us\-east\-2\.

```
jdbc:redshift:iam://123456789012.us-east-2.redshift-serverless.amazonaws.com:5439/dev
```

For more information about drivers, see [Configuring connections](https://docs.aws.amazon.com/redshift/latest/mgmt/configuring-connections.html) in the *Amazon Redshift Cluster Management Guide*\. 

Amazon Redshift Serverless is available in the following AWS Regions:
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ US East \(Ohio\) Region \(us\-east\-2\)
+ US West \(Oregon\) Region \(us\-west\-2\)
+ Europe \(Ireland\) Region \(eu\-west\-2\)
+ Europe \(Frankfurt\) Region \(eu\-central\-1\)
+ Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)

## Connecting to the serverless endpoint with the Data API<a name="serverless-data-api"></a>

You can also use the Amazon Redshift Data API to connect to serverless endpoint\. Leave off the `cluster-identifier` parameter in your AWS CLI calls to route your query to serverless endpoint\. 

The following example runs a SQL statement to retrieve data from serverless endpoint\. This example uses the temporary credentials authentication method\.

```
aws redshift-data execute-statement --sql "select 1;" 
--database dev
```

```
{
    "CreatedAt": 1636062665.587,
    "Database": "dev",
    "Id": "ad30c9a1-be92-4534-9edf-7d9aea4ea6a3"
}
```

The following example describes a SQL statement that was submitted to serverless endpoint\.

```
aws redshift-data describe-statement --id 1d222b16-6470-467f-a1f8-7f38103dab11
```

```
{
    "CreatedAt": 1636064662.659,
    "Duration": 4358742,
    "HasResultSet": true,
    "Id": "1d222b16-6470-467f-a1f8-7f38103dab11",
    "QueryString": "select 1;",
    "RedshiftPid": 1073881246,
    "RedshiftQueryId": 0,
    "ResultRows": 1,
    "ResultSize": 11,
    "Status": "FINISHED",
    "UpdatedAt": 1636064663.324
```

The following example retrieves the results from a SQL statement that ran against serverless endpoint\.

```
aws redshift-data get-statement-result --id 1d222b16-6470-467f-a1f8-7f38103dab11
```

```
{
    "Records": [
        [
            {
                "longValue": 1
            }
        ]
    ],
    "ColumnMetadata": [
        {
            "isCaseSensitive": false,
            "isCurrency": false,
            "isSigned": true,
            "label": "?column?",
            "length": 0,
            "name": "?column?",
            "nullable": 1,
            "precision": 10,
            "scale": 0,
            "schemaName": "",
            "tableName": "",
            "typeName": "int4"
        }
    ],
    "TotalNumRows": 1
}
```

The following example runs a SQL statement to retrieve data from serverless endpoint\. This example uses the AWS Secrets Manager authentication method\.

First, create a secret in AWS Secrets Manager\.

```
aws secretsmanager create-secret --name serverless-test --secret-string '{ 
    "password": "Testing12345", 
    "engine": "redshift", 
    "host": "123456789012.us-east-1.redshift-serverless-dev.amazonaws.com", 
    "port": 5439, 
    "username": "testUser" 
  }'
```

```
{
    "ARN": "arn:aws:secretsmanager:us-east-1:123456789012:secret:serverless-test-YY3nMG",
    "Name": "serverless-test",
    "VersionId": "961a01eb-a30f-4d56-ab05-3708fd60d728"
}
```

Then, run a SQL statement using the secret for authentication\.

```
aws redshift-data execute-statement --sql "select * from sys_query_history;" 
  --database dev 
  --secret-arn arn:aws:secretsmanager:us-east-1:123456789012:secret:serverless-test-YY3nMG
```

```
{
    "CreatedAt": 1635990593.75,
    "Database": "dev",
    "Id": "56662da2-5691-4a8d-b1c1-cb73f577f08d",
    "SecretArn": "arn:aws:secretsmanager:us-east-1:123456789012:secret:serverless-test-YY3nMG"
}
```

You can also run a describe of a SQL statement to see the associated secret\.

```
aws redshift-data describe-statement --id 56662da2-5691-4a8d-b1c1-cb73f577f08d
```

```
{
    "CreatedAt": 1635990593.75,
    "Duration": 632754750,
    "HasResultSet": true,
    "Id": "56662da2-5691-4a8d-b1c1-cb73f577f08d",
    "QueryString": "select * from sys_query_history;",
    "RedshiftPid": 1073963329,
    "RedshiftQueryId": 100880,
    "ResultRows": 7,
    "ResultSize": 4170,
    "SecretArn": "arn:aws:secretsmanager:us-east-1:123456789012:secret:serverless-test-YY3nMG",
    "Status": "FINISHED",
    "UpdatedAt": 1635990595.083
}
```

For more information about the Data API, see  [Using the Amazon Redshift Data API](https://docs.aws.amazon.com/redshift//latest/mgmt/data-api.html) in the *Amazon Redshift Cluster Management Guide*\. 

## Connect with SSL to the serverless endpoint<a name="serverless-secure-ssl"></a>

### Configuring a secure connection to Amazon Redshift Serverless<a name="serverless_secure-ssl"></a>

Amazon Redshift supports Secure Sockets Layer \(SSL\) connections to encrypt queries and data\. To set up a secure connection, you can use the same configuration you use to set up a connection to a provisioned Redshift cluster\. Follow the steps in [Configuring security options for connections](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-ssl-support.html), which describes how to download and install the available SSL certificate bundle\. The bundle works for a connection to both a serverless Redshift instance and a provisioned cluster\.

## Data sharing in Amazon Redshift Serverless<a name="serverless-datasharing"></a>

Use *data sharing* to share the most up\-to\-date and consistent information as it's updated in a serverless endpoint\.

### Data sharing in Amazon Redshift Serverless<a name="serverless_datasharing"></a>

With *data sharing*, you have live access to data so that your users can see the most up\-to\-date and consistent information as it's updated in a serverless endpoint\.

#### Getting started with data sharing in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing"></a>

You can share data for read purposes across different Amazon Redshift Serverless endpoints within or across AWS accounts\.

You can get started with data sharing by using either the SQL interface or the Amazon Redshift console\. For more information, see either [Getting started data sharing using the SQL interface](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-sql.html) or [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide* in the *Amazon Redshift Database Developer Guide*\.

*Within an AWS account*, you can share data for read purposes from a provisioned cluster to a serverless endpoint, or from a serverless endpoint to a provisioned cluster\. For more information about sharing data within an AWS account using the SQL interface, see [Sharing data within an AWS account](https://docs.aws.amazon.com/redshift/latest/dg/within-account.html) in the *Amazon Redshift Database Developer Guide*\. 

*Across AWS accounts*, you can share data for read purposes from a serverless endpoint to another serverless endpoint, from a provisioned cluster to a serverless endpoint, or from a serverless endpoint to a provisioned cluster\. For more information about sharing data across AWS accounts, see [Sharing data across AWS accounts](https://docs.aws.amazon.com/redshift/latest/dg/across-account.html) in the *Amazon Redshift Database Developer Guide*\. 

To get started sharing data within an AWS account, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Serverless configuration** and then **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

To get started sharing data across AWS accounts, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

#### Data sharing considerations in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing_usage"></a>

Following are considerations for working with data sharing in Amazon Redshift Serverless:
+ When sharing data with provisioned clusters, Amazon Redshift only supports data sharing on the ra3\.16xlarge, ra3\.4xlarge, and ra3\.xlplus instance types for producer and consumer clusters\.
+ Serverless endpoints are encrypted by default\.

For a list of datasharing limitations, see [Limitations for data sharing](https://docs.aws.amazon.com/redshift/latest/dg/limitations-datashare.html) in the *Amazon Redshift Database Developer Guide*\. 

## Monitoring queries and workload with Amazon Redshift Serverless<a name="serverless-monitoring"></a>

You can monitor your serverless endpoint queries and workload with the provided system views\. 

### Monitoring views<a name="serverless_views-monitoring"></a>

*Monitoring views* are system views in Amazon Redshift Serverless that are used to monitor query and workload usage\. These views are located in the `pg_catalog` schema\. The system views available have been designed to give you the information needed to monitor the serverless endpoint, which is much simpler than needed for provisioned clusters\. The SYS system views have been designed to work with a serverless endpoint\. To display the information provided by these views, run SQL SELECT statements\.

System views are defined to support the following monitoring objectives\.

**Workload monitoring**  
You can monitor your query activities over time to:  
+ Understand workload patterns, so you know what is normal \(baseline\) and what is within business service level agreements \(SLAs\)\.
+ Rapidly identify deviation from normal, which might be a transient issue or something that warrants further action\.

**Data ingress and egress monitoring**  
Data movement in and out of a serverless endpoint is a critical function\. You use COPY and UNLOAD to ingress or egress data, and you must monitor progress closely in terms of bytes/rows transferred and files completed to track adherence to business SLAs\. This is normally done by running system table queries frequently \(that is, every minute\) to track progress and raise alerts for investigation/corrective action if significant deviations are detected\.

**Failure and problem diagnostics**  
There are cases where you must take action for query or runtime failures\. Developers rely on system tables to self\-diagnose issues and determine correct remedies\.

**Performance tuning**  
You might need to tune queries that are not meeting SLA requirements either from the start or have degraded over time\. To tune, you need to have runtime details including run plan, statistics, duration, and resource consumption\. You need baseline data for offending queries to determine the cause for deviation and to guide you on how to make improvements\.

**User objects event monitoring**  
You need to monitor actions and activities on user objects like refreshing materialized views, vacuum, and analyze\. This includes system managed events like auto\-refresh for materialized views\. You want to monitor when an event ends if it is user initiated, or the last successful execution if system initiated\.

**Usage tracking for billing**  
You can monitor your usage trends over time to:  
+ Inform budget planning and business expansion estimates\.
+ Identify potential cost\-saving opportunities like removing cold data\.

You can't query STL, STV, SVCS, SVL, and some SVV system tables and views with Amazon Redshift Serverless, except the following: 
+ [SVV\_ALL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_COLUMNS.html) 
+ [SVV\_ALL\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_SCHEMAS.html) 
+ [SVV\_ALL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_TABLES.html) 
+ [SVV\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_COLUMNS.html) 
+ [SVV\_DATASHARES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARES.html) 
+ [SVV\_DATASHARE\_CONSUMERS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARE_CONSUMERS.html) 
+ [SVV\_DATASHARE\_OBJECTS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARE_OBJECTS.html) 
+ [SVV\_EXTERNAL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_COLUMNS.html) 
+ [SVV\_EXTERNAL\_DATABASES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_DATABASES.html) 
+ [SVV\_EXTERNAL\_PARTITIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_PARTITIONS.html) 
+ [SVV\_EXTERNAL\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_SCHEMAS.html) 
+ [SVV\_EXTERNAL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_TABLES.html) 
+ [SVV\_REDSHIFT\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_COLUMNS.html) 
+ [SVV\_REDSHIFT\_DATABASES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_DATABASES.html) 
+ [SVV\_REDSHIFT\_FUNCTIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_FUNCTIONS.html) 
+ [SVV\_REDSHIFT\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_SCHEMAS.html) 
+ [SVV\_REDSHIFT\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_TABLES.html) 
+ [SVV\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TABLES.html) 
+ [SVV\_TABLE\_INFO](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TABLE_INFO.html) 
+ [SVV\_TRANSACTIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TRANSACTIONS.html) 

You can query the following SYS system views to monitor a serverless endpoint\. 

**Topics**
+ [SYS\_QUERY\_HISTORY](#SYS_QUERY_HISTORY)
+ [SYS\_QUERY\_DETAIL](#SYS_QUERY_DETAIL)
+ [SYS\_EXTERNAL\_QUERY\_DETAIL](#SYS_EXTERNAL_QUERY_DETAIL)
+ [SYS\_LOAD\_HISTORY](#SYS_LOAD_HISTORY)
+ [SYS\_LOAD\_ERROR\_DETAIL](#SYS_LOAD_ERROR_DETAIL)
+ [SYS\_UNLOAD\_HISTORY](#SYS_UNLOAD_HISTORY)
+ [SYS\_SERVERLESS\_USAGE](#SYS_SERVERLESS_USAGE)

#### SYS\_QUERY\_HISTORY<a name="SYS_QUERY_HISTORY"></a>

Use SYS\_QUERY\_HISTORY to view details of user queries\. Each row represents a user query with accumulated statistics for some of the fields\. This view contains many types of queries, such as data definition language \(DDL\), data manipulation language \(DML\), copy, unload, and Amazon Redshift Spectrum\. It contains both running and finished queries\.

SYS\_QUERY\_HISTORY is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_QUERY_HISTORY-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_QUERY\_DETAIL<a name="SYS_QUERY_DETAIL"></a>

Use SYS\_QUERY\_DETAIL to view details for queries at a step level\. Each row represents a step from a particular WLM query with details\. This view contains many types of queries such as DDL, DML, and utility commands \(for example, copy and unload\)\. Some columns might not be relevant depending on the query type\. For example, external\_scanned\_bytes is not relevant to internal tables\.

**Note**  
A query that does not have a corresponding rewritten query has an entry only in the SYS\_QUERY\_HISTORY view, not in the SYS\_QUERY\_DETAIL view\.

SYS\_QUERY\_DETAIL is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_QUERY_DETAIL-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_EXTERNAL\_QUERY\_DETAIL<a name="SYS_EXTERNAL_QUERY_DETAIL"></a>

Use SYS\_EXTERNAL\_QUERY\_DETAIL to view details for queries at a segment level\. Each row represent a segment from a particular WLM query with details like the number of rows processed, number of bytes processed, and partition info of external tables in Amazon S3\. Each row in this view will also have a corresponding entry in the SYS\_QUERY\_DETAIL view, except this view has more detail information related to external query processing\. 

SYS\_EXTERNAL\_QUERY\_DETAIL is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_EXTERNAL_QUERY_DETAIL-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_LOAD\_HISTORY<a name="SYS_LOAD_HISTORY"></a>

Use SYS\_LOAD\_HISTORY to view details of COPY commands\. Each row represents a COPY command with accumulated statistics for some of the fields\. It contains both running and finished COPY commands\.

SYS\_LOAD\_HISTORY is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_LOAD_HISTORY-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_LOAD\_ERROR\_DETAIL<a name="SYS_LOAD_ERROR_DETAIL"></a>

Use SYS\_LOAD\_ERROR\_DETAIL to view details of COPY command errors\. Each row represents a COPY command\. It contains both running and finished COPY commands\.

SYS\_LOAD\_ERROR\_DETAIL is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_LOAD_ERROR_DETAIL-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_UNLOAD\_HISTORY<a name="SYS_UNLOAD_HISTORY"></a>

Use SYS\_UNLOAD\_HISTORY to view details of UNLOAD commands\. Each row represents a UNLOAD command with accumulated statistics for some of the fields\. It contains both running and finished UNLOAD commands\.

SYS\_UNLOAD\_HISTORY is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_UNLOAD_HISTORY-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

#### SYS\_SERVERLESS\_USAGE<a name="SYS_SERVERLESS_USAGE"></a>

Use SYS\_SERVERLESS\_USAGE to view details of serverless endpoint usage of resources\. 

This view contains the serverless usage summary including how much compute capacity is used to process queries and the amount of Amazon Redshift managed storage used at a 30\-minute granularity\. The serverless endpoint compute capacity is measured in Redshift processing units \(RPUs\) and metered for the workloads you run in RPU\-seconds on a per\-second basis\. RPUs are used to process queries on the data loaded in the data warehouse, queried from an Amazon S3 data lake, or accessed from operational databases using a federated query\.

SYS\_SERVERLESS\_USAGE is visible to all users\. Superusers can see all rows; regular users can see only metadata to which they have access\.

##### Table columns<a name="SYS_SERVERLESS_USAGE-table-columns"></a>

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)

## Security and connections in Amazon Redshift Serverless<a name="serverless-security"></a>

Access to Amazon Redshift requires credentials that AWS can use to authenticate your requests\. 

### Identity and access management in Amazon Redshift Serverless<a name="serverless-iam"></a>

Access to Amazon Redshift requires credentials that AWS can use to authenticate your requests\. Those credentials must have permissions to access AWS resources, such as a serverless endpoint\. 

The following sections provide details about how you can use AWS Identity and Access Management \(IAM\) and Amazon Redshift to help secure your resources by controlling who can access them\. For more information, see [Identity and access management in Amazon Redshift](redshift-iam-authentication-access-control.md)\.

#### Granting permissions to Amazon Redshift Serverless<a name="serverless-security-other-services"></a>

To access other AWS services, Amazon Redshift Serverless requires permissions\.

##### Authorizing Amazon Redshift Serverless to access other AWS services for you<a name="serverless-security-other-services"></a>

Some Amazon Redshift features require Amazon Redshift to access other AWS services on your behalf\. For your Amazon Redshift Serverless endpoints to act for you, supply security credentials to your endpoints\. The preferred method to supply security credentials is to specify an AWS Identity and Access Management \(IAM\) role\. You can also create an IAM role through the Amazon Redshift console and set it as the default\. For more information, see [Creating an IAM role as default for Amazon Redshift](#serverless-default-iam-role)\.

To access other AWS services, create an IAM role with the appropriate permissions\. You also need to associate the role with your serverless endpoint\. In addition, either specify the Amazon Resource Name \(ARN\) of the role when you run the Amazon Redshift command or specify the `default` keyword\.

When changing the trust relationship for the IAM role in the [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/), make sure that you use `redshift.amazonaws.com` as the service name\. For information about how to manage IAM roles to access other AWS services on your behalf, see [Authorizing Amazon Redshift to access other AWS services on your behalf](authorizing-redshift-service.md)\.

##### Creating an IAM role as default for Amazon Redshift<a name="serverless-default-iam-role"></a>

When you create IAM roles through the Amazon Redshift console, Amazon Redshift programmatically creates the roles in your AWS account\. Amazon Redshift also automatically attaches existing AWS managed policies to them\. This approach means that you can stay within the Amazon Redshift console and don't have to switch to the IAM console for role creation\.

The IAM role that you create through the console for your cluster has the `AmazonRedshiftAllCommandsFullAccess` managed policy automatically attached\. This IAM role allows Amazon Redshift to copy, unload, query, and analyze data for AWS resources in your IAM account\. The related commands include COPY, UNLOAD, CREATE EXTERNAL FUNCTION, CREATE EXTERNAL TABLE, CREATE EXTERNAL SCHEMA, CREATE MODEL, and CREATE LIBRARY\. For more information about how to create an IAM role as default for Amazon Redshift, see [Creating an IAM role as default for Amazon Redshift](#serverless-default-iam-role)\.

To get started creating an IAM role as default for Amazon Redshift, open the AWS Management Console, choose the Amazon Redshift console, and then choose **Try Amazon Redshift Serverless**\. On the Amazon Redshift Serverless console, choose **Customize settings**\. Under **Permissions**, follow the procedures in [Using the console to manage IAM role associations](copy-unload-iam-role.md#managing-iam-role-association-with-cluster-console)\.

When you already have a serverless endpoint and want to configure IAM roles for your endpoint, open the AWS Management Console\. Choose the Amazon Redshift console, and then choose **Go to serverless**\. On the Amazon Redshift Serverless console, choose **Serverless configuration** and then **Data access**\. Under **Permissions**, follow the procedures in [Using the console to manage IAM role associations](copy-unload-iam-role.md#managing-iam-role-association-with-cluster-console)\.

#### Getting started with IAM credentials for Amazon Redshift<a name="serverless-iam-credentials"></a>

When you sign in to the Amazon Redshift console for the first time and first try out Amazon Redshift Serverless, you can either log on as an IAM user or an IAM role\. After you get started creating a serverless endpoint, Amazon Redshift records your IAM user name or role name that you used when you signed in\. You can use the same IAM credentials to sign in to the Amazon Redshift console and the Amazon Redshift Serverless console\.

While creating a serverless endpoint, you can create a database in your serverless endpoint\. Use the query editor v2 to connect to the database with the temporary credentials option\.

To add a new admin user name and password that persist for the database, choose **Customize admin user credentials** and enter a new admin user name and admin user password\. 

To get started using Amazon Redshift Serverless and create a serverless endpoint in the serverless endpoint console for the first time, use an IAM user or IAM role\. Make sure that this user or role has either the administrator permission ` arn:aws:iam::aws:policy/AdministratorAccess` or the full Amazon Redshift permission `arn:aws:iam::aws:policy/AmazonRedshiftFullAccess` attached to the IAM policy that you used\.

The following scenarios outline how your IAM credentials are used by Amazon Redshift Serverless when you get started on teh Amazon Redshift Serverless console:
+ If you choose **Use default settings** – Amazon Redshift Serverless translates your current IAM identity to a database superuser\. You can use the same IAM identity with the Amazon Redshift Serverless console to perform superuser actions in your database in the serverless endpoint\.
+ If you choose **Customize settings** without specifying **Admin user name** and password Amazon Redshift Serverless You current IAM credentials are used as your default admin user credentials\. This is basically the same as **Use default settings**\. 
+ If you choose **Customize settings** and specify **Admin user name** and password Amazon Redshift Serverless – Amazon Redshift Serverless translates your current IAM identity to a database superuser\. Amazon Redshift Serverless also creates another long\-term login username and password pair also as a superuser\. You can either use your current IAM identity or the created username and password pair to login in to your database as a superuser\. 

## Audit logging for Amazon Redshift Serverless<a name="serverless-audit-logging"></a>

### Exporting logs<a name="serverless_audit-logging"></a>

You can configure Amazon Redshift Serverless to export connection, user, and user\-activity log data to a log group in Amazon CloudWatch Logs\. With Amazon CloudWatch Logs, you can perform real\-time analysis of the log data and use CloudWatch to create alarms and view metrics\. You can use CloudWatch Logs to store your log records in durable storage\.

 Redshift generates connection and user log data by default\. Generation of user\-activity log data, such as data showing queries run by individual users, must be enabled explicitly\. To do this, set the configuration setting to generate user\-activity data\.  

```
aws redshift-serverless set-configuration --config-parameters parameterKey=enable_user_activity_logging,parameterValue=true
```



 To export generated log data to Amazon CloudWatch Logs, the respective logs must be selected for export in the serverless endpoint configuration settings, on the console\. 

#### Monitoring log events in CloudWatch<a name="db-auditing-manage-logs-cloudwatch-monitoring"></a>

After selecting which Redshift logs to export, you can monitor events in Amazon CloudWatch Logs\. A new log group is automatically created for Amazon Redshift Serverless under the following prefix, in which `log_type` represents the log type\.

```
/aws/redshift/serverless/<log_type>
```

For example, if you choose to export the connection log, log data is stored in the following log group\.

```
/aws/redshift/serverless/connectionlog
```

Log events are exported to a log group using the serverless log stream\. The behavior depends on which of the following conditions are true:
+ **A log group with the specified name exists\.** Redshift exports log data for the serverless endpoint using the existing log group\. To create log groups with predefined log\-retention periods, metric filters, and customer access, you can use automated configuration, such as that provided by **AWS CloudFormation**\.
+ **A log group with the specified name doesn't exist\.** When a matching log entry is detected in the log for the instance, Amazon Redshift Serverless creates a new log group in Amazon CloudWatch Logs automatically\. The log group uses the default log\-retention period of *Never Expire*\. To change the log\-retention period, use the Amazon CloudWatch Logs console, the AWS CLI, or the Amazon CloudWatch Logs API\. For more information about changing log\-retention periods in CloudWatch Logs, see *Change log data retention* in [Working with log groups and log streams](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html)\.

To search for information within log events for your serverless endpoint, use the Amazon CloudWatch Logs console, the AWS CLI, or the Amazon CloudWatch Logs API\. For more information about searching and filtering log data, see [Searching and filtering log data](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/MonitoringLogData.html)\. 

#### Amazon Redshift Serverless metrics<a name="db-auditing-manage-logs-cloudwatch-monitoring-metrics"></a>

CloudWatch metrics are the following:


**CloudWatch metrics**  

| Metric name | Units | Description | Dimension sets | 
| --- | --- | --- | --- | 
| QueriesCompletedPerSecond | Number of queries | The number of queries completed each second\. | \{DatabaseName, Latency\}, \{DatabaseName\}, \{Latency\}, \{\} | 
| QueryDuration | Microseconds | The average amount of time to complete a query\. | \{DatabaseName, Latency\}, \{DatabaseName\}, \{Latency\}, \{\} | 
| QueriesRunning | Number of queries | The number of running queries at a point in time\. | \{DatabaseName, QueryType\}, \{DatabaseName\}, \{QueryType\}, \{\} | 
| QueriesQueued | Number of queries | The number of queries in the queue at a point in time\. | \{DatabaseName, QueryType\}, \{DatabaseName\}, \{QueryType\}, \{\} | 
| DatabaseConnections | Number of connections | The number of connections to a database at a point in time\. | \{DatabaseName\}, \{\} | 
| QueryRuntimeBreakdown | Milliseconds | The total time queries ran, by query stage\. | \{DatabaseName, Stage\}, \{DatabaseName\}, \{Stage\}, \{\} | 
| DataStorage | Bytes | The number of bytes used, in storage space, for Redshift data\. | \{\} | 
| SnapshotStorage | Bytes | The number of bytes used, in storage space, for snapshots\. | \{\} | 
| ComputeCapacity | RPU | Average number of compute units allocated during the past 30 minutes, rounded up to the nearest integer\. | \{\} | 
| ComputeSeconds | RPU\-seconds | Accumulated compute\-unit seconds used in the last 30 minutes\. | \{\} | 
| CrossRegionTransferredData | Bytes | Accumulated data transferred cross\-region for cross\-region datasharing during the last 30 minutes\. | \{\} | 
| QueriesSucceeded | Number of queries | The number of queries that succeeded in the last 5 minutes\. | \{DatabaseName, QueryType\}, \{DatabaseName\}, \{QueryType\}, \{\} | 
| QueriesFailed | Number of queries | The number of queries that failed in the last 5 minutes\. | \{DatabaseName, QueryType\}, \{DatabaseName\}, \{QueryType\}, \{\} | 

Dimension sets are the grouping dimensions applied to your metrics\. You can use these dimension groups to specify how your statistics are retrieved\.

The following table details dimensions and dimension values for specific metrics:


**CloudWatch dimensions and dimension values**  

| Dimension | Description and values | 
| --- | --- | 
| DatabaseName | The name of the database\. A custom value\. | 
| latency | Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html)  | 
| QueryType | Possible values are INSERT, DELETE, UPDATE, UNLOAD, LOAD, SELECT, CTAS, and OTHER\. | 
| stage | The execution stages for a query\. Possible values are as follows: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-serverless.html) | 

## Connecting to Amazon Redshift Serverless from other VPC endpoints<a name="serverless-vpc-connect"></a>

You can connect to Amazon Redshift Serverless from other VPC endpoints, including on\-premises and public VPC endpoints\.

### Connecting from the public subnet to the Amazon Redshift Serverless endpoint using Network Load Balancer<a name="serverless-endpoint-public-access-nlb"></a>

To enable public access to Amazon Redshift Serverless endpoint, configure the Network Load Balancer in the VPC to listen to the new target group, which is configured to route the traffic to the Amazon Redshift Serverless managed VPC endpoint in the account\. For more information, see [AWS PrivateLink Guide](https://docs.aws.amazon.com/vpc/latest/privatelink/) and [User Guide for Network Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/)\.

1. Get the VPC endpoint ID and SubnetId in all of the availability zones\.

1. Get the privateIpAddress, subnetIds, and vpcId\.

1. Use AWS CloudFormation to install the redshift\-nlb\.yml template to create the Network Load Balancer and the Listener Target group\. Be sure to provide the privateIpAddress and SubnetIDs to create the stack\.

1. In the AWS CloudFormation Outputs tab, choose the CFN tab to get the Network Load Balancer DNS name\.

### Connecting to Amazon Redshift from a public subnet<a name="database-connect-from-public-subnet"></a>

You use the Network Load Balancer DNS to connect to Amazon Redshift from the public subnet\.

In PSQL, you use syntax similar to the following\.

```
psql "host=redshift-serverless-dns-name.region.amazonaws.com dbname=dev port=5439 user=admin"
```

To use your preferred query editor and the JDBC driver version 2, be sure that SSL is enabled and server certificate verification is disabled\. You use syntax similar to the following\.

```
jdbc:redshift://dns-name.region.amazonaws.com:5439/dev?useSSL=true&verifyServerCertificate=false
```

## Working with snapshots and recovery points<a name="serverless-snapshots-recovery-points"></a>

You can create and delete manual snapshots that include all databases in a serverless endpoint\. You can restore your serverless endpoint with snapshots from a serverless endpoint or provisioned cluster\. You **Manage access** to which snapshots can be used from your same or from another AWS account\. The restore operation replaces the serverless endpoint with all databases contained in the snapshot\. In addition, you can restore your data warehouse to specific recovery points in last 24 hours at 30 minute intervals\. 

When your serverless endpoint is restored, the first phase of the restore completes in a few minutes and your data is available for queries\. There is a second phase of restore where your database is tuned\. You might notice minor performance degradation during this phase\. The second phase can last from a few hours to several days and in some cases a couple of weeks\. The length of time depends on the size of data, but performance progressively improves\. At the end of this phase, your serverless endpoint is fully tuned\. 

### Snapshots<a name="serverless-snapshots"></a>

You can restore a snapshot that you created on the Amazon Redshift Serverless console to replace the data in your serverless endpoint\.  When a snapshot is restored to a serverless endpoint, the KMS key of the serverless endpoint is used to encrypt the data\.

To manage access to a snapshot

1. Start on the Amazon Redshift Serverless console, navigate to the **Serverless configuration** page, **Data backup** tab\.

1. Choose **Action**, **Manage access**\.

1. Enter an **AWS account ID** to authorize it to use the snapshot\.

To restore a snapshot from your serverless account

1. Start on the Amazon Redshift Serverless console, navigate to the **Serverless configuration** page, **Data backup** tab\.

1. Choose a snapshot to use\.

1. Choose **Action**, **Restore from snapshot**\.

To restore a snapshot from your provisioned cluster to your serverless endpoint

1. Start on the Amazon Redshift provisioned cluster console, navigate to the **Clusters**, **Snapshots** page\.

1. Choose a snapshot to use\.

1. Choose **Restore from snapshot**, **Restore to serverless endpoint**\.

1. Confirm you want to restore from your snapshot\. This action replaces all the databases in your serverless endpoint with the data from your provisioned cluster\.

For more information about snapshots on provisioned clusters, see [Amazon Redshift snapshots](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-snapshots.html) in the *Amazon Redshift Cluster Management Guide*\.

### Recovery points<a name="serverless-recovery-point"></a>

Recovery points are created for your serverless endpoint every 30 minutes and saved for 24 hours\. 

On the Amazon Redshift Serverless console, **Severless configuration**, **Backup data** tab, you can manage recovery points\. You can do the following operations:
+ Restore a serverless endpoint to a recovery point\.
+ Convert a recovery point to a snapshot\.

## Automatic capacity scaling using base and maximum parameters<a name="serverless-scaling-base-max"></a>

Amazon Redshift Serverless measures data warehouse compute capacity in Redshift Processing Units \(RPUs\)\. You pay for the workloads you run in RPU\-hours on a per\-second basis\. For storage, you pay for data stored in Amazon Redshift managed storage at a rate based on gigabytes\-per\-month\. 

### Resource provisioning and scaling in Amazon Redshift Serverless<a name="serverless-resource-provisioning"></a>

Optionally, you can use Base and Max settings to control performance and costs\.

Base  
Represents the base data warehouse capacity that Amazon Redshift Serverless uses to process queries\. Base capacity is specified in Redshift Processing Units \(RPUs\)\. Setting a higher base compute capacity can improve query performance, especially for data processing and ETL jobs that process large amounts of data\. Base defaults to AUTO, enabling the serverless endpoint to assign and adjust the proper base data warehouse capacity automatically\. You can adjust the Base from 32 RPUs to 512 RPUs in units of 8 \(32,40,48\.\.\.512\) through the serverless endpoint management console or by invoking an Amazon Redshift API\.

Max  
Specifies usage limits\. Setting a higher Max compute capacity can improve the overall throughput of the system, benefiting workloads with high concurrency while maintaining high performance\.   
For storage, you pay for data stored in Amazon Redshift managed storage and storage used for user snapshots at the fixed GB\-month rate similar to Amazon Redshift provisioned clusters\. With Amazon Redshift Serverless, you can restore your data warehouse to specific points in the last 24 hours at a 30 minute granularity without additional cost\. Data transfer costs and machine learning costs apply at the same rate as provisioned clusters\. 

## Billing for Amazon Redshift Serverless<a name="serverless-billing"></a>

### How billing works<a name="serverless_billing"></a>

Amazon Redshift Serverless is billed according to the time that your compute resources spend running queries\. The compute power of a serverless endpoint is measured in Redshift Processing Units, or RPUs\. An RPU refers to a baseline unit of resources, measured in virtual CPUs and memory\. A serverless endpoint with more resources has a higher number of RPUs\. Billing accrues at a rate of RPUs per second\.

You aren't billed for compute capacity, but rather for time spent processing queries\. This means that you aren't charged for idle capacity\. Storage is charged for, separately, at a rate of GB per month\.

To control your cost, you can set a maximum spend rate on your serverless endpoint\. This is a processing capacity limit that provides a cost ceiling\. You can adjust it at any time without an interruption in query processing\.

#### How Amazon Redshift Serverless billing differs from provisioned clusters<a name="serverless-billing-provisioned"></a>

When you use a provisioned Redshift cluster, you're billed according to the number of nodes in the cluster and the type of the nodes\. With Amazon Redshift Serverless, you don't manage cluster size or node type\. The serverless endpoint adjusts to load automatically\. You also don't have to pause your endpoint when it's inactive\. Amazon Redshift Serverless handles idle time automatically\. 

#### Additional billing details<a name="db-serverless-billing-details"></a>
+ Billing includes concurrency\-scaling capacity for periods of heavy workloads\. Concurrency scaling is included in the billing charge\.
+ System queries for management and monitoring aren't charged for\. Additionally, automatic optimizations like automatic sort and vacuum are not charged for\.
+ For Redshift Spectrum queries, you are billed at the same rate as for a query on local data\.
+ Federated queries are charged in terms of RPUs used over a specific time interval, the same as queries on the serverless endpoint\.
+ Storage is billed for separately, at the given rate per GB\.
+ The minimum billing interval is one second\. If you run a query for 100 milliseconds, for example, you are charged for a second\.
+ Snapshot billing doesn't change\. It's charged at a rate of GB per month\.
+ If you run a query and cancel it before it finishes, the query time is billed\.