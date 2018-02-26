# Install and Configure the Amazon Redshift ODBC Driver on Microsoft Windows Operating Systems<a name="install-odbc-driver-windows"></a>

## System Requirements<a name="odbc-driver-sysreq-windows"></a>

You install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet the following minimum system requirements: 

+ Microsoft Windows Vista operating system or later

+ 55 MB of available disk space

+ Administrator privileges on the client computer

+ An Amazon Redshift master user or user account to connect to the database

## Installing the Amazon Redshift Driver on Windows Operating Systems<a name="odbc-driver-windows-how-to-install"></a>

Use the steps in this section to download the Amazon Redshift ODBC drivers for Microsoft Windows operating systems\. You should only use a driver other than these if you are running a third\-party application that is certified for use with Amazon Redshift and that requires a specific driver for that application\. 

**To install the ODBC driver**

1. Download one of the following, depending on the system architecture of your SQL client tool or application:

   + 32\-bit: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC32\-1\.3\.7\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC32-1.3.7.1000.msi) 

     The name for this driver is Amazon Redshift \(x86\)\.

   +  64\-bit: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC64\-1\.3\.7\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC64-1.3.7.1000.msi) 

     The name for this driver is Amazon Redshift \(x64\)\.
**Note**  
Download the MSI package that corresponds to the system architecture of your SQL client tool or application\. For example, if your SQL client tool is 64\-bit, install the 64\-bit driver\.

    Then download and review the [Amazon Redshift ODBC Driver License Agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+Driver+License+Agreement.pdf)\. If you need to distribute these drivers to your customers or other third parties, please email *redshift\-pm@amazon\.com* to arrange an appropriate license\. 

1.  Double\-click the \.msi file, and then follow the steps in the wizard to install the driver\. 

## Creating a System DSN Entry for an ODBC Connection on Microsoft Windows<a name="create-dsn-odbc-windows"></a>

After you download and install the ODBC driver, you need to add a data source name \(DSN\) entry to the client machine or Amazon EC2 instance\. SQL client tools use this data source to connect to the Amazon Redshift database\. 

**Note**  
For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, additional steps are required\. For more information, see [Configure a JDBC or ODBC Connection to Use IAM Credentials](generating-iam-credentials-configure-jdbc-odbc.md)\.

**To create a system DSN entry**

1. In the **Start** menu, in your list of programs, locate the driver folder or folders\. 
**Note**  
If you installed the 32\-bit driver, the folder is named **Amazon Redshift ODBC Driver \(32\-bit\)**\. If you installed the 64\-bit driver, the folder is named **Amazon Redshift ODBC Driver \(64\-bit\)**\. If you installed both drivers, you'll have a folder for each driver\. 

1. Click **ODBC Administrator**, and then type your administrator credentials if you are prompted to do so\.

1.  Select the **System DSN** tab if you want to configure the driver for all users on the computer, or the **User DSN** tab if you want to configure the driver for your user account only\. 

1.  Click **Add**\. The **Create New Data Source** window opens\. 

1.  Select the **Amazon Redshift** ODBC driver, and then click **Finish**\. The ** Amazon Redshift ODBC Driver DSN Setup** window opens\.

1. Under **Connection Settings**, enter the following information:
<a name="rs-mgmt-dsn"></a>
**Data Source Name**  
Type a name for the data source\. You can use any name that you want to identify the data source later when you create the connection to the cluster\. For example, if you followed the *Amazon Redshift Getting Started*, you might type `exampleclusterdsn` to make it easy to remember the cluster that you will associate with this DSN\.
<a name="rs-mgmt-server"></a>
**Server**  
Specify the endpoint for your Amazon Redshift cluster\. You can find this information in the Amazon Redshift console on the cluster’s details page\. For more information, see [Configuring Connections in Amazon Redshift](configuring-connections.md)\.
<a name="rs-mgmt-port"></a>
**Port**  
Type the port number that the database uses\. By default, Amazon Redshift uses 5439, but you should use the port that the cluster was configured to use when it was launched\.
<a name="rs-mgmt-database"></a>
**Database**  
Type the name of the Amazon Redshift database\. If you launched your cluster without specifying a database name, type *dev*; otherwise, use the name that you chose during the launch process\. If you followed the *Amazon Redshift Getting Started*, type *dev*\.

1. Under **Credentials**, enter the following information:
<a name="rs-mgmt-creds-user"></a>
**User**  
Type the user name for the database user account that you want to use to access the database\. If you followed the *Amazon Redshift Getting Started*, type *masteruser*\.
<a name="rs-mgmt-creds-password"></a>
**Password**  
Type the password that corresponds to the database user account\.

1. Under **SSL Settings**, specify a value for the following:
<a name="rs-mgmt-ssl-authentication"></a>
**SSL Authentication**  
Select a mode for handling Secure Sockets Layer \(SSL\)\. In a test environment, you might use `prefer`, but for production environments and when secure data exchange is required, use `verify-ca` or `verify-full`\. For more information about using SSL, see [Connect Using SSL](connecting-ssl-support.md#connect-using-ssl)\.

1. Under **Additional Options**, select one of the following options to specify how to return query results to your SQL client tool or application: 

   + **Single Row Mode**\. Select this option if you want query results to be returned one row at a time to the SQL client tool or application\. Use this option if you plan to query for large result sets and don't want the entire result in memory\. Disabling this option improves performance, but it can increase the number of out\-of\-memory errors\.

   + **Use Declare/Fetch**\. Select this option if you want query results to be returned to the SQL client tool or application in a specified number of rows at a time\. Specify the number of rows in **Cache Size**\.

   + **Use Multiple Statements**\. Select this option to return results based on multiple SQL statements in a query\.

   + **Retrieve Entire Result Into Memory**\. Select this option if you want query results to be returned all at once to the SQL client tool or application\. The default is enabled\. 

1. In **Logging Options**, specify values for the following: 

   + **Log Level**\. Select an option to specify whether to enable logging and the level of detail that you want captured in the logs\. 
**Important**  
You should only enable logging when you need to capture information about an issue\. Logging decreases performance, and it can consume a large amount of disk space\.

   + **Log Path**\. Specify the full path to the folder where you want to save log files\.

    Then click **OK**\.

1. In **Data Type Options**, specify values for the following: 

   + **Use Unicode**\. Select this option to enable support for Unicode characters\. The default is enabled\.

   + **Show Boolean Column As String**\. Select this option if you want Boolean values to be displayed as string values instead of bit values\. If you enable this, **"1"** and **"0"** display instead of **1** and **0**\. The default is enabled\.

   + **Text as LongVarChar**\. Select this option to enable showing text as LongVarChar\. The default is enabled\.

   + **Max Varchar**\. Specify the maximum value for the Varchar data type\. A Varchar field with a value larger than the maximum specified will be promoted to LongVarchar\. The default value is 255\.

   + **Max LongVarChar**\. Specify the maximum value for the LongVarChar data type\. A LongVarChar field value that is larger than the maximum specified will be truncated\. The default value is 8190\.

   + **Max Bytea**\. Specify the maximum value for the Bytea data type\. A Bytea field value that is larger than the maximum specified will be truncated\. The default value is 255\. 
**Note**  
The Bytea data type is only used by Amazon Redshift system tables and views, and otherwise is not supported\.

   Then click **OK**\.

1.  Click **Test**\. If the client computer can connect to the Amazon Redshift database, you will see the following message: **Connection successful**\. 

 If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting Connection Issues in Amazon Redshift](troubleshooting-connections.md)\. 