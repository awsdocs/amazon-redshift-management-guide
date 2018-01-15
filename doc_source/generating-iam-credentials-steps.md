# Creating Temporary IAM User Credentials<a name="generating-iam-credentials-steps"></a>

In this section, you can find the steps to configure your system to generate temporary IAM\-based database user credentials and log on to your database using the new credentials\.

At a high level, the process flows as follows:

1. [Step 1: Create an IAM Role for IAM Single Sign\-On \(SSO\) Access](generating-iam-credentials-sso-role.md)

   \(Optional\) You can authenticate users for access to an Amazon Redshift database by integrating IAM authentication and a third\-party identity provider \(IdP\), such as PingFederate, Okta, or ADFS\. 

1. [Step 2: Configure SAML Assertions for Your IdP](configuring-saml-assertions.md)

   \(Optional\) To use IAM authentication using an IdP, you need to define a claim rule in your IdP application that maps users or groups in your organization to the IAM role\. Optionally, you can include attribute elements to set GetClusterCredentials parameters\.

1. [Step 3: Create an IAM Role or User With Permissions to Call GetClusterCredentials](generating-iam-credentials-role-permissions.md)

   Your SQL client application assumes the IAM role when it calls the GetClusterCredentials action\. If you created an IAM role for identity provider access, you can add the necessary permission to that role\.

1. [Step 4: Create a Database User and Database Groups](generating-iam-credentials-user-and-groups.md)

   \(Optional\) By default, GetClusterCredentials returns credentials for existing users\. You can choose to have GetClusterCredentials create a new user if the user name doesn't exist\. You can also choose to specify user groups that users join at logon\. By default, database users join the PUBLIC group\.

1. [Step 5: Configure a JDBC or ODBC Connection to Use IAM Credentials](generating-iam-credentials-configure-jdbc-odbc.md)

   To connect to your Amazon Redshift database, you configure your SQL client to use an Amazon Redshift JDBC or ODBC driver\. 