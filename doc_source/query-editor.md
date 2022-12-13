# Querying a database using the query editor<a name="query-editor"></a>

Using the query editor is an easy way to run queries on databases hosted by your Amazon Redshift cluster\. After creating your cluster, you can immediately run queries by using the query editor on the Amazon Redshift console\.

**Note**  
You can't query data in Amazon Redshift Serverless using this original query editor\. Use Amazon Redshift query editor v2 instead\.

In February 2021, an updated query editor was deployed and authorization permissions to use the query editor changed\. The new query editor uses the Amazon Redshift Data API to run queries\. The `AmazonRedshiftQueryEditor` policy, which is an AWS\-managed AWS Identity and Access Management \(IAM\) policy, was updated to include the necessary permissions\. If you have a custom IAM policy, make sure that you update it\. Use `AmazonRedshiftQueryEditor` as a guide\. The changes to `AmazonRedshiftQueryEditor` include the following: 
+ Permission to manage query editor statement results requires the statement owner user\. 
+ Permission to use Secrets Manager to connect to a database has been added\.

For more information, see [Permissions required to use the Amazon Redshift console query editor](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor)\.

When you connect to your cluster from the new query editor, you can use one of two authentication methods, as described in [Connecting with the query editor](#query-editor-connecting)\. 

Using the query editor, you can do the following:
+ Run single SQL statement queries\.
+ Download result sets as large as 100 MB to a comma\-separated value \(CSV\) file\.
+ Save queries for reuse\. You can't save queries in the Europe \(Paris\) Region, the Asia Pacific \(Osaka\) Region, the Asia Pacific \(Hong Kong\) Region, or the Middle East \(Bahrain\) Region\.
+ View query runtime details for user\-defined tables\.
+ Schedule queries to run at a future time\. 
+ View a history of queries that you created in the query editor\. 
+ Run queries against clusters using enhanced VPC routing\. 

## Query editor considerations<a name="query-editor-considerations"></a>

Consider the following about working with queries when you use the query editor:
+ The maximum duration of a query is 24 hours\. 
+ The maximum query result size is 100 MB\. If a call returns more than 100 MB of response data, the call is terminated\. 
+ The maximum retention time for query results is 24 hours\. 
+ The maximum query statement size is 100 KB\. 
+ The cluster must be in a virtual private cloud \(VPC\) based on the Amazon VPC service\. 
+ You can't use transactions in the query editor\. For more information about transactions, see [BEGIN](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html) in the *Amazon Redshift Database Developer Guide\.*
+ You can save a query up to 3,000 characters long\. 

## Enabling access to the query editor<a name="query-cluster-configure"></a>

To access the query editor, you need permission\. To enable access, attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` AWS\-managed policies for IAM permissions to the IAM user that you use to access your cluster\. You can use the IAM console \([https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\) to attach IAM policies\. 

If you have already created an IAM user to access Amazon Redshift, you can attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` AWS\-managed policies to that user\. If you haven't created an IAM user yet, create one and attach the policy to the IAM user\. 

The AWS\-managed policy `AmazonRedshiftQueryEditor` allows the action `redshift:GetClusterCredentials`, which by default gives a database user superuser access to the database\. To restrict access, you can do one of the following:
+ Create a custom policy that allows calling `redshift:GetClusterCredentials` and restricts the resource to a given value for `DbUser`\.
+ Add a policy to the user that denies permission to `redshift:GetClusterCredentials` and then requires users of the query editor to sign in with temporary credentials\. For example, a denial policy might be similar to the following policy\.

  ```
  {
    "Version": "2012-10-17",
    "Statement": {
      "Effect": "Deny",
      "Action": "redshift:GetClusterCredentials",
      "Resource": "*"
    }
  }
  ```

For more information, see [Create an IAM role or user role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\.

If you are granted access to Amazon Redshift query editor by attaching the AWS\-managed policy `AmazonRedshiftQueryEditor`, then you can list all secrets\. However, you can create and retrieve only secrets that are tagged with the key `RedshiftQueryOwner` and the value `${aws:userid}`\. If you create the key from the Amazon Redshift query editor, then the key is automatically tagged\. To use a secret that wasn't created with the Amazon Redshift query editor, confirm that the secret is tagged with the key `RedshiftQueryOwner` and a value of your unique IAM user identifier, for example `AIDACKCEVSQ6C2EXAMPLE`\. 

**To attach the required IAM policies for the query editor**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**\.

1. Choose the user that needs access to the query editor\.

1. Choose **Add permissions**\.

1. Choose **Attach existing policies directly**\.

1. For **Policy names**, choose **AmazonRedshiftQueryEditor** and **AmazonRedshiftReadOnlyAccess**\. 

1. Choose **Next: Review**\.

1. Choose **Add permissions**\.

## Connecting with the query editor<a name="query-editor-connecting"></a>

When you connect to a cluster with the query editor, you use one of the following authentication methods\. Each method requires a different combination of input from the Amazon Redshift console\. 

**AWS Secrets Manager**  
With this method, provide a secret value for **secret\-arn** that is stored in AWS Secrets Manager\. This secret contains credentials to connect to your database\. 

**Temporary credentials**  
With this method, provide your **database** and **db\-user** values\. 

### Storing database credentials in AWS Secrets Manager<a name="query-editor-secrets"></a>

When you call the query editor, you can pass credentials for the cluster by using a secret in AWS Secrets Manager\. To pass credentials in this way, you specify the name or the Amazon Resource Name \(ARN\) of the secret\. 

For more information about the minimum permissions, see [Creating and Managing Secrets with AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/managing-secrets.html) in the *AWS Secrets Manager User Guide*\. 

**To store your credentials in a secret for an Amazon Redshift cluster**

1. Use AWS Secrets Manager to create a secret that contains credentials for the cluster\. When you choose **Store a new secret**, choose **Credentials for Redshift cluster**\. Store a value for **User name** \(the database user\), **Password**, and **DB cluster ** \(cluster identifier\) in your secret\. 

   For instructions, see [Creating a Basic Secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/manage_create-basic-secret.html) in the *AWS Secrets Manager User Guide*\.

1. Use the AWS Secrets Manager console to view the details for the secret you created, or run the `aws secretsmanager describe-secret` AWS CLI command\.

## Using the query editor<a name="using-query-editor"></a>

In the following example, you use the query editor to perform the following tasks:
+ Run SQL commands\.
+ View query execution details\.
+ Save a query\.
+ Download a query result set\.

To complete the following example, you need an existing Amazon Redshift cluster\. If you don't have a cluster, create one by following the procedure described in [Creating a cluster](managing-clusters-console.md#create-cluster)\.

**To use the query editor on the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Query editor**, then connect to a database in your cluster\. 

1. For **Schema**, choose **public** to create a new table based on that schema\.

1. Enter the following in the query editor window and choose **Run** to create a new table\.

   ```
   create table shoes(
                   shoetype varchar (10),
                   color varchar(10));
   ```

1. Choose **Clear**\.

1. Enter the following command in the query editor window, and choose **Run** to add rows to the table\.

   ```
   insert into shoes values 
   ('loafers', 'brown'),
   ('sandals', 'black');
   ```

1. Choose **Clear**\.

1. Enter the following command in the query editor window and choose **Run** to query the new table\.

   ```
   select * from shoes; 
   ```

   The **Query results** displays the results\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/query-editor.html)

1. Choose **Execution** to view the run details\.

1. Choose **Data**, then **Export** to download the query results as a file\.