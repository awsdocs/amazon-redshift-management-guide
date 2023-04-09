# Installing and configuring the Amazon Redshift ODBC driver on Microsoft Windows<a name="odbc20-install-config-win"></a>

## System Requirements<a name="odbc20-system-requirements-win"></a>

You must install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. For each computer where you install the driver, there are the following minimum requirements: 
+ Administrator rights on the machine\. 
+ The machine meets the following system requirements:
  + One of the following operating systems:
    + Windows 10 or 8\.1\.
    + Windows Server 2019, 2016, or 2012\.
  + 100 MB of available disk space\.
  + Visual C\+\+ Redistributable for Visual Studio 2015 for 64\-bit Windows installed\. You can download the installation package at [ Download Visual C\+\+ Redistributable for Visual Studio 2022](https://visualstudio.microsoft.com/downloads/#microsoft-visual-c-redistributable-for-visual-studio-2022) on the Microsoft website\.

## Installing the Amazon Redshift ODBC driver<a name="odbc20-install-win"></a>

Use the following procedure to download and install the Amazon Redshift ODBC driver for Windows operating systems\. Only use a different driver if you're running a third\-party application that is certified for use with Amazon Redshift, and that application requires that specific driver\.

To download and install the ODBC driver: 

1. Download the following driver: [64\-bit ODBC driver version 2\.0\.0\.5](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/2.0.0.5/AmazonRedshiftODBC64-2.0.0.5.msi) 

   The name for this driver is **Amazon Redshift ODBC Driver \(x64\)**\.
**Note**  
32\-bit ODBC drivers are discontinued\. Further updates will not be released, except for urgent security patches\. To download and install ODBC drivers for 32\-bit operating systems, see [ Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-connection.html#install-odbc-driver-windows)\.

1. Review the [ Amazon Redshift ODBC driver version 2\.x license](https://github.com/aws/amazon-redshift-odbc-driver/blob/master/LICENSE)\.

1. Double\-click the \.msi file, then follow the steps in the wizard to install the driver\.

## Creating a system DSN entry for an ODBC connection<a name="odbc20-dsn-win"></a>

After you download and install the ODBC driver, add a data source name \(DSN\) entry to the client computer or Amazon EC2 instance\. SQL client tools can use this data source to connect to the Amazon Redshift database\. 

We recommend that you create a system DSN instead of a user DSN\. Some applications load the data using a different database user account, and might not be able to detect user DSNs that are created under another database user account\.

**Note**  
For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, additional steps are required\. For more information, see [ Configure a JDBC or ODBC connection to use IAM credentials](https://docs.aws.amazon.com/redshift/latest/mgmt/generating-iam-credentials-configure-jdbc-odbc.html)\.

To create a system DSN entry for an ODBC connection:

1. In the **Start** menu, type "ODBC Data Sources\." Choose **ODBC Data Sources**\.

   Make sure that you choose the ODBC Data Source Administrator that has the same bitness as the client application that you are using to connect to Amazon Redshift\. 

1. In the **ODBC Data Source Administrator**, choose the **Driver** tab and locate the following driver folder: **Amazon Redshift ODBC Driver \(x64\)**\.

1. Choose the **System DSN** tab to configure the driver for all users on the computer, or the **User DSN** tab to configure the driver for your database user account only\.

1. Choose **Add**\. The **Create New Data Source** window opens\.

1. Choose the **Amazon Redshift ODBC driver \(x64\)**, and then choose **Finish**\. The **Amazon Redshift ODBC Driver DSN Setup** window opens\.

1. Under the **Connection Settings** section, enter the following information: 

**Data source name**  
 Enter a name for the data source\. For example, if you followed the *Amazon Redshift Getting Started Guide*, you might type `exampleclusterdsn` to make it easy to remember the cluster that you associate with this DSN\. 

**Server**  
 Specify the endpoint host for your Amazon Redshift cluster\. You can find this information in the Amazon Redshift console on the cluster's details page\. For more information, see [ Configuring connections in Amazon Redshift ](https://docs.aws.amazon.com/redshift/latest/mgmt/configuring-connections.html)\. 

**Port**  
 Enter the port number that the database uses\. Depending on the port you selected when creating, modifying or migrating the cluster, allow access to the selected port\.  

**Database**  
 Enter the name of the Amazon Redshift database\. If you launched your cluster without specifying a database name, enter `dev`\. Otherwise, use the name that you chose during the launch process\. If you followed the *Amazon Redshift Getting Started Guide*, enter `dev`\. 

1. Under the **Authentication** section, specify the configuration options to configure standard or IAM authentication\. 

1. Choose **SSL Options** and specify a value for the following:

**Authentication mode**  
Choose a mode for handling Secure Sockets Layer \(SSL\)\. In a test environment, you might use `prefer`\. However, for production environments and when secure data exchange is required, use `verify-ca` or `verify-full`\.

1.  In the **Proxy** tab, specify any proxy connection setting\. 

1. In the **Cursor** tab, specify options on how to return query results to your SQL client tool or application\. 

1. In **Advanced Options**, specify values for the logging option and other options\. 

1. Choose **Test**\. If the client computer can connect to the Amazon Redshift database, the following message appears: **Connection successful**\. If the client computer fails to connect to the database, you can troubleshoot possible issues by generating a log file and contacting AWS support\. For information on generating logs, see \(LINK\)\. 

1.  Choose **OK**\. 