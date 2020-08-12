# Generating IAM database credentials using the Amazon Redshift CLI or API<a name="generating-iam-credentials-cli-api"></a>

To programmatically generate temporary database user credentials, Amazon Redshift provides the [get\-cluster\-credentials](https://docs.aws.amazon.com/cli/latest/reference/redshift/get-cluster-credentials.html) command for the AWS Command Line Interface \(AWS CLI\) and the [GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) API operation\. Alternatively, you can configure your SQL client with Amazon Redshift JDBC or ODBC drivers that manage the process of calling the `GetClusterCredentials` operation, retrieving the database user credentials, and establishing a connection between your SQL client and your Amazon Redshift database\. For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.

**Note**  
We recommend using the Amazon Redshift JDBC or ODBC drivers to generate database user credentials\.

In this section, you can find steps to programmatically call the `GetClusterCredentials` operation or get\-cluster\-credentials command, retrieve database user credentials, and connect to the database\.

**To generate and use temporary database credentials**

1. Create or modify an IAM user or role with the required permissions\. For more information about IAM permissions, see [Create an IAM role or user role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\.

1. As an IAM user or role you authorized in the previous step, execute the get\-cluster\-credentials CLI command or call the `GetClusterCredentials` API operation and provide the following values:
   + **Cluster identifier** – The name of the cluster that contains the database\.
   + **Database user name** – The name of an existing or new database user\.
     + If the user doesn't exist in the database and AutoCreate is true, a new user is created with PASSWORD disabled\.
     +  If the user doesn't exist, and AutoCreate is false, the request fails\. 
     + For this example, the database user name is `temp_creds_user`\. 
   +  **Autocreate ** – \(Optional\) Create a new user if the database user name doesn't exist\.
   +  **Database name ** – \(Optional\) The name of the database that the user is authorized to log on to\. If database name isn't specified, the user can log on to any cluster database\.
   +  **Database groups ** – \(Optional\) A list of existing database user groups\. Upon successful login, the database user is added to the specified user groups\. If no group is specified, the user has only PUBLIC permissions\. The user group names must match the dbgroup resources ARNs specified in the IAM policy attached to the IAM user or role\. 
   +  **Expiration time** – \(Optional\) The time, in seconds, until the temporary credentials expire\. You can specify a value between 900 seconds \(15 minutes\) and 3600 seconds \(60 minutes\)\. The default is 900 seconds\.

1. Amazon Redshift verifies that the IAM user has permission to call the `GetClusterCredentials` operation with the specified resources\. 

1. Amazon Redshift returns a temporary password and the database user name\.

   The following example uses the Amazon Redshift CLI to generate temporary database credentials for an existing user named `temp_creds_user`\.

   ```
   aws redshift get-cluster-credentials --cluster-identifier examplecluster --db-user temp_creds_user --db-name exampledb --duration-seconds 3600
   ```

   The result is as follows\.

   ```
   {
     "DbUser": "IAM:temp_creds_user", 
     "Expiration": "2016-12-08T21:12:53Z", 
     "DbPassword": "EXAMPLEjArE3hcnQj8zt4XQj9Xtma8oxYEM8OyxpDHwXVPyJYBDm/gqX2Eeaq6P3DgTzgPg=="
   }
   ```

   The following example uses the Amazon Redshift CLI with autocreate to generate temporary database credentials for a new user and add the user to the group `example_group`\.

   ```
   aws redshift get-cluster-credentials --cluster-identifier examplecluster --db-user temp_creds_user -–auto-create --db-name exampledb -–db-groups example_group --duration-seconds 3600
   ```

   The result is as follows\.

   ```
   {
     "DbUser": "IAMA:temp_creds_user:example_group", 
     "Expiration": "2016-12-08T21:12:53Z", 
     "DbPassword": "EXAMPLEjArE3hcnQj8zt4XQj9Xtma8oxYEM8OyxpDHwXVPyJYBDm/gqX2Eeaq6P3DgTzgPg=="
   }
   ```

1. Establish a Secure Socket Layer \(SSL\) authentication connection with the Amazon Redshift cluster and send a login request with the user name and password from the `GetClusterCredentials` response\. Include the `IAM:` or `IAMA:` prefix with the user name, for example `IAM:temp_creds_user` or `IAM:temp_creds_user`\.
**Important**  
Configure your SQL client to require SSL\. Otherwise, if your SQL client automatically tries to connect with SSL, it can fall back to non\-SSL if there is any kind of failure\. In that case, the first connection attempt might fail because the credentials are expired or invalid, then a second connection attempt fails because the connection is not SSL\. If that occurs, the first error message might be missed\. For more information about connecting to your cluster using SSL, see [Configuring security options for connections](connecting-ssl-support.md)\.

1. If the connection doesn't use SSL, the connection attempt fails\. 

1. The cluster sends an `authentication` request to the SQL client\. 

1. The SQL client then sends the temporary password to the cluster\. 

1. If the password is valid and has not expired, the cluster completes the connection\. 