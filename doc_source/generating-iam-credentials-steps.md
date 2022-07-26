# Creating temporary IAM user credentials<a name="generating-iam-credentials-steps"></a>

In this section, you can find how to configure your system to generate temporary IAM\-based database user credentials and log in to your database using the new credentials\.

At a high level, the process flows as follows:

1. [Step 1: Create an IAM role for IAM Single Sign\-On \(SSO\) access](generating-iam-credentials-sso-role.md)

   \(Optional\) You can authenticate users for access to an Amazon Redshift database by integrating IAM authentication and a third\-party identity provider \(IdP\)\. 

1. [Step 2: Configure SAML assertions for your IdP](configuring-saml-assertions.md)

   \(Optional\) To use IAM authentication using an IdP, you need to define a claim rule in your IdP application that maps users or groups in your organization to the IAM role\. Optionally, you can include attribute elements to set `GetClusterCredentials` parameters\.

1. [Step 3: Create an IAM role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)

   Your SQL client application assumes the IAM role when it calls the `GetClusterCredentials` operation\. If you created an IAM role for identity provider access, you can add the necessary permission to that role\.

1. [Step 4: Create a database user and database groups](generating-iam-credentials-user-and-groups.md)

   \(Optional\) By default, `GetClusterCredentials` returns credentials create a new user if the user name doesn't exist\. You can also choose to specify user groups that users join at logon\. By default, database users join the PUBLIC group\.

1. [Step 5: Configure a JDBC or ODBC connection to use IAM credentials](generating-iam-credentials-configure-jdbc-odbc.md)

   To connect to your Amazon Redshift database, you configure your SQL client to use an Amazon Redshift JDBC or ODBC driver\. 