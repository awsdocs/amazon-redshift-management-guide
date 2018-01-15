# Using IAM Authentication to Generate Database User Credentials<a name="generating-user-credentials"></a>

To better manage the access your users have to your Amazon Redshift database, you can generate temporary database credentials based on permissions granted through an AWS Identity and Access Management \(IAM\) permissions policy\. 

Commonly, Amazon Redshift database users log on to the database by providing a database user name and password\. As an alternative to maintaining user names and passwords in your Amazon Redshift database, you can configure your system to permit users to create user credentials and log on to the database based on their IAM credentials\. You can also configure your system to let users sign on using federated single sign\-on \(SSO\) through a SAML 2\.0\-compliant identity provider\. 


+ [Overview](generating-iam-credentials-overview.md)
+ [Creating Temporary IAM User Credentials](generating-iam-credentials-steps.md)
+ [Options for Providing IAM Credentials](options-for-providing-iam-credentials.md)
+ [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)
+ [Generating IAM Database Credentials Using the Amazon Redshift CLI or API](generating-iam-credentials-cli-api.md)