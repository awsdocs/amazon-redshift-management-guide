# JDBC and ODBC Options for Creating Database User Credentials<a name="jdbc-and-odbc-options-for-database-credentials"></a>

To use the Amazon Redshift JDBC or ODBC driver to create database user credentials, provide the database user name as a JDBC or ODBC option\. Optionally, you can have the driver create a new database user if one doesn't exist, and you can specify a list of database user groups the user joins at logon\. 

If you use an identity provider \(IdP\), work with your IdP administrator to determine the correct values for these options\. Your IdP administrator can also configure your IdP to provide these options, in which case you don't need to provide them as JDBC or ODBC options\. For more information, see [Configure SAML Assertions for Your IdP](configuring-saml-assertions.md)\. 

The following table lists the options for creating database user credentials\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/jdbc-and-odbc-options-for-database-credentials.html)