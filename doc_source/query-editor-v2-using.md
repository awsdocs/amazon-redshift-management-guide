# Working with query editor v2<a name="query-editor-v2-using"></a>

The query editor v2 is primarily used to edit and run queries, visualize results, and share your work with your team\. With query editor v2, you can create databases, schemas, tables, and user\-defined functions \(UDFs\)\. In a tree\-view panel, for each of your databases, you can view its schemas\. For each schema, you can view its tables, views, UDFs, and stored procedures\.

**Topics**
+ [Opening query editor v2](#query-editor-v2-open)
+ [Connecting to an Amazon Redshift database](#query-editor-v2-connecting)
+ [Browsing an Amazon Redshift database](#query-editor-v2-object-browse)
+ [Creating database objects](#query-editor-v2-object-create)
+ [Viewing query and tab history](#query-editor-v2-history)
+ [Changing account settings](#query-editor-v2-settings)

## Opening query editor v2<a name="query-editor-v2-open"></a>

**To open the query editor v2**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. From the navigator menu, choose **Editor**, then **Query editor V2**\. The query editor v2 opens in a new browser tab\.

The query editor page has a navigator menu where you choose a view as follows:

**Editor ![\[Editor\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-align-left.png)**  
You manage and query your data organized as tables and contained in a database\. The database can contain stored data or contain a reference to data stored elsewhere, such as Amazon S3\. You connect to a database contained in either a cluster or a serverless workgroup\.  
When working in the **Editor** view, you have the following controls:   
+ The **Cluster** or **Workgroup** field displays the name you are currently connected to\. The **Database** field displays the databases within the cluster or workgroup\. The actions that you perform in the **Database** view default to act on the database you have selected\. 
+ A tree\-view hierarchical view of your clusters or workgroups, databases, and schemas\. Under schemas, you can work with your tables, views, functions, and stored procedures\. Each object in the tree view supports a context menu to perform associated actions, such as **Refresh** or **Drop**, for the object\. 
+ A ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png) **Create** action to create databases, schemas, tables, and functions\.
+ A ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/download.png) **Load data** action to load data from Amazon S3 into your databases\.
+ A ![\[Save\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-floppy-disk.png) **Save** icon to save your query\. 
+ A ![\[Shortcuts\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-key-command.png) **Shortcuts** icon to display keyboard shortcuts for the editor\. 
+ An ![\[Editor\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Editor** area where you can enter and run your query\. 

  After you run a query, a **Result** tab appears with the results\. Here is where you can turn on **Chart** to visualize your results\. You can also **Export** your results\.
+ A ![\[Notebook\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Notebook** area where you can add sections to enter and run SQL or add Markdown\. 

  After you run a query, a **Result** tab appears with the results\. Here is where you can **Export** your results\.

**Queries ![\[Queries\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-folder-close.png)**  
A query contains the SQL commands to manage and query your data in a database\. When you use query editor v2 to load sample data, it also creates and saves sample queries for you\.  
 When you choose a saved query, you can open, rename, and delete it using the context \(right\-click\) menu\. You can view attributes such as the **Query ARN** of a saved query by choosing **Query details**\. You can also view its version history, edit tags attached to the query, and share it with your team\.

**Notebooks ![\[Notebooks\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-manual.png)**  
A SQL notebook contains SQL and Markdown cells\. Use notebooks to organize, annotate, and share multiple SQL commands in a single document\.  
 When you choose a saved notebook, you can open, rename, duplicate, and delete it using the context \(right\-click\) menu\. You can view attributes such as the **Notebook ARN** of a saved notebook by choosing **Notebook details**\. You can also view its version history, edit tags attached to the notebook, export it, and share it with your team\. For more information, see [Authoring and running notebooks](query-editor-v2-notebooks.md)\.

**Charts ![\[Chart\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-chart.png)**  
A chart is a visual representation of your data\. The query editor v2 provides the tools to create many types of charts and save them\.   
 When you choose a saved chart, you can open, rename, and delete it using the context \(right\-click\) menu\. You can view attributes such as the **Chart ARN** of a saved chart by choosing **Chart details**\. You can also edit tags attached to the chart and export it\. For more information, see [Visualizing query results](query-editor-v2-charts.md)\. 

**History ![\[History\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-clock.png)**  
The query history is a list of queries you ran using Amazon Redshift query editor v2\. These queries either ran as individual queries or as part of a SQL notebook\. For more information, see [Viewing query and tab history](#query-editor-v2-history)\. 

 All query editor v2 views have the following icons:
+ A ![\[Visual mode\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-moon.png) **Visual mode** icon to toggle between light mode and dark mode\.
+ A ![\[Settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-cog.png) **Settings** icon to show a menu of the different settings screens\.
  + An ![\[Editor preferences\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-properties.png) **Editor preferences** icon to edit your preferences when you use query editor v2\.
  + A ![\[Connections\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-connection.png) **Connections** icon to view the connections used by your editor tabs\.

    A connection is used to retrieve data from a database\. A connection is created for a specific database\. With an isolated connection, the results of a SQL command that changes the database, such as creating a temporary table, in one editor tab, are not visible in another editor tab\. When you open an editor tab in query editor v2, the default is an isolated connection\. When you create a shared connection, that is, turn off the **Isolated session** switch, then the results in other shared connections to the same database are visible to each other\. However, editor tabs using a shared connection to a database don't run in parallel\. Queries using the same connection must wait until the connection is available\. A connection to one database can't be shared with another database, and thus SQL results are not visible across different database connections\.

    The number of connections any user in the account can have active is controlled by a query editor v2 administrator\.
  + An ![\[Account settings\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-settings.png) **Account settings** icon used by an administrator to change certain settings of all users in the account\. For more information, see [Changing account settings](#query-editor-v2-settings)\.

## Connecting to an Amazon Redshift database<a name="query-editor-v2-connecting"></a>

To connect to a database, choose the cluster or workgroup name in the tree\-view panel\. If prompted, enter the connection parameters\.

When you connect to a cluster or workgroup and its databases, you usually provide a  **Database** name\.  You also provide parameters required for one of the following authentication methods:

**Federated user**  
With this method, the principal tags of your IAM role or user must provide the connection details\. You configure these tags in AWS Identity and Access Management or your identity provider \(IdP\)\. The query editor v2 relies on the following tags\.  
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
This option is only available when connecting to a cluster\. With this method, instead of a database name, you provide a **Secret** stored in Secrets Manager that contains your database and sign\-in credentials\.  Use the Secrets Manager console \([https://console\.aws\.amazon\.com/secretsmanager/](https://console.aws.amazon.com/secretsmanager/)\) to **Store a new secret** and choose the secret type **Credentials for Amazon Redshift cluster** to enter your credentials for a cluster\. You must also add a tag key that starts with the string "Redshift" for the secret to be listed on the query editor v2 console\. 

When you select a cluster or workgroup with query editor v2, depending on the context, you can create, edit, and delete connections using the context \(right\-click\) menu\.  You can view attributes such as the **Connection ARN** of the connection by choosing **Connection details**\. You can also edit tags attached to the connection\.

## Browsing an Amazon Redshift database<a name="query-editor-v2-object-browse"></a>

Within a database, you can manage schemas, tables, views, functions, and stored procedures in the tree\-view panel\. Each object in the view has actions associated with it in a context \(right\-click\) menu\.

The hierarchical tree\-view panel displays database objects\. To refresh the tree\-view panel to display database objects that might have been created after the tree\-view was last displayed, choose the ![\[Refresh\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-refresh.png) icon\. Open the context \(right\-click\) menu for an object to see what actions you can perform\.

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

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png)**Create**, and then choose **Database**\.

1. Enter a **Database name**\.

1. \(Optional\) Select **Users and groups**, and choose a **Database user**\.

1. Choose **Create database**\.

   The new database displays in the tree\-view panel\.

**To create a schema**

For information about schemas, see [Schemas](https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png)**Create**, and then choose **Schema**\.

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

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png)**Create**, and choose **Table**\.

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

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-add.png)**Create**, and choose **Function**\.

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

You can view your query history with query editor v2\. Only queries that you ran using query editor v2 appear in the query history\. Both queries that ran from using an **Editor** tab or **Notebook** tab are shown\. You can filter the list displayed by a time period, such as `This week`, where a week is defined as Monday–Sunday\. The list of queries fetches 25 rows of queries that match your filter at a time\. Choose **Load more** to see the next set\. Choose a query and from the **Actions** menu\. The actions available depend on whether the chosen query has been saved\. You can choose the following operations:
+ **View query details** – Displays a query details page with more information about the query that ran\.
+ **Open query in a new tab** – Opens a new editor tab and primes it with the chosen query\. If still connected, the cluster or workgroup and database are automatically selected\. To run the query, first confirm that the correct cluster or workgroup and database are chosen\.
+ **Open source tab** – If it is still open, navigates to the editor or notebook tab that contained the query when it ran\. The contents of the editor or notebook might have changed after the query ran\.
+ **Open saved query** – Navigates to the editor or notebook tab and opens the query\.

You can also view the history of queries run in an **Editor** tab or the history of queries run in a **Notebook** tab\. To see a history of queries in a tab, choose **Tab history**\. Within the tab history, you can do the following operations:
+ **Copy query** – Copies the query version SQL content to the clipboard\.
+ **Open query in a new tab** – Opens a new editor tab and primes it with the chosen query\. To run the query, you must choose the cluster or workgroup and database\.
+ **View query details** – Displays a query details page with more information about the query that ran\.

## Changing account settings<a name="query-editor-v2-settings"></a>

A user with the right IAM permissions can view and change **Account settings** for other users in the same AWS account\. This administrator can view or set the following:
+ The maximum concurrent database connections per user in the account\. This includes connections for **Isolated sessions**\. When you change this value, it can take 10 minutes for the change to take effect\.
+ Select whether to **Authenticate with IAM credentials**:
  + When you select this option, then account users are shown **Temporary credentials using your IAM identity** on the connection window to authenticate with their IAM credentials\.
  + When you deselect this option, then account users are shown **Temporary credentials** on the connection window to authenticate with database user name and password\. The default is for this option to be deselected\.
+ Allow users in the account to export an entire result set from a SQL command to a file\.
+ Load and display sample databases with some associated saved queries\.
+ Specify an Amazon S3 path used by account users to load data from a local file\.
+ View the KMS key ARN used to encrypt query editor v2 resources\.