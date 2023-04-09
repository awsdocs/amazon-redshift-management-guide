# Working with datashares<a name="query-editor-v2-datashare-using"></a>

You can create a datashare so that users on another cluster can query the data\. The cluster containing the data that you want to share is called the *producer* cluster\. You create a datashare on the producer cluster for the database objects that you want to share\. You can share schemas, tables, views, and SQL user\-defined functions \(UDFs\)\. The cluster that you want to share the data to is called the *consumer* cluster\. On the consumer cluster, you create a database from the datashare\. Then, users on the consumer cluster can query the data\. For more information, see [Getting started data sharing](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare.html) in the *Amazon Redshift Database Developer Guide*\.

## Creating datashares<a name="query-editor-v2-create-datashare"></a>

You create a datashare on the cluster that you want to use as the producer cluster\. To learn more about datashare considerations, see [Data sharing considerations in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/considerations.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Choose the database on the producer cluster that you want to use\.

1. Create the datashare\. For example:

   ```
   create datashare mysource;
   ```

1. Set permissions on the datashare\. For example:

   ```
   grant alter, share on datashare mysource to admin;
   ```

1. Set permissions on the database objects that you want to share\. For example:

   ```
   alter datashare mysource add schema public;
   ```

   ```
   alter datashare mysource add table public.event;
   ```

1. Set permissions on the consumer cluster namespace to access the datashare\. For example:

   ```
   grant usage on datashare mysource to namespace '2b12345-1234-5678-9012-bb1234567890';
   ```

## Showing datashares<a name="query-editor-v2-show-datashare"></a>

You can show the datashares that you've created on the producer cluster\. 

1. Choose the producer cluster\.

1. Show the datashares\. For example:

   ```
   show datashares;
   ```

   ```
   share_name	share_owner	source_database		consumer_database	share_type	createdate	is_publicaccessible	share_acl	producer_account	producer_namespace
   test_datashare	100		db_producer		NULL			OUTBOUND	2/15/2022		FALSE		admin		123456789012		p1234567-8765-4321-p10987654321
   ```

## Creating the consumer database<a name="query-editor-v2-datashare-consumer"></a>

On the consumer cluster, you create a database from the datashare\. These steps describe how to share data between two clusters in the same account\. For information on sharing data across AWS accounts, see [Sharing data across AWS accounts](https://docs.aws.amazon.com/redshift/latest/dg/across-account.html) in the *Amazon Redshift Database Developer Guide*\.

You can use SQL commands or the query editor v2 tree\-view panel to create the database\.

**To use SQL**

1. Create a database from the datashare for your account and the namespace of the producer cluster\. For example:

   ```
   create database share_db from datashare mysource of account '123456789012' namespace 'p1234567-8765-4321-p10987654321'; 
   ```

1. Set permissions so that users can access the database and the schema\. For example:

   ```
   grant usage on database share_db to usernames;
   ```

   ```
   grant usage on schema public to usernames;
   ```

**To use the query editor v2 tree\-view panel**

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png)**Create**, and then choose **Database**\.

1. Enter a **Database name**\.

1. \(Optional\) Select **Users and groups**, and choose a **Database user**\.

1. Choose **Create using a datashare**\.

1. Choose the datashare\.

1. Choose **Create database**\.

   The new ![\[datashare\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-datashare.png)**datashare** database displays in the query editor v2 tree\-view panel\.

1. Set permissions so that users can access the database and the schema\. For example:

   ```
   grant usage on database share_db to usernames;
   ```

   ```
   grant usage on schema public to usernames;
   ```

## Querying datashare objects<a name="query-editor-v2-query-datashare"></a>

On the consumer cluster, you can query datashare objects using fully qualified object names expressed with the three\-part notation: database name, schema, and name of the object\. 

1. In the query editor v2 tree\-view panel, choose the schema\.

1. To view a table definition, choose a table\.

   The table columns and data types display\.

1. To query a table, choose the table and use the context menu \(right\-click\) to choose **Select table**\.

1. Query tables using SELECT commands\. For example:

   ```
   select top 10 * from test_db.public.event;
   ```