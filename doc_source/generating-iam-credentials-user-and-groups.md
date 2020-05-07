# Step 4: Create a database user and database groups<a name="generating-iam-credentials-user-and-groups"></a>

Optionally, you can create a database user that you use to log in to the cluster database\. If you create temporary user credentials for an existing user, you can disable the user's password to force the user to log on with the temporary password\. Alternatively, you can use the `GetClusterCredentials` Autocreate option to automatically create a new database user\. 

You can create database user groups with the permissions you want the IAM database user to join at login\. When you call the `GetClusterCredentials` operation, you can specify a list of user group names that the new user joins at login\. These group memberships are valid only for sessions created using credentials generated with the given request\.

**To create a database user and database groups**

1. Log in to your Amazon Redshift database and create a database user using [CREATE USER](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_USER.html) or alter an existing user using [ALTER USER](https://docs.aws.amazon.com/redshift/latest/dg/r_ALTER_USER.html)\. 

1. Optionally, specify the PASSWORD DISABLE option to prevent the user from using a password\. When a user's password is disabled, the user can log on only using temporary IAM user credentials\. If the password is not disabled, the user can log on either with the password or using temporary IAM user credentials\. You can't disable the password for a superuser\.

   The following example creates a user with password disabled\.

   ```
   create user temp_creds_user password disable; 
   ```

   The following example disables the password for an existing user\. 

   ```
   alter user temp_creds_user password disable;
   ```

1. Create database user groups using [CREATE GROUP](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_GROUP.html)\. 

1. Use the [GRANT](https://docs.aws.amazon.com/redshift/latest/dg/r_GRANT.html) command to define access privileges for the groups\.