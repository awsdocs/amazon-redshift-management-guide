# Using a federated identity to manage Amazon Redshift access to local resources and Amazon Redshift Spectrum external tables<a name="authorization-fas-spectrum"></a>

Using identity federation in AWS with credentials provided from `GetDatabaseCredentials` can simplify authorization and access to local data and to external data\. Currently, to give users access to external data that resides in Amazon S3, you create an IAM role with permissions defined in a permissions policy\. Then, users with the role attached can access the external data\. This works, but if you want to provide granular rules, such as making specific columns unavailable for a particular user, you may have to do additional configuration on the external schema\. In this topic, we show how to provide access to resources with AWS identity federation, instead of using a specific IAM role\. Identity federation, with credentials provided from `GetDatabaseCredentials`, can provide access to AWS Glue and Redshift Spectrum resources via granular IAM rules that are easier to specify and change\. This makes it easier to apply access that conforms to your business rules\.

The benefits of using federated credentials are the following: 
+ You don't have to manage cluster\-attached IAM roles for Redshift Spectrum\.
+ Cluster administrators can create an external schema that's accessible by consumers with different IAM contexts\. This is useful, for example, to perform column filtering on a table, where different consumers query the same external schema and get varying fields in returned records\.
+ You can query Amazon Redshift using a user with IAM permissions, rather than only with a role\.

## Preparing an identity to log in with federated identity<a name="authorization-fas-spectrum-getting-started-iam"></a>

Before logging in with federated identity, you must perform several preliminary steps\. These instructions assume you have an existing Redshift Spectrum external schema that references a data file stored in an Amazon S3 bucket, and the bucket is in the same account as your Amazon Redshift cluster or Amazon Redshift Serverless data warehouse\. 

1. Create an IAM identity\. This can be a user or an IAM role\. Use any name supported by IAM\.

1. Attach permissions policies to the identity\. Specify either of the following:
   + `redshift:GetClusterCredentialsWithIAM` \(for an Amazon Redshift provisioned cluster\)
   + `redshift-serverless:GetCredentials` \(for Amazon Redshift Serverless\)

   You can add permissions with the policy editor, using the IAM console\.

   The IAM identity also needs permissions to access external data\. Grant access to Amazon S3 by adding the following AWS managed policies directly:
   + `AmazonS3ReadOnlyAccess`
   + `AWSGlueConsoleFullAccess`

    The last managed policy is required if you're using AWS Glue to prepare your external data\. For more information about the steps for granting access to Amazon Redshift Spectrum, see [Create an IAM role for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/c-getting-started-using-spectrum-create-role.html), which is part of the getting\-started guide for Amazon Redshift and Redshift Spectrum\. It shows the steps for adding IAM policies to access Redshift Spectrum\. 

