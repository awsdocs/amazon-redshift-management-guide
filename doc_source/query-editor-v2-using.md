# Working with query editor v2<a name="query-editor-v2-using"></a>

The query editor v2 is primarily used to edit and run queries, visualize results, and share your work with your team\. With query editor v2, you can create databases, schemas, tables, and user\-defined functions \(UDFs\)\. In a tree\-view panel, for each of your databases, you can view its schemas\. For each schema, you can view its tables, views, UDFs, and stored procedures\.

**Topics**
+ [Opening query editor v2](#query-editor-v2-open)
+ [Connecting to an Amazon Redshift database](#query-editor-v2-connecting)
+ [Browsing an Amazon Redshift database](#query-editor-v2-object-browse)
+ [Creating database objects](#query-editor-v2-object-create)
+ [Viewing query and tab history](#query-editor-v2-history)
+ [Loading sample data](#query-editor-v2-loading-sample-data)
+ [Loading data from Amazon S3](#query-editor-v2-loading-data)
+ [Changing account settings](#query-editor-v2-settings)

## Opening query editor v2<a name="query-editor-v2-open"></a>

**To open the query editor v2**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. From the navigator menu, choose **Editor**, then **Query editor V2**\. The query editor v2 opens in a new browser tab\.

The query editor page has a navigator menu where you choose a view as follows:

**Database ![\[Database\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-database.png)**  
You manage and query your data organized as tables and contained in a database\. The database can contain stored data or contain a reference to data stored elsewhere, such as Amazon S3\. You connect to a database contained in either a cluster or a serverless workgroup\.  
When working in the **Database** view, you have the following controls:   
+ The **Cluster** or **Workgroup** field displays the name you are currently connected to\. The **Database** field displays the databases within the cluster or workgroup\. The actions that you perform in the **Database** view default to act on the database you have selected\. 
+ A tree\-view hierarchical view of your clusters or workgroups, databases, and schemas\. Under schemas, you can work with your tables, views, functions, and stored procedures\. Each object in the tree view supports a context menu to perform associated actions, such as **Refresh** or **Drop**, for the object\. 
+ A ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Create** action to create databases, schemas, tables, and functions\.
+ A ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/download.png) **Load data** action to load data from Amazon S3 into your databases\.
+ A ![\[Save\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-floppy-disk.png) **Save** icon to save your query\. 
+ A ![\[Shortcuts\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-key-command.png) **Shortcuts** icon to display keyboard shortcuts for the editor\. 
+ An ![\[Editor\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Editor** area where you can enter and run your query\. 

  After you run a query, a **Result** tab appears with the results\. Here is where you can turn on **Chart** to visualize your results\. You can also **Export** your results\.
+ A ![\[Notebook\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Notebook** area where you can add sections to enter and run SQL or add Markdown\. 

  After you run a query, a **Result** tab appears with the results\. Here is where you can **Export** your results\.

**Queries ![\[Queries\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-folder-close.png)**  
A query contains the SQL commands to manage and query your data in a database\. When you use query editor v2 to load sample data, it also creates and saves sample queries for you\. You can share a saved query with your team\.

**Notebooks ![\[Notebooks\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-manual.png)**  
A SQL notebook contains SQL and Markdown cells\. Use notebooks to organize, annotate, and share multiple SQL commands in a single document\. You can share a notebook with your team\. For more information, see [Authoring and running notebooks](query-editor-v2-notebooks.md)\.

**Charts ![\[Chart\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-chart.png)**  
A chart is a visual representation of your data\. The query editor v2 provides the tools to create many types of charts and save them\. You can share a chart with your team\. For more information, see [Visualizing query results](query-editor-v2-charts.md)\. 

**History ![\[History\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-clock.png)**  
The query history is a list of queries you ran using Amazon Redshift query editor v2\. These queries either ran as individual queries or as part of a SQL notebook\. For more information, see [Viewing query and tab history](#query-editor-v2-history)\. 

 All query editor v2 views have the following icons:
+ A ![\[Visual mode\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-moon.png) **Visual mode** icon to toggle between light mode and dark mode\.
+ A ![\[Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/settings.png) **Settings** icon to show a menu of the different settings screens\.
  + An ![\[Editor preferences\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-properties.png) **Editor preferences** icon to edit your preferences when you use query editor v2\.
  + A ![\[Connections\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-connection.png) **Connections** icon to view the connections used by your editor tabs\.

    A connection is used to retrieve data from a database\. A connection is created for a specific database\. With an isolated connection, the results of a SQL command that changes the database, such as creating a temporary table, in one editor tab, are not visible in another editor tab\. When you open an editor tab in query editor v2, the default is an isolated connection\. When you create a shared connection, that is, turn off the **Isolated session** switch, then the results in other shared connections to the same database are visible to each other\. However, editor tabs using a shared connection to a database don't run in parallel\. Queries using the same connection must wait until the connection is available\. A connection to one database can't be shared with another database, and thus SQL results are not visible across different database connections\.

    The number of connections any user in the account can have active is controlled by a query editor v2 administrator\.
  + An ![\[Account settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-settings.png) **Account settings** icon used by an administrator to change certain settings of all users in the account\. For more information, see [Changing account settings](#query-editor-v2-settings)\.

## Connecting to an Amazon Redshift database<a name="query-editor-v2-connecting"></a>

To connect to a database, choose the cluster or workgroup name in the tree\-view panel\. If prompted, enter the connection parameters\.

When you connect to a cluster or workgroup and its databases, you usually provide a  **Database** name\.  You also provide parameters required for one of the following authentication methods:

**Federated user**  
With this method, the principal tags of your IAM role or IAM user must provide the connection details\. You configure these tags in AWS Identity and Access Management or your identity provider \(IdP\)\. The query editor v2 relies on the following tags\.  
+ `RedshiftDbUser` – This tag defines the database user that is used by query editor v2\. This tag is required\.
+ `RedshiftDbGroups` – This tag defines the database groups that are joined when connecting to query editor v2\. This tag is optional and its value must be a colon\-separated list such as `group1:group2:group3`\. Empty values are ignored, that is, `group1::::group2` is interpreted as `group1:group2`\. 
These tags are forwarded to the `redshift:GetClusterCredentials `  API to get credentials for your cluster\. For more information, see [Setting up principal tags to connect to query editor v2 as a federated user](query-editor-v2-getting-started.md#query-editor-v2-principal-tags-iam)\.

**Temporary credentials**  
This option is only available when connecting to a cluster\. With this method, query editor v2, provide a **User name** for the database\. The query editor v2 generates a temporary password to connect to the database\. Your query editor v2 administrator chooses whether this method is shown to all users in the account by configuring it in **Account settings**\. For more information, see [Changing account settings](#query-editor-v2-settings)\.

**Temporary credentials using your IAM identity**  
This option is only available when connecting to a cluster\. With this method, query editor v2 maps a user name to your IAM identity and generates a temporary password to connect to the database\. Your query editor v2 administrator chooses whether this method is shown to all users in the account by configuring it in **Account settings**\. For more information, see [Changing account settings](#query-editor-v2-settings)\. 

**Database user name and password**  
With this method, also provide a **User name** and **Password** for the database that you are connecting to\. The query editor v2 creates a secret on your behalf stored in AWS Secrets Manager\. This secret contains credentials to connect to your database\. 

**AWS Secrets Manager**  
This option is only available when connecting to a cluster\. With this method, instead of a database name, you provide a **Secret** stored in Secrets Manager that contains your database and sign in credentials\.

When you select a cluster or workgroup with query editor v2, depending on the context, you can create, edit, and delete connections using the context \(right\-click\) menu\.

## Browsing an Amazon Redshift database<a name="query-editor-v2-object-browse"></a>

Within a database, you can manage schemas, tables, views, functions, and stored procedures in the tree\-view panel\. Each object in the view has actions associated with it in a context \(right\-click\) menu\.

The hierarchical tree\-view panel displays database objects\. To refresh the tree\-view panel to display database objects which might have been created after the tree\-view was last displayed, choose the ![\[Refresh\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-refresh.png) icon\. Open the context \(right\-click\) menu for an object to see what actions you can perform\.

![\[Tree-view icons\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/sqlworkbench-tree-view.png)

After you choose a table, you can do the following:
+ To start a query in the editor with a SELECT statement that queries all columns in the table, use **Select table**\.
+ To see the attributes or a table, use **Show table definition**\. Use this to see column names, column types, encoding, distribution keys, sort keys, and whether a column can contain null values\. For more information about table attributes, see [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) in the *Amazon Redshift Database Developer Guide*\.
+ To delete a table, use **Delete**\. You can either use **Truncate table** to delete all rows from the table or **Drop table** to remove the table from the database\. For more information, see [TRUNCATE](https://docs.aws.amazon.com/redshift/latest/dg/r_TRUNCATE.html) and [DROP TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_DROP_TABLE.html) in the *Amazon Redshift Database Developer Guide*\. 

Choose a schema to **Refresh** or **Drop schema**\. 

Choose a view to **Show view definition** or **Drop view**\. 

Choose a function to **Show function definition** or **Drop function**\. 

Choose a stored procedure to **Show procedure definition** or **Drop procedure**\. 

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

1. Choose either **Local** or **External** as the **Schema type**\.

   For more information about local schemas, see [CREATE SCHEMA](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_SCHEMA.html) in the *Amazon Redshift Database Developer Guide*\. For more information about external schemas, see [CREATE EXTERNAL SCHEMA](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_SCHEMA.html) in the *Amazon Redshift Database Developer Guide*\.

1. If you choose **External**, then you have the following choices of an external schema\.
   + **Glue Data Catalog** – to create an external schema in Amazon Redshift that references tables in AWS Glue\. Besides choosing the AWS Glue database, choose the IAM role associated with the cluster and the IAM role associated with the Data Catalog\.
   + **PostgreSQL** – to create an external schema in Amazon Redshift that references an Amazon RDS for PostgreSQL or Amazon Aurora PostgreSQL\-Compatible Edition database\. Also provide the connection information to the database\. For more information about federated queries, see [Querying data with federated queries](https://docs.aws.amazon.com/redshift/latest/dg/federated-overview.html) in the *Amazon Redshift Database Developer Guide*\.
   + **MySQL** – to create an external schema in Amazon Redshift that references an Amazon RDS for MySQL or and Amazon Aurora MySQL\-Compatible Edition database\. Also provide the connection information to the database\. For more information about federated queries, see [Querying data with federated queries](https://docs.aws.amazon.com/redshift/latest/dg/federated-overview.html) in the *Amazon Redshift Database Developer Guide*\.

1. Choose **Create schema**\.

   The new schema appears in the tree\-view panel\.

**To create a table**

You can create a table based on a comma\-separated value \(CSV\) file that you specify or define each column of the table\. For information about tables, see [Designing tables](https://docs.aws.amazon.com/redshift/latest/dg/c_designing-tables-best-practices.html) and [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) in the *Amazon Redshift Database Developer Guide*\. 

Choose **Open query in editor** to view and edit the CREATE TABLE statement before you run the query to create the table\. 

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png)**Create**, and choose **Table**\.

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

## Viewing query and tab history<a name="query-editor-v2-history"></a>

You can view your query history with query editor v2\. Only queries that you ran using query editor v2 appear in the query history\. Both queries that ran from using an **Editor** tab or **Notebook** tab are shown\. You can filter the list displayed by a time period, such as `This week`, where a week is defined as Monday–Sunday\. The list of queries fetches 25 rows of queries that match your filter at a time\. Choose **Load more** to see the next set\. Choose a query and from the **Actions** menu\. The actions available depend on whether the choosen query has been saved\. You can choose the following operations:
+ **View query details** – Displays a query details page with more information about the query that ran\.
+ **Open query in a new tab** – Opens a new editor tab and primes it with the chosen query\. If still connected, the cluster or workgroup and database are automatically selected\. To run the query, first confirm that the correct cluster or workgroup and database are chosen\.
+ **Open source tab** – If it is still open, navigates to the editor or notebook tab that contained the query when it ran\. The contents of the editor or notebook might have changed after the query ran\.
+ **Open saved query** – Navigates to the editor or notebook tab and opens the query\.

You can also view the history of queries run in an **Editor** tab or the history of queries run in a **Notebook** tab\. To see a history of queries in a tab, choose **Tab history**\. Within the tab history, you can do the following operations:
+ **Copy query** – Copies the query version SQL content to the clipboard\.
+ **Open query in a new tab** – Opens a new editor tab and primes it with the chosen query\. To run the query, you must choose the cluster or workgroup and database\.
+ **View query details** – Displays a query details page with more information about the query that ran\.

## Loading sample data<a name="query-editor-v2-loading-sample-data"></a>

The query editor v2 comes with sample data and notebooks available to be loaded into a sample database and corresponding schema\. 

To load sample data, choose the ![\[External\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/external.png) icon associated with the sample data you want to load\. The query editor v2 then loads the data into a schema in database `sample_data_dev` and creates a folder of saved notebooks in your **Notebooks** folder\. 

The following sample datasets are available\.

**tickit**  
Most of the examples in the Amazon Redshift documentation use sample data called `tickit`\. This data consists of seven tables: two fact tables and five dimensions\. When you load this data the schema `tickit` is updated with sample data\. For more information about the `tickit` data, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

**tpch**  
This data is used for a decision support benchmark\. When you load this data the schema `tpch` is updated with sample data\. For more information about the `tpch` data, see [TPC\-H](http://www.tpc.org/tpch/)\. 

**tpcds**  
This data is used for a decision support benchmark\. When you load this data the schema `tpcds` is updated with sample data\. For more information about the `tpcds` data, see [TPC\-DS](http://www.tpc.org/tpcds/)\. 

## Loading data from Amazon S3<a name="query-editor-v2-loading-data"></a>

You can load data into an existing table from Amazon S3\.

**To load data into an existing table**

The COPY command is used by query editor v2 to load data from Amazon S3\. The COPY command generated and used in the query editor v2 load data wizard supports all the parameters available to the COPY command syntax to copy from Amazon S3\. For information about the COPY command and its options used to load data from Amazon S3, see [COPY from Amazon Simple Storage Service](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-source-s3.html) in the *Amazon Redshift Database Developer Guide*\. 

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

## Changing account settings<a name="query-editor-v2-settings"></a>

A user with the right IAM permissions can view and change **Account settings** for other users in the same AWS account\. This administrator can view or set the following:
+ The maximum concurrent database connections per user in the account\. This includes connections for **Isolated sessions**\. When you change this value, it can take 10 minutes for the change to take effect\.
+ Select whether to **Authenticate with IAM credentials**:
  + When you select this option, then account users are shown **Temporary credentials using your IAM identity** on the connection window to authenticate with their IAM credentials\.
  + When you deselect this option, then account users are shown **Temporary credentials** on the connection window to authenticate with database user name and password\. The default is for this option to be deselected\.
+ Allow users in the account to export an entire result set from a SQL command to a file\.
+ Load and display sample databases with some associated saved queries\.
+ View the KMS key ARN used to encrypt query editor v2 resources\.