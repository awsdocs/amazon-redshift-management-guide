# Working with query editor v2<a name="query-editor-v2-using"></a>

The query editor v2 is primarily used to edit and run queries, visualize results, and share your work with your team\. With query editor v2, you can create databases, schemas, tables, and user\-defined functions \(UDFs\)\. In a tree\-view panel, for each of your databases, you can view its schemas\. For each schema, you can view its tables, views, UDFs, and stored procedures\.

**Topics**
+ [Opening query editor v2](#query-editor-v2-open)
+ [Connecting to an Amazon Redshift database](#query-editor-v2-connecting)
+ [Browsing an Amazon Redshift database](#query-editor-v2-object-browse)
+ [Creating database objects](#query-editor-v2-object-create)
+ [Loading sample data](#query-editor-v2-loading-sample-data)
+ [Loading data from Amazon S3](#query-editor-v2-loading-data)
+ [Working with SQL notebooks \(preview\)](#query-editor-v2-notebooks)

## Opening query editor v2<a name="query-editor-v2-open"></a>

**To open the query editor v2**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. From the navigator menu, choose **Editor**, then **Query editor V2**\.

The query editor v2 opens in a new tab\.

The query editor page has a navigator menu where you choose **Database** to work with data in your cluster or workgroup, **Queries** to work with saved queries, and **Charts** to work with saved charts\. The navigator menu is similar to the following\.

![\[Navigator icons\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/sqlworkbench-navigator-menu.png)

When working in the **Database** view, you have the following controls: 
+ The **Cluster** or **Workgroup** field displays the name of the cluster or workgroup you are currently connected to\. The **Database** field displays the databases within the cluster or workgroup\. The actions that you perform in the **Database** view default to act on the database you have selected\. 
+ A tree\-view hierarchical view of your clusters or workgroups, databases, and schemas\. Under schemas, you can work with your tables, views, functions, and stored procedures\. Each object in the tree view supports a context menu to perform associated actions, such as **Refresh** or **Drop**, for the object\. 
+ A ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Create** action to create databases, schemas, tables, and functions\.
+ A ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/download.png) **Load data** action to load data from Amazon S3 into your databases\.
+ A ![\[Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/settings.png) preferences icon to edit your preferences\.
+ A ![\[Save\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/save-icon.png) **Save** icon to save your query\. 
+ A ![\[Shortcuts\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/shortcuts-icon.png) **Shortcuts** icon to display keyboard shortcuts for the editor\. 
+ An editor area where you can enter and run your query\. 
+ After you run a query, a **Result** tab appears with the results\. Here is where you can turn on **Chart** to visualize your results\.

## Connecting to an Amazon Redshift database<a name="query-editor-v2-connecting"></a>

To connect to a database, choose the cluster or workgroup name in the tree\-view panel\. If prompted, enter the connection parameters\.

When you connect to a cluster or workgroup and its databases, you provide a  **Database** name\.  You also provide parameters required for one of the following authentication methods:

**Federated user**  
With this method, the principal tags of your IAM role or IAM user must provide the connection details\. You configure these tags in AWS Identity and Access Management or your identity provider \(IdP\)\. The query editor v2 relies on the following tags\.  
+ `RedshiftDbUser` – This tag defines the database user that is used by query editor v2\. This tag is required\.
+ `RedshiftDbGroups` – This tag defines the database groups that are joined when connecting to query editor v2\. This tag is optional and its value must be a colon\-separated list such as `group1:group2:group3`\. Empty values are ignored, that is, `group1::::group2` is interpreted as `group1:group2`\. 
These tags are forwarded to the `redshift:GetClusterCredentials `  API to get credentials for your cluster\. For more information, see [Setting up principal tags to connect to query editor v2 as a federated user](query-editor-v2-getting-started.md#query-editor-v2-principal-tags-iam)\.

**Database user name and password**  
With this method, also provide a **User name** and **Password** for the database that you are connecting to\. The query editor v2 creates a secret on your behalf stored in AWS Secrets Manager\. This secret contains credentials to connect to your database\. 

**Temporary credentials**  
With this method, query editor v2, provide a **User name** for the database\. query editor v2 generates a temporary password to connect to the database\. 

When you select a cluster or workgroup with query editor v2, depending on the context, you can create, edit, and delete connections using the context \(right\-click\) menu\.

## Browsing an Amazon Redshift database<a name="query-editor-v2-object-browse"></a>

Within a database, you can manage schemas, tables, views, functions, and stored procedures in the tree\-view panel\. Each object in the view has actions associated with it in a context \(right\-click\) menu\.

After you choose a table, you can do the following:
+ To start a query in the editor with a SELECT statement that queries all columns in the table, use **Select table**\.
+ To see the attributes or a table, use **Show table definition**\. Use this to see column names, column types, encoding, distribution keys, sort keys, and whether a column can contain null values\. For more information about table attributes, see [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) in the *Amazon Redshift Database Developer Guide*\.
+ To delete a table, use **Delete**\. You can either use **Truncate table** to delete all rows from the table or **Drop table** to remove the table from the database\. For more information, see [TRUNCATE](https://docs.aws.amazon.com/redshift/latest/dg/r_TRUNCATE.html) and [DROP TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_DROP_TABLE.html) in the *Amazon Redshift Database Developer Guide*\. 

Choose a schema to **Refresh** or **Drop schema**\. 

Choose a view to **Show view definition** or **Drop view**\. 

Choose a function to **Show function definition** or **Drop function**\. 

Choose a stored procedure to **Show procedure definition** or **Drop procedure**\. 

The hierarchical tree\-view panel is similar to the following\. Open the context \(right\-click\) menu for an icon to see what actions you can perform\.

![\[Tree-view icons\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/sqlworkbench-tree-view.png)

## Creating database objects<a name="query-editor-v2-object-create"></a>

You can create database objects, including databases, schemas, tables, and user\-defined functions \(UDFs\)\. You must be connected to a cluster or workgroup and a database to create database objects\.

**To create a database**

For information about databases, see [CREATE DATABASE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_DATABASE.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-editor-new-tab.png)**Create**, and then choose **Database**\.

1. Enter a **Database name**\.

1. \(Optional\) Select **Users and groups**, and choose a **Database user**\.

1. Choose **Create database**\.

   The new database displays in the tree\-view panel\.

**To create a schema**

For information about schemas, see [Schemas](https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-editor-new-tab.png)**Create**, and then choose **Schema**\.

1. Enter a **Schema name**\.

1. \(Optional\) Select **Authorize user**, and choose a **Database user**\.

1. Choose **Create schema**\.

   The new schema appears in the tree\-view panel\.

**To create a table**

You can create a table based on a comma\-separated value \(CSV\) file that you specify or define each column of the table\. For information about tables, see [Designing tables](https://docs.aws.amazon.com/redshift/latest/dg/c_designing-tables-best-practices.html) and [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) in the *Amazon Redshift Database Developer Guide*\. 

Choose **Open query in editor** to view and edit the CREATE TABLE statement before you run the query to create the table\. 

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png)**Create** and select **Table**\.

1. Choose a schema\.

1. Enter a table name\.

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Add field** to add a column\. 

1. Use a CSV file as a template for the table definition:

   1. Choose **Load from CSV**\.

   1. Browse to the file location\.

      If you use a CSV file, be sure that the first row of the file contains the column headings\.

   1. Choose the file and choose **Open**\. Confirm that the column names and data types are what you intend\.

1. For each column, choose the column and choose the options that you want:
   + Choose a value for **Encoding**\.
   + Choose a **Default value**\.
   + Turn on **Automatically increment** if you want the column values to increment\. Then specify a value for **Auto increment seed** and **Auto increment step**\.
   + Turn on **Not NULL** if the column should always contain a value\.
   + Enter a **Size** value for the column\.
   + Turn on **Primary key** if you want the column to be a primary key\.
   + Turn on **Unique key** if you want the column to be a unique key\.

1. \(Optional\) Choose **Table details** and choose any of the following options:
   + Distribution key column and style\.
   + Sort key column and sort type\.
   + Turn on **Backup** to include the table in snapshots\.
   + Turn on **Temporary table** to create the table as a temporary table\.

1. Choose **Open query in editor** to continue specifying options to define the table or choose **Create table** to create the table\.

**To create a function**

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png)**Create**, and choose **Function**\.

1. For **Type**, choose **SQL** or **Python**\.

1. Choose a value for **Schema**\.

1. Enter a value for **Name** for the function\.

1. Enter a value for **Volatility** for the function\.

1. Choose **Parameters** by their data types in the order of the input parameters\.

1. For **Returns**, choose a data type\.

1. Enter the **SQL program** code for the function\.

1. Choose **Create**\.

For more information about user\-defined functions \(UDFs\), see [Creating user\-defined functions](https://docs.aws.amazon.com/redshift/latest/dg/user-defined-functions.html) in the *Amazon Redshift Database Developer Guide*\. 

## Loading sample data<a name="query-editor-v2-loading-sample-data"></a>

The query editor v2 comes with sample data and queries available to be loaded into a sample database and corresponding schema\. 

To load sample data, choose the ![\[External\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/external.png) icon associated with the sample data you want to load\. The query editor v2 then loads the data into a schema in database `sample_data_dev` and creates a folder of saved queries in your **Queries** folder\. 

The following sample datasets are available\.

**tickit**  
Most of the examples in the Amazon Redshift documentation use a sample data called `tickit`\. This data consists of seven tables: two fact tables and five dimensions\. When you load this data the schema `tickit` is updated with sample data\. For more information about the `tickit` data, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

**tpch**  
This data is used for a decision support benchmark\. When you load this data the schema `tpch` is updated with sample data\. For more information about the `tpch` data, see [TPC\-H](http://www.tpc.org/tpch/)\. 

**tpcds**  
This data is used for a decision support benchmark\. When you load this data the schema `tpcds` is updated with sample data\. For more information about the `tpcds` data, see [TPC\-DS](http://www.tpc.org/tpcds/)\. 

## Loading data from Amazon S3<a name="query-editor-v2-loading-data"></a>

You can load data into an existing table from Amazon S3\.

**To load data into an existing table**

The COPY command is used by query editor v2 to load data from Amazon S3\. The COPY command generated and used in the query editor v2 Load data wizard supports all the parameters available to the COPY command syntax to copy from Amazon S3\. For information about the COPY command and its options used to load data from Amazon S3, see [COPY from Amazon Simple Storage Service](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-source-s3.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Confirm that the table is already created in the database where you want to load data\. The query editor v2 can only load data into an existing table\.

1. Choose ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/download.png)**Load data**\.

1. In **S3 URIs**, choose **Browse S3** to look for the Amazon S3 bucket that contains the data to load\. 

1. If the specified Amazon S3 bucket isn't in the same AWS Region with the target table, then choose the **S3 file location** for the AWS Region where the data is located\.

1. Choose **This file is a manifest file** if the Amazon S3 file is actually a manifest containing multiple Amazon S3 bucket URIs\.

1. Choose an **IAM role** that has the required permissions to load data from Amazon S3\.

1. Choose the **File format** for the file to be uploaded\. The supported data formats are CSV, JSON, DELIMITER, FIXEDWIDTH, SHAPEFILE, AVRO, PARQUET, and ORC\. Depending on the specified file format, you can choose the respective **File options**\. You can also select **Data is encrypted** if the data is encrypted and enter the Amazon Resource Name \(ARN\) of the KMS key used to encrypt the data\.

   For PARQUET and ORC, there isn't any file option to configure\.

1. Choose a compression method to compress your file\. The default is no compression\.

1. \(Optional\) The **Advanced settings** support various **Data conversion parameters** and **Load operations**\. Enter this information as needed for your file\.

   For more information about data conversion and data load parameters, see [Data conversion parameters](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html) and [Data load operations](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-load.html) in the *Amazon Redshift Database Developer Guide*\.

1. Confirm or choose the location of the **Target table** including database, schema, and table name where the data is loaded\.

1. Choose **Load data** to start the data load\.

   When the load completes, the query editor displays with the generated COPY command that was used to load your data\. The **Result** of the COPY is shown\. If successful, you can now use SQL to select data from the loaded table\. When there is an error, query the system view STL\_LOAD\_ERRORS to get more details\. For information about COPY command errors, see [STL\_LOAD\_ERRORS](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_LOAD_ERRORS.html) in the *Amazon Redshift Database Developer Guide*\.

## Working with SQL notebooks \(preview\)<a name="query-editor-v2-notebooks"></a>


|  | 
| --- |
| This is prerelease documentation for query editor v2 notebooks, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only in test environments, and not in production environments\. For preview terms and conditions, see Beta Service Participation in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

You can use SQL notebooks to organize, annotate, and share multiple SQL queries in a single document\. You can add multiple SQL query and Markdown cells to an SQL notebook\. SQL notebooks provide a way to group queries and explanations associated with a data analysis in a single document by using multiple query and Markdown cells\. You can add text and format the appearance using Markdown syntax to provide context and additional information for your data analysis tasks\. You can share your SQL notebooks with team members\.

To use the SQL notebook feature, you must add a policy for the SQL notebook \(preview\) feature to a principal \(an IAM user or IAM role\) that already has one of the query editor v2 managed policies\. For more information, see [Accessing the query editor v2](query-editor-v2-getting-started.md#query-editor-v2-configure)\.

For a demo of notebooks, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/8mPocHMP0C8/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/8mPocHMP0C8)

**To create an SQL notebook**

1. Choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) and then choose **Notebook**\.

   By default, an SQL query cell appears in the new SQL notebook\.

1. \(Optionally\) Choose **Rename** and enter a name for the SQL notebook\.

1. In the SQL query cell, do one of the following:
   + Enter a query\.
   + Paste a query that you copied\.

1. \(Optionally\) Choose **Add markdown** to add a Markdown cell where you can provide descriptive or explanatory text using standard Markdown syntax\. 

1. \(Optionally\) Choose **Add SQL** and **Add markdown** to insert additional SQL and Markdown text cells\. 

**To open a notebook**

1. From the navigator menu, choose **Notebooks**\.

1. Choose the SQL notebook that you want to open and double\-click it\.

**To share an SQL notebook with your team**
+ Choose **Share to *team\-name***\.