1. Set up your SQL client to connect to Amazon Redshift\. Use the Amazon Redshift JDBC driver, and add your user's credentials to the tool's credential properties\. A client like SQL Workbench/J works well for this\. Set the following client\-connection extended properties:
   + *AccessKeyID* – Your access key identifier\.
   + *SecretAccessKey* – Your secret access key\. \(Note the security risk of transmitting the secret key if you don't use encryption\.\) 
   + *SessionToken* – A set of temporary credentials for an IAM role\.
   + *groupFederation* – Set to `true` if you're configuring federated identity for a provisioned cluster\. Don't set this parameter if you are using Amazon Redshift Serverless\. 
   + *LogLevel* – Integer log\-level value\. This is optional\.

1. Set the URL to the JDBC endpoint found in the Amazon Redshift or Amazon Redshift Serverless console\. Replace your URL schema with *jdbc:redshift:iam:* and use this formatting:
   + Format for an Amazon Redshift provisioned cluster: `jdbc:redshift:iam://<cluster_id>.<unique_suffix>.<region>.redshift.amazonaws.com:<port>/<database_name>`

     Example: `jdbc:redshift:iam://test1.12345abcdefg.us-east-1.redshift.amazonaws.com:5439/dev`
   + Format for Amazon Redshift Serverless: `jdbc:redshift:iam://<workgroup-name>.<account-number>.<aws-region>.redshift-serverless.amazonaws.com:5439:<port>/<database_name>`

     Example: `jdbc:redshift:iam://default.123456789012.us-east-1.redshift-serverless.amazonaws.com:5439/dev`

   After you connect to the database for the first time, using an IAM identity, Amazon Redshift automatically creates an Amazon Redshift identity with the same name, prefixed with `IAM:` for a user or `IAMR:` for an IAM role\. The remaining steps in this topic show examples for a user\.

   If a Redshift user isn't automatically created, you can create one by running a `CREATE USER` statement, using an admin account, specifying the user name in the format `IAM:<user name>`\.

1.  As your Amazon Redshift cluster administrator, grant the Redshift user the required permissions to access the external schema\.

   ```
   GRANT ALL ON SCHEMA my_schema to "IAM:my_user";
   ```

   To grant the ability to your Redshift user to create tables in the external schema, they must be a schema owner\. For example:

   ```
   ALTER SCHEMA my_schema owner to "IAM:my_user";
   ```

1. To verify the configuration, run a query as the user, using the SQL client, after permissions are granted\. This query sample retrieves data from an external table\. 

   ```
   SELECT * FROM my_schema.my_table;
   ```

## Getting started with identity and authorization propagation to Redshift Spectrum<a name="authorization-fas-spectrum-getting-started"></a>

To pass a federated identity to query external tables, you set `SESSION` as the value for the `IAM_ROLE` query parameter of `CREATE EXTERNAL SCHEMA`\. The following steps show how to set up and leverage `SESSION` to authorize queries on the external schema\.

1. Create local tables and external tables\. External tables catalogued with AWS Glue work for this\. 

1. Connect to Amazon Redshift with your IAM identity\. As mentioned in the previous section, when the identity connects to Amazon Redshift, a Redshift database user is created\. The user is created if they didn't previously exist\. If the user is new, the administrator must grant them permissions to perform tasks in Amazon Redshift, like querying and creating tables\. 

1. Connect to Redshift with your admin account\. Run the command to create an external schema, using the `SESSION` value\. 

   ```
   create external schema spectrum_schema from data catalog
   database '<my_external_database>' 
   region '<my_region>'
   iam_role 'SESSION'
   catalog_id '<my_catalog_id>';
   ```

   Note that `catalog_id` is set in this case\. This is a new setting added with the feature, because `SESSION` replaces a specific role\.

   In this example, values in the query mimic how real values appear\.

   ```
   create external schema spectrum_schema from data catalog
   database 'spectrum_db' 
   region 'us-east-1'
   iam_role 'SESSION'
   catalog_id '123456789012'
   ```

   The `catalog_id` value in this case is your AWS account ID\.

1. Run queries to access your external data, using the IAM identity you connected with in step 2\. For example:

   ```
   select * from spectrum_schema.table1;
   ```

   In this case, `table1` can be, for example, JSON\-formatted data in a file, in an Amazon S3 bucket\.

1. If you already have an external schema that uses a cluster\-attached IAM role, pointing to your external database or schema, you can either replace the existing schema and use a federated identity as detailed in these steps, or create a new one\.

`SESSION` indicates that federated identity credentials are used to query the external schema\. When you use the `SESSION` query parameter, make sure you set the `catalog_id`\. It's required because it points to the data catalog used for the schema\. Previously, `catalog_id` was retrieved from the value assigned to `iam_role`\. When you set up identity and authorization propagation this way, for instance, to Redshift Spectrum, by using federated credentials to query an external schema, authorization by means of an IAM role isn’t required\. 

### Additional resources<a name="authorization-fas-spectrum-resources"></a>

These links provide additional information for managing access to external data\.
+ You can still access Redshift Spectrum data using an IAM role\. For more information, see [Authorizing Amazon Redshift to access other AWS services on your behalf](authorizing-redshift-service.md)\.
+ When you manage access to external tables with AWS Lake Formation, you can query them using Redshift Spectrum with federated IAM identities\. You no longer have to manage cluster\-attached IAM roles for Redshift Spectrum to query data registered with AWS Lake Formation\. For more information, see [Using AWS Lake Formation with Amazon Redshift Spectrum](https://docs.aws.amazon.com/lake-formation/latest/dg/RSPC-lf.html)\.