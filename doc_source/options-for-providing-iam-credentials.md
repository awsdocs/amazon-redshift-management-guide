# Options for Providing IAM Credentials<a name="options-for-providing-iam-credentials"></a>

To provide IAM credentials for a JDBC or ODBC connection, choose one of the following authentication types\.
+ **AWS Profile** 

  As an alternative to providing credentials values in the form of JDBC or ODBC settings, you can put the values in a named profile\. 
+ **AWS IAM Credentials**

  Provide values for AccessKeyID, SecretAccessKey, and, optionally, SessionToken in the form of JDBC or ODBC settings\. SessionToken is required only for an IAM role with temporary credentials\. For more information, see Temporary Security Credentials\. 
+ **Identity Provider** 

  If you use an identity provider for authentication, specify the name of an identity provider plugin\. The Amazon Redshift JDBC and ODBC drivers include plugins for the following SAML\-based credential providers: 
  + AD FS
  + PingFederate
  + Okta
  + Microsoft Azure AD

    For the steps to set up Microsoft Azure AD as an identity provider, see [Setting Up JDBC or ODBC Single Sign\-on Authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\. 
**Note**  
To use Azure AD with JDBC, the Amazon Redshift JDBC driver must be version 1\.2\.37\.1061 or later\. To use Azure AD with ODBC, the Amazon Redshift ODBC driver must be version 1\.4\.10\.1000 or later\. 

  You can provide the plugin name and related values in the form of JDBC or ODBC settings or by using a profile\. For more information, see [Configure JDBC Driver Options](configure-jdbc-connection.md#configure-jdbc-options) and [Configure ODBC Driver Options](configure-odbc-connection.md#configure-odbc-options)\. 

For more information, see [Configure a JDBC or ODBC Connection to Use IAM Credentials](generating-iam-credentials-configure-jdbc-odbc.md)\.

## JDBC and ODBC Options for Providing IAM Credentials<a name="jdbc-options-for-providing-iam-credentials"></a>

The following table lists the JDBC and ODBC options for providing IAM credentials\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

## Using a Credentials Provider Plugin<a name="using-credentials-provider-plugin"></a>

The following credential provider plugins are included with the Amazon Redshift JDBC driver\.
+ Active directory federation service \(AD FS\) 
+ Ping Federate \(Ping\) 

  Ping is supported only with the predetermined PingFederate IdP Adapter using Forms authentication\. 
+ Okta 

  Okta is supported only for the Okta\-supplied AWS Console default application\. 
+ Azure AD

 To use a SAML\-based credential provider plugin, specify the following options using JBDC or ODBC options or in a named profile: 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

The following example shows credentials provider plugin parameters in a named profile\.

```
[plug-in-creds]
plugin_name=com.amazon.redshift.plugin.AdfsCredentialsProvider
idp_host=demo.example.com
idp_port=443  
preferred_role=arn:aws:iam::123456789012:role/ADFS-Dev
user=example\user
password=Password1234
```

## Using a Configuration Profile<a name="using-configuration-profile"></a>

You can supply the IAM credentials options and `GetClusterCredentials` options as settings in named profiles in your AWS configuration file\. Provide the profile name by using the Profile JDBC option\. 

The configuration is stored in a file named `config` in a folder named `.aws` in your home directory\. Home directory location varies but can be referred to using the environment variables `%UserProfile%` in Windows and `$HOME` or `~` \(tilde\) in Unix\-like systems\.

When using the Amazon Redshift JDBC driver or ODBC driver with a bundled SAML\-based credential provider plugin, the following settings are supported\. If `plugin_name` is not used, the listed options are ignored\.
+ plugin\_name 
+ idp\_host 
+ idp\_port 
+ preferred\_role 
+ user 
+ password 
+ ssl\_insecure 
+ app\_id \(for Okta only\)
+ idp\_tenant \(for Microsoft Azure only\)
+ client\_secret \(for Microsoft Azure only\)
+ client\_id \(for Microsoft Azure only\)

The following example shows a configuration file with three profiles\. The `plug-in-creds` example includes the optional DbUser, AutoCreate, and DbGroups options\.

```
[default]
aws_access_key_id=AKIAIOSFODNN7EXAMPLE
aws_secret_access_key=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

[user2]
aws_access_key_id=AKIAI44QH8DHBEXAMPLE
aws_secret_access_key=je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
session_token=AQoDYXdzEPT//////////wEXAMPLEtc764bNrC9SAPBSM22wDOk4x4HIZ8j4FZTwdQWLWsKWHGBuFqwAeMicRXmxfpSPfIeoIYRqTflfKD8YUuwthAx7mSEI/qkPpKPi/kMcGd
QrmGdeehM4IC1NtBmUpp2wUE8phUZampKsburEDy0KPkyQDYwT7WZ0wq5VSXDvp75YU
9HFvlRd8Tx6q6fE8YQcHNVXAkiY9q6d+xo0rKwT38xVqr7ZD0u0iPPkUL64lIZbqBAz
+scqKmlzm8FDrypNC9Yjc8fPOLn9FX9KSYvKTr4rvx3iSIlTJabIQwj2ICCR/oLxBA==

[plug-in-creds]
plugin_name=com.amazon.redshift.plugin.AdfsCredentialsProvider
idp_host=demo.example.com
idp_port=443
preferred_role=arn:aws:iam::1234567:role/ADFS-Dev
user=example\user
password=Password1234
```

To use the credentials for the `user2` example, specify `Profile=user2` in the JDBC URL\. To use the credentials for the `plug-in creds` example, specify `Profile=plug-in-creds` in the JDBC URL\. 

For more information, see [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html) in the AWS Command Line Interface User Guide\. 

## Setting Up JDBC or ODBC Single Sign\-on Authentication with Microsoft Azure AD<a name="setup-azure-ad-identity-provider"></a>

You can use Microsoft Azure AD as an identity provider \(IdP\) to access your Amazon Redshift cluster\. Following, you can find a procedure that describes how to set up a trust relationship for this purpose\. For more information about configuring AWS as a service provider for the IdP, see [Configuring Your SAML 2\.0 IdP with Relying Party Trust and Adding Claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html#saml_relying-party) in the *IAM User Guide*\.

**To set up Azure AD and your AWS account to trust each other**

1. Create or use an existing Amazon Redshift cluster for your Azure AD users to connect to\. To configure the connection, certain properties of this cluster are needed, such as the cluster identifier\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)\.

1. Add Amazon Redshift as a non\-gallery application on the Microsoft Azure portal\. For detailed steps, see [Configure SAML\-based single sign\-on to non\-gallery applications](https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications) in the Microsoft Azure Application Management documentation\. 

1. Set up Azure Enterprise Application to control Amazon Redshift access on the Microsoft Azure portal: 
   + In the **Setup up Single Sign\-On with SAML** page, in the **Basic SAML Configuration** section, choose the **Edit** icon\.
   + For **Identifier \(Entity ID\)**, enter **https://signin\.aws\.amazon\.com/saml**\. 
   + For **Reply URL \(Assertion Consumer Service URL\)**, enter **https://signin\.aws\.amazon\.com/saml**\. 
   + In the **User attributes and claims** section, create the claims as shown in the following table\. Here, `123456789012` is your AWS account, *`AzureSSO`* is an IAM role you created, and *`AzureADProvider`* is the IAM provider\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

   At the **Manage the SAML signing certificate** step, save the Federation Metadata XML file for use when you create an IAM SAML identity provider\. 

1. Create an IAM SAML identity provider on the IAM console\. The Metadata Document that you provide is the Federation Metadata XML file that you saved when you set up Azure Enterprise Application\. For detailed steps, see [ Creating and Managing an IAM Identity Provider \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console) in the *IAM User Guide*\. 

1. Create an IAM role for SAML 2\.0 federation on the IAM console\. For detailed steps, see [ Creating a Role for SAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html#idp_saml_Create) in the *IAM User Guide*\. 

1. Create an IAM policy that you can attach to the IAM role that you created for SAML 2\.0 federation on the IAM console\. For detailed steps, see [ Creating IAM Policies \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. 

   Modify the following policy \(in JSON format\) for your environment: 
   + Substitute the AWS Region of your cluster for `us-west-1`\. 
   + Substitute your AWS account for *`123456789012`*\. 
   + Substitute your cluster identifier \(or `*` for all clusters\) for *`cluster-identifier`*\. 
   + Substitute your database \(or `*` for all databases\) for *`dev`*\. 
   + Substitute the unique identifier of your IAM role for *`AROAJ2UCCR6DPCEXAMPLE`*\. 
   + Substitute your tenant or company email for `example.com`\. 
   + Substitute the database group you plan to assign user to for *`my_dbgroup`*\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "redshift:GetClusterCredentials",
               "Resource": [
                   "arn:aws:redshift:us-west-1:123456789012:dbname:cluster-identifier/dev",
                   "arn:aws:redshift:us-west-1:123456789012:dbuser:cluster-identifier/${redshift:DbUser}",
                   "arn:aws:redshift:us-west-1:123456789012:cluster:cluster-identifier"
               ],
               "Condition": {
                   "StringEquals": {
                       "aws:userid": "AROAJ2UCCR6DPCEXAMPLE:${redshift:DbUser}@example.com"
                   }
               }
           },
           {
               "Effect": "Allow",
               "Action": "redshift:CreateClusterUser",
               "Resource": "arn:aws:redshift:us-west-1:123456789012:dbuser:cluster-identifier/${redshift:DbUser}"
           },
           {
               "Effect": "Allow",
               "Action": "redshift:JoinGroup",
               "Resource": "arn:aws:redshift:us-west-1:123456789012:dbgroup:cluster-identifier/my_dbgroup"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "redshift:DescribeClusters",
                   "iam:ListRoles"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

   This policy grants permissions as follows:
   + The first section grants permission to the `GetClusterCredentials` API operation to get temporary credentials for the specified cluster\. In this example, the resource is `cluster-identifier` with database *`dev`*, in account *`123456789012`*, and in AWS Region *`us-west-1`*\. The `${redshift:DbUser}` clause allows only users that match the `DbUser` value specified in Azure AD to connect\.
   + The condition clause enforces that the only users to get temporary credentials are users under the role specified by the role unique ID *`AROAJ2UCCR6DPCEXAMPLE`* in the IAM account identified by an email address in your company's email domain\. For more information about unique IDs, see [Unique IDs](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-unique-ids) in the *AWS IAM User Guide*\. 

     Your setup with your IdP \(in this case, Azure AD\) determines how the condition clause is written\. If your employee's email is `johndoe@example.com`, first set `${redshift:DbUser}` to the super field that matches the employee's user name `johndoe`\. Then set the AWS SAML `RoleSessionName` field to the super field that matches the employee’s email `johndoe@example.com` to make this condition work\. Consider the following:
     + If you set `${redshift:DbUser}` to be the employee's email, then remove the `@example.com` in the example JSON to match the `RoleSessionName`\. 
     + If you set the `RoleSessionId` to be just the employee's user name, then remove the `@example.com` in the example to match the `RoleSessionName`\. 
     + In example JSON, the `${redshift:DbUser}` and `RoleSessionName` are both set to the employee's email\. This example JSON uses the Amazon Redshift database user name with `@example.com` to sign the user in to access the cluster\.
   + The second section grants permission to create a `dbuser` name in the specified cluster\. In this example JSON, it restricts creation to `${redshift:DbUser}`\. 
   +  The third section grants permission to specify which `dbgroup` a user can join\. In this example JSON, a user can join the `my_dbgroup` group in the specified cluster\. 
   + The fourth section grants permission to actions the user can do on all resources\. In this example JSON, it allows users to call `redshift:DescribeClusters` to get cluster information such as the cluster endpoint, AWS Region, and port\. It also allows users to call `iam:ListRoles` to check which roles a user can assume\. 

1. Configure your database client to connect to your cluster through JDBC using your Azure AD single sign\-on\. 

   You can use any client that uses a JDBC driver to connect using Azure AD single sign\-on or use a language like Java to connect using a script\. For installation and configuration information, see [Configuring a JDBC Connection](configure-jdbc-connection.md)\.

   For example, you can use SQLWorkbench/J as the client\. When you configure SQLWorkbench/J, the URL of your database uses the following format\.

   ```
   jdbc:redshift:iam://cluster-identifier:us-west-1/dev
   ```

   If you use SQLWorkbench/J as the client, take the following steps:

   1. Start SQL Workbench/J\. In the **Select Connection Profile** page, add a **Profile Group** called **AzureAuth**\.

   1. For **Connection Profile**, enter **Azure**\.

   1. Choose **Manage Drivers**, and choose **Amazon Redshift**\. Choose the **Open Folder** icon next to **Library**, then choose the appropriate JDBC \.jar file\. 

   1. On the **Select Connection Profile** page, add information to the connection profile as follows:
      + For **User**, enter your Microsoft Azure user name\. This is the user name of the Microsoft Azure account that you are using for single sign\-on that has permission to the cluster that you are trying to authenticate using\.
      + For **Password**, enter your Microsoft Azure password\.
      + For **Drivers**, choose **Amazon Redshift \(com\.amazon\.comredshift\.jdbc\.Driver\)**\.
      + For **URL**, enter **jdbc:redshift:iam//*your\-cluster\-identifier*:*your\-cluster\-region*/*your\-database\-name***\.

   1. Choose **Extended Properties** to add additional information to the connection properties as follows:
      + For **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.AzureCredentialsProvider**\. This value specifies to the driver to use Azure single sign\-on as the authentication method\. 
      + For **idp\_tenant**, enter **your\-idp\-tenant**\. This is the tenant name of your company configured on your IdP \(Azure\)\. This value can either be the tenant name or the tenant unique ID with hyphens\.
      + For **client\_secret**, enter **your\-azure\-redshift\-application\-client\-secret**\. This is your client secret of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
      + For **client\_id**, enter **your\-azure\-redshift\-application\-client\-id**\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 

1. Configure your database client to connect to your cluster through ODBC using your Azure AD single sign\-on\. 

   Amazon Redshift provides ODBC drivers for Linux, Windows, and MAC OS X operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. 

   Also install and configure the latest Amazon Redshift OBDC driver for your operating system as follows:
   + For Windows, see [Install and Configure the Amazon Redshift ODBC Driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.
   + For MAC OS, see [Install the Amazon Redshift ODBC Driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.
   + For Linux, see [Install the Amazon Redshift ODBC Driver on Linux](configure-odbc-connection.md#install-odbc-driver-linux)\.

   On Windows, in the **Amazon Redshift ODBC Driver DSN Setup** page, under **Connection Settings**, enter the following information: 
   + For **Data Source Name**, enter ***your\-DSN***\. This specifies the data source name used as the ODBC profile name\. 
   + For **Auth type**, specify **Identity Provider: Azure AD**\. This is authentication method that the ODBC driver uses to authenticate using Azure single sign\-on\.
   + For **Cluster ID**, enter ***your\-cluster\-identifier***\. 
   + For **Region**, enter ***your\-cluster\-region***\.
   + For **Database**, enter ***your\-database\-name***\.
   + For **User**, enter ***your\-azure\-username***\. This is the user name for the Microsoft Azure account that you are using for single sign\-on that has permission to the cluster that you're trying to authenticate using\.
   + For **Password**, enter ***your\-azure\-password***\. 
   + For **IdP Tenant**, enter ***your\-idp\-tenant***\. This is the tenant name of your company configured on your IdP \(Azure\)\. This value can either be the tenant name or the tenant unique ID with hyphens\.
   + For **Azure Client Secret**, enter ***your\-azure\-redshift\-application\-client\-secret***\. This is the client secret of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
   + For **Azure Client ID**, enter ***your\-azure\-redshift\-application\-client\-id***\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 

   On Mac OS and Linux, edit the `odbc.ini` file as follows: 
**Note**  
All entries are case\-insensitive\.
   + For **clusterid**, enter ***your\-cluster\-identifier***\. This is the name of the created Amazon Redshift cluster\.
   + For **region**, enter ***your\-cluster\-region***\. This is the AWS Region of the created Amazon Redshift cluster\.
   + For **database**, enter ***your\-database\-name***\. This is the name of the database that you're trying to access on the Amazon Redshift cluster\.
   + For **locale**, enter **en\-us**\. This is the language that error messages display in\.
   + For **iam**, enter **1**\. This value specifies to the driver to authenticate using IAM credentials\.
   + For **plugin\_name**, enter **AzureAD**\. This specifies to the driver to use Azure Single Sign\-On as the authentication method\. 
   + For **uid**, enter ***your\-azure\-username***\. This is the user name of the Microsoft Azure account you are using for single sign\-on that has permission to the cluster you are trying to authenticate against\.
   + For **pwd**, enter ***your\-azure\-password***\. 
   + For **idp\_tenant**, enter ***your\-idp\-tenant***\. This is the tenant name of your company configured on your IdP \(Azure\)\. This value can either be the tenant name or the tenant unique ID with hyphens\.
   + For **client\_secret**, enter ***your\-azure\-redshift\-application\-client\-secret***\. This is the client secret of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
   + For **client\_id**, enter ***your\-azure\-redshift\-application\-client\-id***\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 

   On Mac OS and Linux, also edit the profile settings to add the following exports:

   ```
   export ODBCINI=/opt/amazon/redshift/Setup/odbc.ini
   ```

   ```
   export ODBCINSTINI=/opt/amazon/redshift/Setup/odbcinst.ini
   ```