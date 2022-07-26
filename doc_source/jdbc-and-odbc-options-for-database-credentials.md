# JDBC and ODBC Options for Creating Database User Credentials<a name="jdbc-and-odbc-options-for-database-credentials"></a>

To use the Amazon Redshift JDBC or ODBC driver to create database user credentials, provide the database user name as a JDBC or ODBC option\. Optionally, you can have the driver create a new database user if one doesn't exist, and you can specify a list of database user groups the user joins at login\. 

If you use an identity provider \(IdP\), work with your IdP administrator to determine the correct values for these options\. Your IdP administrator can also configure your IdP to provide these options, in which case you don't need to provide them as JDBC or ODBC options\. For more information, see [Configure SAML assertions for your IdP](configuring-saml-assertions.md)\. 

**Note**  
If you use an IAM policy variable `${redshift:DbUser}`, as described in [Resource policies for GetClusterCredentials](redshift-iam-access-control-identity-based.md#redshift-policy-resources.getclustercredentials-resources) the value for `DbUser` is replaced with the value retrieved by the API operation's request context\. The Amazon Redshift drivers use the value for the `DbUser` variable provided by the connection URL, rather than the value supplied as a SAML attribute\.   
To help secure this configuration, we recommend that you use a condition in an IAM policy to validate the `DbUser` value with the `RoleSessionName`\. You can find examples of how to set a condition using an IAM policy in [Example policy for using GetClusterCredentials](redshift-iam-access-control-identity-based.md#redshift-policy-examples-getclustercredentials)\.

The following table lists the options for creating database user credentials\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/jdbc-and-odbc-options-for-database-credentials.html)