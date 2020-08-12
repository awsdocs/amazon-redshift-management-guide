# Options for providing IAM credentials<a name="options-for-providing-iam-credentials"></a>

To provide IAM credentials for a JDBC or ODBC connection, choose one of the following options\.
+ **AWS profile** 

  As an alternative to providing credentials values in the form of JDBC or ODBC settings, you can put the values in a named profile\. For more information, see [Using a Configuration Profile](#using-configuration-profile)\.
+ **IAM credentials**

  Provide values for AccessKeyID, SecretAccessKey, and, optionally, SessionToken in the form of JDBC or ODBC settings\. SessionToken is required only for an IAM role with temporary credentials\. For more information, see [JDBC and ODBC options for providing IAM credentials](#jdbc-options-for-providing-iam-credentials)\.
+ **Identity provider federation** 

  When you use identity provider federation to enable users from an identity provider to authenticate to Amazon Redshift, specify the name of a credential provider plugin\. For more information, see [Using a credentials provider plugin](#using-credentials-provider-plugin)\.

  The Amazon Redshift JDBC and ODBC drivers include plugins for the following SAML\-based identity federation credential providers: 
  + Microsoft Active Identity Federation Services \(AD FS\)
  + PingOne
  + Okta
  + Microsoft Azure Active Directory \(Azure AD\)

  You can provide the plugin name and related values in the form of JDBC or ODBC settings or by using a profile\. For more information, see [Configure JDBC driver options](configure-jdbc-connection.md#configure-jdbc-options) and [Configure ODBC driver options](configure-odbc-connection.md#configure-odbc-options)\. 

For more information, see [Configure a JDBC or ODBC connection to use IAM credentials](generating-iam-credentials-configure-jdbc-odbc.md)\.

## Using a Configuration Profile<a name="using-configuration-profile"></a>

You can supply the IAM credentials options and `GetClusterCredentials` options as settings in named profiles in your AWS configuration file\. To provide the profile name, use the Profile JDBC option\. The configuration is stored in a file named `config` in a folder named `.aws` in your home directory\. 

For a SAML\-based credential provider plugin included with an Amazon Redshift JDBC or ODBC driver, you can use the settings described just preceding in [Using a credentials provider plugin](#using-credentials-provider-plugin)\. If `plugin_name` isn't used, the other options are ignored\.

The following example shows a configuration file with two profiles\. 

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
```

To use the credentials for the `user2` example, specify `Profile=user2` in the JDBC URL\. 

For more information on using profiles, see [Named Profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html) in the* AWS Command Line Interface User Guide\.* 

For more information on using profiles for JDBC driver, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.41.1065/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

For more information on using profiles for ODBC driver, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

## JDBC and ODBC options for providing IAM credentials<a name="jdbc-options-for-providing-iam-credentials"></a>

The following table lists the JDBC and ODBC options for providing IAM credentials\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

## Using a credentials provider plugin<a name="using-credentials-provider-plugin"></a>

Amazon Redshift uses credentials provider plugins for SSO authentication\.

To support SSO authentication, Amazon Redshift provides the Azure AD plugin for Microsoft Azure Active Directory\. For information on how to configure this plugin, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\.

### Setting up multi\-factor authentication<a name="setting_mfa"></a>

To support multi\-factor authentication \(MFA\), Amazon Redshift provides browser\-based plugins\. Use the browser SAML plugin for Okta, PingOne, and the browser Azure AD plugin for Microsoft Azure Active Directory\.

With the browser SAML plugin, SAML authentication flows like this:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/BrowserSAML_plugin.png)

1. A user tries to log in\.

1. The plugin launches a local server to listen to incoming connections on the localhost\.

1. The plugin launches a web browser to request a SAML response over HTTPS from the specified SSO login URL federated identity provider endpoint\.

1. The web browser follows the link and prompts the user to enter credentials\.

1. After the user authenticates and grants consent, the federated identity provider endpoint returns a SAML response over HTTPS to the URI indicated by `redirect_uri`\.

1. The web browser moves the response message with the SAML response to the indicated `redirect_uri`\.

1. The local server accepts the incoming connection and the plugin retrieves the SAML response and passes it to Amazon Redshift\.

With the browser Azure AD plugin, SAML authentication flows like this:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/BrowserAzure_plugin.png)

1. A user tries to log in\.

1. The plugin launches a local server to listen to incoming connections on the localhost\.

1. The plugin launches a web browser to request an authorization code from the Azure AD `oauth2/authorize` endpoint\.

1. The web browser follows the generated link over HTTPS and prompts the user to enter credentials\. The link is generated using configuration properties, such as tenant and client\_id\.

1. After the user authenticates and grants consent, the Azure AD `oauth2/authorize` endpoint returns and sends a response over HTTPS with the authorization code to the indicated `redirect_uri`\.

1. The web browser moves the response message with the SAML response to the indicated `redirect_uri`\.

1. The local server accepts the incoming connection and the plugin requests and retrieves the authorization code and sends a POST request to the Azure AD `oauth2/token` endpoint\.

1. The Azure AD `oauth2/token` endpoint returns a response with an access token to the indicated `redirect_uri`\.

1. The plugin retrieves the SAML response and passes it to Amazon Redshift\.

See the following sections:
+ Active Directory Federation Services \(AD FS\)

  For more information, see [Setting up JDBC or ODBC Single Sign\-on authentication with AD FS](#setup-adfs-identity-provider)\.
+ PingOne \(Ping\) 

  Ping is supported only with the predetermined PingOne IdP Adapter using Forms authentication\. 

  For more information, see [Setting up JDBC or ODBC Single Sign\-on authentication with Ping Identity](#setup-pingfederate-identity-provider)\.
+ Okta 

  Okta is supported only for the Okta\-supplied application used with the AWS Management Console\. 

  For more information, see [Setting up JDBC or ODBC Single Sign\-on authentication with Okta](#setup-okta-identity-provider)\.
+ Microsoft Azure Active Directory

  For more information, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\.

### Configuring plugin options<a name="configuring_plugin_options"></a>

To use a SAML\-based credentials provider plugin, specify the following options using JBDC or ODBC options or in a named profile\. If `plugin_name` isn't specified, the other options are ignored\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

## Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD<a name="setup-azure-ad-identity-provider"></a>

You can use Microsoft Azure AD as an identity provider \(IdP\) to access your Amazon Redshift cluster\. Following, you can find a procedure that describes how to set up a trust relationship for this purpose\. For more information about configuring AWS as a service provider for the IdP, see [Configuring Your SAML 2\.0 IdP with Relying Party Trust and Adding Claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html#saml_relying-party) in the *IAM User Guide*\.

**Note**  
To use Azure AD with JDBC, the Amazon Redshift JDBC driver must be version 1\.2\.37\.1061 or later\. To use Azure AD with ODBC, the Amazon Redshift ODBC driver must be version 1\.4\.10\.1000 or later\. 

To learn how to federate Amazon Redshift access with Microsoft Azure AD single sign\-on, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/aXs9hEgJCss/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/aXs9hEgJCss)

**To set up Azure AD and your AWS account to trust each other**

1. Create or use an existing Amazon Redshift cluster for your Azure AD users to connect to\. To configure the connection, certain properties of this cluster are needed, such as the cluster identifier\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)\.

1. Set up an Azure Active Directory, groups, users used for AWS on the Microsoft Azure portal\.

1. Add Amazon Redshift as an enterprise application on the Microsoft Azure portal to use for single sign\-on to the AWS Console and federated login to Amazon Redshift\. Choose **Enterprise application**\.

1. Choose **\+New application**\. The Add an application page appears\.

1. Search for **AWS** in the search field\.

1. Choose **Amazon Web Services \(AWS\)** and choose **Add**\. This creates the AWS application\.

1. Under **Manage**, choose **Single sign\-on**\.

1. Choose **SAML**\. The Amazon Web Services \(AWS\) \| SAML\-based Sign\-on page appears\.

1. Choose **Yes** to proceed to the Set up Single Sign\-On with SAML page\. This page shows the list of pre\-configured AWS SSO\-related attributes\.

1. For **Basic SAML Configuration**, choose the edit icon and choose **Save**\.

1. When you are configuring for more than one application, provide an identifier value\. For example, enter ***https://signin\.aws\.amazon\.com/saml\#2***\. Note that from the second application onwards, use this format with a \# sign to specify a unique SPN value\.

1. In the **User Attributes and Claims** section, choose the edit icon\.

   By default, the Unique User Identifier \(UID\), Role, RoleSessionName, and SessionDuration claims are pre\-configured\.

1. Choose **\+ Add new claim** to add a claim for database users\.

   For **Name**, enter **DbUser**\.

   For **Namespace**, enter **https://redshift\.amazon\.com/SAML/Attributes**\.

   For **Source**, choose **Attribute**\.

   For **Source attribute**, choose **user\.userprincipalname**\. Then, choose **Save**\.

1. Choose **\+ Add new claim** to add a claim for AutoCreate\.

   For **Name**, enter **AutoCreate**\.

   For **Namespace**, enter **https://redshift\.amazon\.com/SAML/Attributes**\.

   For **Source**, choose **Attribute**\.

   For **Source attribute**, choose **"true"**\. Then, choose **Save**\.

   Here, `123456789012` is your AWS account, *`AzureSSO`* is an IAM role you created, and *`AzureADProvider`* is the IAM provider\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

1. Under **App Registration > ***your\-application\-name*** > Authentication**, add **Mobile And Desktop Application**\. Specify the URL as http://localhost/redshift/\.

1. In the **SAML Signing Certificate** section, choose **Download** to download and save the federation metadata XML file for use when you create an IAM SAML identity provider\. This file is used to create the AWS SSO federated identity\.

1. Create an IAM SAML identity provider on the IAM console\. The metadata document that you provide is the federation metadata XML file that you saved when you set up Azure Enterprise Application\. For detailed steps, see [Creating and Managing an IAM Identity Provider \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console) in the *IAM User Guide*\. 

1. Create an IAM role for SAML 2\.0 federation on the IAM console\. For detailed steps, see [ Creating a Role for SAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html#idp_saml_Create) in the *IAM User Guide*\. 

1. Create an IAM policy that you can attach to the IAM role that you created for SAML 2\.0 federation on the IAM console\. For detailed steps, see [Creating IAM Policies \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\.

   Modify the following policy \(in JSON format\) for your environment: 
   + Substitute the AWS Region of your cluster for `us-west-1`\. 
   + Substitute your AWS account for *`123456789012`*\. 
   + Substitute your cluster identifier \(or `*` for all clusters\) for *`cluster-identifier`*\. 
   + Substitute your database \(or `*` for all databases\) for *`dev`*\. 
   + Substitute the unique identifier of your IAM role for *`AROAJ2UCCR6DPCEXAMPLE`*\. 
   + Substitute your tenant or company email domain for `example.com`\. 
   + Substitute the database group that you plan to assign the user to for *`my_dbgroup`*\. 

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
   + The condition clause enforces that only certain users get temporary credentials\. These are users under the role specified by the role unique ID *`AROAJ2UCCR6DPCEXAMPLE`* in the IAM account identified by an email address in your company's email domain\. For more information about unique IDs, see [Unique IDs](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-unique-ids) in the *IAM User Guide*\. 

     Your setup with your IdP \(in this case, Azure AD\) determines how the condition clause is written\. If your employee's email is `johndoe@example.com`, first set `${redshift:DbUser}` to the super field that matches the employee's user name `johndoe`\. Then, to make this condition work, set the AWS SAML `RoleSessionName` field to the super field that matches the employee’s email `johndoe@example.com`\. When you take this approach, consider the following:
     + If you set `${redshift:DbUser}` to be the employee's email, then remove the `@example.com` in the example JSON to match the `RoleSessionName`\. 
     + If you set the `RoleSessionId` to be just the employee's user name, then remove the `@example.com` in the example to match the `RoleSessionName`\. 
     + In the example JSON, the `${redshift:DbUser}` and `RoleSessionName` are both set to the employee's email\. This example JSON uses the Amazon Redshift database user name with `@example.com` to sign the user in to access the cluster\.
   + The second section grants permission to create a `dbuser` name in the specified cluster\. In this example JSON, it restricts creation to `${redshift:DbUser}`\. 
   + The third section grants permission to specify which `dbgroup` a user can join\. In this example JSON, a user can join the `my_dbgroup` group in the specified cluster\. 
   + The fourth section grants permission to actions the user can do on all resources\. In this example JSON, it allows users to call `redshift:DescribeClusters` to get cluster information such as the cluster endpoint, AWS Region, and port\. It also allows users to call `iam:ListRoles` to check which roles a user can assume\. 

**To set up JDBC for authentication to Microsoft Azure AD**
+ Configure your database client to connect to your cluster through JDBC using your Azure AD single sign\-on\. 

  You can use any client that uses a JDBC driver to connect using Azure AD single sign\-on or use a language like Java to connect using a script\. For installation and configuration information, see [Configuring a JDBC connection](configure-jdbc-connection.md)\.

  For example, you can use SQLWorkbench/J as the client\. When you configure SQLWorkbench/J, the URL of your database uses the following format\.

  ```
  jdbc:redshift:iam://cluster-identifier:us-west-1/dev
  ```

  If you use SQLWorkbench/J as the client, take the following steps:

  1. Start SQL Workbench/J\. On the **Select Connection Profile** page, add a **Profile Group** called **AzureAuth**\.

  1. For **Connection Profile**, enter **Azure**\.

  1. Choose **Manage Drivers**, and choose **Amazon Redshift**\. Choose the **Open Folder** icon next to **Library**, then choose the appropriate JDBC \.jar file\. 

  1. On the **Select Connection Profile** page, add information to the connection profile as follows:
     + For **User**, enter your Microsoft Azure user name\. This is the user name of the Microsoft Azure account that you are using for single sign\-on that has permission to the cluster that you are trying to authenticate using\.
     + For **Password**, enter your Microsoft Azure password\.
     + For **Drivers**, choose **Amazon Redshift \(com\.amazon\.redshift\.jdbc\.Driver\)**\.
     + For **URL**, enter **jdbc:redshift:iam//*your\-cluster\-identifier*:*your\-cluster\-region*/*your\-database\-name***\.

  1. Choose **Extended Properties** to add additional information to the connection properties, as described following\.

     For Azure AD SSO configuration, add additional information as follows:
     + For **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.AzureCredentialsProvider**\. This value specifies to the driver to use Azure AD Single Sign\-On as the authentication method\. 
     + For **idp\_tenant**, enter ***your\-idp\-tenant***\. Used only for Microsoft Azure AD\. This is the tenant name of your company configured on Azure AD\. This value can either be the tenant name or the tenant unique ID with hyphens\.
     + For **client\_secret**, enter ***your\-azure\-redshift\-application\-client\-secret***\. Used only for Microsoft Azure AD\. This is your client secret of the Amazon Redshift application that you created when setting up your Azure Single Sign\-On configuration\. This is only applicable to the com\.amazon\.redshift\.plugin\.AzureCredentialsProvider plugin\. 
     + For **client\_id**, enter ***your\-azure\-redshift\-application\-client\-id***\. Used only for Microsoft Azure AD\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure Single Sign\-On configuration\. 

     For Azure AD SSO with MFA configuration, add additional information to the connection properties as follows:
     + For **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.BrowserAzureCredentialsProvider**\. This value specifies to the driver to use Azure AD SSO with MFA as the authentication method\. 
     + For **idp\_tenant**, enter ***your\-idp\-tenant***\. Used only for Microsoft Azure AD\. This is the tenant name of your company configured on Azure AD\. This value can either be the tenant name or the tenant unique ID with hyphens\.
     + For **client\_id**, enter ***your\-azure\-redshift\-application\-client\-id***\. This option is used only for Microsoft Azure AD\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure AD SSO with MFA configuration\. 
     + For **listen\_port**, enter ***your\-listen\-port***\. This is the port that local server is listening to\. The default is 7890\. 
     + For **idp\_response\_timeout**, enter ***the\-number\-of\-seconds***\. This is the number of seconds to wait before timing out when the IdP server sends back a response\. The minimum number of seconds must be 10\. If establishing the connection takes longer than this threshold, then the connection is aborted\.

**To set up ODBC for authentication to Microsoft Azure AD**
+ Configure your database client to connect to your cluster through ODBC using your Azure AD single sign\-on\. 

  Amazon Redshift provides ODBC drivers for Linux, Windows, and macOS operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. 

  Also install and configure the latest Amazon Redshift OBDC driver for your operating system as follows:
  + For Windows, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.
  + For macOS, see [Install the Amazon Redshift ODBC driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.
  + For Linux, see [Install the Amazon Redshift ODBC driver on Linux](configure-odbc-connection.md#install-odbc-driver-linux)\.

  On Windows, in the **Amazon Redshift ODBC Driver DSN Setup** page, under **Connection Settings**, enter the following information: 
  + For **Data Source Name**, enter ***your\-DSN***\. This specifies the data source name used as the ODBC profile name\. 
  + For **Auth type** for Azure AD SSO configuration, choose **Identity Provider: Azure AD**\. This is the authentication method that the ODBC driver uses to authenticate using Azure single sign\-on\.
  + For **Auth type** for Azure AD SSO with MFA configuration, choose **Identity Provider: Browser Azure AD**\. This is the authentication method that the ODBC driver uses to authenticate using Azure single sign\-on with MFA\.
  + For **Cluster ID**, enter ***your\-cluster\-identifier***\. 
  + For **Region**, enter ***your\-cluster\-region***\.
  + For **Database**, enter ***your\-database\-name***\.
  + For **User**, enter ***your\-azure\-username***\. This is the user name for the Microsoft Azure account that you are using for single sign\-on that has permission to the cluster that you're trying to authenticate using\. Use this only for **Auth Type** is **Identity Provider: Azure AD**\.
  + For **Password**, enter ***your\-azure\-password***\. Use this only for **Auth Type** is **Identity Provider: Azure AD**\. 
  + For **IdP Tenant**, enter ***your\-idp\-tenant***\. This is the tenant name of your company configured on your IdP \(Azure\)\. This value can either be the tenant name or the tenant unique ID with hyphens\.
  + For **Azure Client Secret**, enter ***your\-azure\-redshift\-application\-client\-secret***\. This is the client secret of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
  + For **Azure Client ID**, enter ***your\-azure\-redshift\-application\-client\-id***\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
  + For **Listen Port**, enter ***your\-listen\-port***\. This is the default listen port that local server is listening to\. The default is 7890\. This applies only to the Browser Azure AD plugin\. 
  + For **Response Timeout**, enter ***the\-number\-of\-seconds***\. This is the number of seconds to wait before timing out when the IdP server sends back a response\. The minimum number of seconds must be 10\. If establishing the connection takes longer than this threshold, then the connection is aborted\. This option applies only to the Browser Azure AD plugin\.

  On macOS and Linux, edit the `odbc.ini` file as follows: 
**Note**  
All entries are case\-insensitive\.
  + For **clusterid**, enter ***your\-cluster\-identifier***\. This is the name of the created Amazon Redshift cluster\.
  + For **region**, enter ***your\-cluster\-region***\. This is the AWS Region of the created Amazon Redshift cluster\.
  + For **database**, enter ***your\-database\-name***\. This is the name of the database that you're trying to access on the Amazon Redshift cluster\.
  + For **locale**, enter **en\-us**\. This is the language that error messages display in\.
  + For **iam**, enter **1**\. This value specifies to the driver to authenticate using IAM credentials\.
  + For **plugin\_name** for Azure AD SSO configuration, enter **AzureAD**\. This specifies to the driver to use Azure Single Sign\-On as the authentication method\. 
  + For **plugin\_name** for Azure AD SSO with MFA configuration, enter **BrowserAzureAD**\. This specifies to the driver to use Azure Single Sign\-On with MFA as the authentication method\. 
  + For **uid**, enter ***your\-azure\-username***\. This is the user name of the Microsoft Azure account you are using for single sign\-on that has permission to the cluster you are trying to authenticate against\. Use this only for **plugin\_name** is **AzureAD**\.
  + For **pwd**, enter ***your\-azure\-password***\. Use this only for **plugin\_name** is **AzureAD**\. 
  + For **idp\_tenant**, enter ***your\-idp\-tenant***\. This is the tenant name of your company configured on your IdP \(Azure\)\. This value can either be the tenant name or the tenant unique ID with hyphens\.
  + For **client\_secret**, enter ***your\-azure\-redshift\-application\-client\-secret***\. This is the client secret of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
  + For **client\_id**, enter ***your\-azure\-redshift\-application\-client\-id***\. This is the client ID \(with hyphens\) of the Amazon Redshift application that you created when setting up your Azure single sign\-on configuration\. 
  + For **listen\_port**, enter ***your\-listen\-port***\. This is the port that local server is listening to\. The default is 7890\. This applies to the Browser Azure AD plugin\.
  + For **idp\_response\_timeout**, enter ***the\-number\-of\-seconds***\. This is the specified period of time in seconds to wait for response from Azure\. This option applies to the Browser Azure AD plugin\.

  On macOS and Linux, also edit the profile settings to add the following exports\.

  ```
  export ODBCINI=/opt/amazon/redshift/Setup/odbc.ini
  ```

  ```
  export ODBCINSTINI=/opt/amazon/redshift/Setup/odbcinst.ini
  ```

**To troubleshoot issues with the Browser Azure AD plugin**

1. To use the Browser Azure AD plugin, you must set the reply URL specified in the request to match the reply URL configured for your application\.

   Navigate to the **Set up Single Sign\-On with SAML** page on the Microsoft Azure portal\. Then check the **Reply URL** is set to http://localhost/redshift/\.

1. If you get an IdP tenant error, verify that the **IdP Tenant** name matches the domain name you initially used to set up the Active Directory in Microsoft Azure\.

   On Windows, navigate to the **Connection Settings** section of the **Amazon Redshift ODBC DSN Setup** page\. Then check the tenant name of your company configured on your IdP \(Azure\) matches the domain name you initially used to set up the Active Directory in Microsoft Azure\.

   On macOS and Linux, find the *odbc\.ini* file\. Then check the tenant name of your company configured on your IdP \(Azure\) matches the domain name you initially used to set up the Active Directory in Microsoft Azure\.

1. If you get an error that the reply URL specified in the request does not match the reply URLs configured for your application, verify that the **Redirect URIs** is the same as the reply URL\.

   Navigate to the **App registration** page of your application on the Microsoft Azure portal\. Then check the Redirect URIs matches the reply URL\.

1. If you get the unexpected response: unauthorized error, verify that you completed the **Mobile and desktop applications** configuration\.

   Navigate to the ** App registration** page of your application on the Microsoft Azure portal\. Then navigate to **Authentication** and check that you configured **Mobile and desktop applications** to use http://localhost/redshift/ as the redirect URIs\.

## Setting up JDBC or ODBC Single Sign\-on authentication with AD FS<a name="setup-adfs-identity-provider"></a>

You can use AD FS as an identity provider \(IdP\) to access your Amazon Redshift cluster\. Following, you can find a procedure that describes how to set up a trust relationship for this purpose\. For more information about configuring AWS as a service provider for AD FS, see [Configuring Your SAML 2\.0 IdP with Relying Party Trust and Adding Claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html#saml_relying-party) in the *IAM User Guide*\.

**To set up AD FS and your AWS account to trust each other**

1. Create or use an existing Amazon Redshift cluster for your AD FS users to connect to\. To configure the connection, certain properties of this cluster are needed, such as the cluster identifier\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)\.

1. Set up AD FS to control Amazon Redshift access on the Microsoft Management Console: 

   1. Choose **ADFS 2\.0**, and then choose **Add Relying Party Trust**\. On the **Add Relying Party Trust Wizard** page, choose **Start**\.

   1. On the **Select Data Source** page, choose **Import data about the relying party published online or on a local network**\.

   1. For **Federation metadata address \(host name or URL\)**, enter `https://signin.aws.amazon.com/saml-metadata.xml`\. The metadata XML file is a standard SAML metadata document that describes AWS as a relying party\.

   1. On the **Specify Display Name** page, enter a value for **Display name**\. 

   1. On the **Choose Issuance Authorization Rules** page, choose an issuance authorization rule to either permit or deny all users to access this relying party\.

   1. On the **Ready to Add Trust** page, review your settings\.

   1. On the **Finish** page, choose **Open the Edit Claim Rules dialog for this relying party trust when the wizard closes**\.

   1. On the context \(right\-click\) menu, choose **Relying Party Trusts**\.

   1. For your relying party, open the context \(right\-click\) menu and choose **Edit Claim Rules**\. On the **Edit Claim Rules** page, choose **Add Rule**\.

   1. For **Claim rule template**, choose **Transform an Incoming Claim**, and then on the **Edit Rule – NameId **page, do the following:
      + For **Claim rule name**, enter **NameId**\.
      + For **Incoming claim name**, choose **Windows Account Name**\.
      + For **Outgoing claim name**, choose **Name ID**\.
      + For **Outgoing name ID format**, choose **Persistent Identifier**\.
      + Choose **Pass through all claim values**\.

   1. On the **Edit Claim Rules** page, choose **Add Rule**\. On the **Select Rule Template** page, for **Claim rule template**, choose **Send LDAP Attributes as Claims**\.

   1. On the **Configure Rule** page, do the following:
      + For **Claim rule name**, enter **RoleSessionName**\.
      + For **Attribute store**, choose **Active Directory**\.
      + For **LDAP Attribute**, choose **Email Addresses**\.
      + For **Outgoing Claim Type**, choose **https://aws\.amazon\.com/SAML/Attributes/RoleSessionName**\.

   1. On the **Edit Claim Rules** page, choose **Add Rule**\. On the **Select Rule Template** page, for **Claim rule template**, choose **Send Claims Using a Custom Rule**\.

   1. On the **Edit Rule – Get AD Groups** page, for **Claim rule name**, enter **Get AD Groups**\.

   1. For **Custom rule**, enter the following\.

      ```
      c:[Type ==
                                          "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",
                                          Issuer == "AD AUTHORITY"] => add(store = "Active Directory",
                                          types = ("http://temp/variable"), query = ";tokenGroups;{0}",
                                          param = c.Value);
      ```

   1. On the **Edit Claim Rules** page, choose **Add Rule**\. On the **Select Rule Template** page, for **Claim rule template**, choose **Send Claims Using a Custom Rule**\.

   1. On the **Edit Rule – Roles** page, for **Claim rule name**, type **Roles**\.

   1. For **Custom rule,** enter the following\.

      ```
      c:[Type == "http://temp/variable", Value =~ "(?i)^AWS-"] => issue(Type = "https://aws.amazon.com/SAML/Attributes/Role", Value = RegExReplace(c.Value, "AWS-", "arn:aws:iam::123456789012:saml-provider/ADFS,arn:aws:iam::123456789012:role/ADFS-"));
      ```

      Note the ARNs of the SAML provider and role to assume\. In this example, `arn:aws:iam:123456789012:saml-provider/ADFS` is the ARN of the SAML provider and `arn:aws:iam:123456789012:role/ADFS-` is the ARN of the role\.

1. Make sure that you have downloaded the `federationmetadata.xml` file\. Check that the document contents do not have invalid characters\. This is the metadata file you use when configuring the trust relationship with AWS\. 

1. Create an IAM SAML identity provider on the IAM console\. The metadata document\. that you provide is the federation metadata XML file that you saved when you set up Azure Enterprise Application\. For detailed steps, see [ Creating and Managing an IAM Identity Provider \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console) in the *IAM User Guide*\. 

1. Create an IAM role for SAML 2\.0 federation on the IAM console\. For detailed steps, see [Creating a Role for SAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html#idp_saml_Create) in the *IAM User Guide*\. 

1. Create an IAM policy that you can attach to the IAM role that you created for SAML 2\.0 federation on the IAM console\. For detailed steps, see [Creating IAM Policies \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. For an Azure AD example, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\. 

**To set up JDBC for authentication to AD FS**
+ Configure your database client to connect to your cluster through JDBC using AD FS SSO\. 

  You can use any client that uses a JDBC driver to connect using AD FS SSO or use a language like Java to connect using a script\. For installation and configuration information, see [Configuring a JDBC connection](configure-jdbc-connection.md)\.

  For example, you can use SQLWorkbench/J as the client\. When you configure SQLWorkbench/J, the URL of your database uses the following format\.

  ```
  jdbc:redshift:iam://cluster-identifier:us-west-1/dev
  ```

  If you use SQLWorkbench/J as the client, take the following steps:

  1. Start SQL Workbench/J\. In the **Select Connection Profile** page, add a **Profile Group**, for example **ADFS**\.

  1. For **Connection Profile**, enter your connection profile name, for example **ADFS**\.

  1. Choose **Manage Drivers**, and choose **Amazon Redshift**\. Choose the **Open Folder** icon next to **Library**, then choose the appropriate JDBC \.jar file\. 

  1. On the **Select Connection Profile** page, add information to the connection profile as follows:
     + For **User**, enter your AD FS user name\. This is the user name of the account that you are using for single sign\-on that has permission to the cluster that you are trying to authenticate using\.
     + For **Password**, enter your AD FS password\.
     + For **Drivers**, choose **Amazon Redshift \(com\.amazon\.redshift\.jdbc\.Driver\)**\.
     + For **URL**, enter **jdbc:redshift:iam//*your\-cluster\-identifier*:*your\-cluster\-region*/*your\-database\-name***\.

  1. Choose **Extended Properties**\. For **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.AdfsCredentialsProvider**\. This value specifies to the driver to use AD FS SSO as the authentication method\. 

**To set up ODBC for authentication to AD FS**
+ Configure your database client to connect to your cluster through ODBC using AD FS SSO\. 

  Amazon Redshift provides ODBC drivers for Linux, Windows, and macOS operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. 

  Also install and configure the latest Amazon Redshift OBDC driver for your operating system as follows:
  + For Windows, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.
  + For macOS, see [Install the Amazon Redshift ODBC driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.
  + For Linux, see [Install the Amazon Redshift ODBC driver on Linux](configure-odbc-connection.md#install-odbc-driver-linux)\.

  On Windows, in the **Amazon Redshift ODBC Driver DSN Setup** page, under **Connection Settings**, enter the following information: 
  + For **Data Source Name**, enter ***your\-DSN***\. This specifies the data source name used as the ODBC profile name\. 
  + For **Auth type**, choose **Identity Provider: SAML**\. This is the authentication method that the ODBC driver uses to authenticate using AD FS SSO\.
  + For **Cluster ID**, enter ***your\-cluster\-identifier***\. 
  + For **Region**, enter ***your\-cluster\-region***\.
  + For **Database**, enter ***your\-database\-name***\.
  + For **User**, enter ***your\-adfs\-username***\. This is the user name for the AD FS account that you are using for SSO that has permission to the cluster that you're trying to authenticate using\. Use this only for **Auth type** is **Identity Provider: SAML**\.
  + For **Password**, enter ***your\-adfs\-password***\. Use this only for **Auth type** is **Identity Provider: SAML**\. 

  On macOS and Linux, edit the `odbc.ini` file as follows: 
**Note**  
All entries are case\-insensitive\.
  + For **clusterid**, enter ***your\-cluster\-identifier***\. This is the name of the created Amazon Redshift cluster\.
  + For **region**, enter ***your\-cluster\-region***\. This is the AWS Region of the created Amazon Redshift cluster\.
  + For **database**, enter ***your\-database\-name***\. This is the name of the database that you're trying to access on the Amazon Redshift cluster\.
  + For **locale**, enter **en\-us**\. This is the language that error messages display in\.
  + For **iam**, enter **1**\. This value specifies to the driver to authenticate using IAM credentials\.
  + For **plugin\_name**, do one of the following:
    + For AD FS SSO with MFA configuration, enter **BrowserSAML**\. This is the authentication method that the ODBC driver uses to authenticate to AD FS\. 
    + For AD FS SSO configuration, enter **ADFS**\. This is the authentication method that the ODBC driver uses to authenticate using Azure AD SSO\. 
  + For **uid**, enter ***your\-adfs\-username***\. This is the user name of the Microsoft Azure account that you are using for SSO that has permission to the cluster you are trying to authenticate against\. Use this only for **plugin\_name** is **ADFS**\.
  + For **pwd**, enter ***your\-adfs\-password***\. Use this only for **plugin\_name** is **ADFS**\. 

  On macOS and Linux, also edit the profile settings to add the following exports\.

  ```
  export ODBCINI=/opt/amazon/redshift/Setup/odbc.ini
  ```

  ```
  export ODBCINSTINI=/opt/amazon/redshift/Setup/odbcinst.ini
  ```

## Setting up JDBC or ODBC Single Sign\-on authentication with Ping Identity<a name="setup-pingfederate-identity-provider"></a>

You can use Ping Identity as an identity provider \(IdP\) to access your Amazon Redshift cluster\. Following, you can find a procedure that describes how to set up a trust relationship for this purpose\. For more information about configuring AWS as a service provider for Ping Identity, see [Configuring Your SAML 2\.0 IdP with Relying Party Trust and Adding Claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html#saml_relying-party) in the *IAM User Guide*\.

**To set up Ping Identity and your AWS account to trust each other**

1. Create or use an existing Amazon Redshift cluster for your Ping Identity users to connect to\. To configure the connection, certain properties of this cluster are needed, such as the cluster identifier\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)\.

1. Add Amazon Redshift as a new SAML application on the PingOne portal\. For detailed steps, see the [Ping Identity documentation](https://docs.pingidentity.com/)\. 

   1. Go to **My Applications**\.

   1. Under **Add Application**, choose **New SAML Application**\.

   1. For **Application Name**, enter **Amazon Redshift**\.

   1. For **Protocol Version**, choose **SAML v2\.0**\.

   1. For **Category**, choose ***your\-application\-category***\.

   1. For **Assertion Consumer Service \(ACS\)**, type ***your\-redshift\-local\-host\-url***\. This is the local host and port that the SAML assertion redirects to\.

   1. For **Entity ID**, enter `urn:amazon:webservices`\.

   1. For **Signing**, choose **Sign Assertion**\.

   1. In the **SSO Attribute Mapping** section, create the claims as shown in the following table\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

1. For **Group Access**, set up the following group access, if needed:
   + `https://aws.amazon.com/SAML/Attributes/Role`
   + `https://aws.amazon.com/SAML/Attributes/RoleSessionName`
   + `https://redshift.amazon.com/SAML/Attributes/AutoCreate`
   + `https://redshift.amazon.com/SAML/Attributes/DbUser`

1. Review your setup and make changes, if necessary\. 

1. Use the **Initiate Single Sign\-On \(SSO\) URL** as the login URL for the Browser SAML plugin\.

1. Create an IAM SAML identity provider on the IAM console\. The metadata document that you provide is the federation metadata XML file that you saved when you set up Ping Identity\. For detailed steps, see [ Creating and Managing an IAM Identity Provider \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console) in the *IAM User Guide*\. 

1. Create an IAM role for SAML 2\.0 federation on the IAM console\. For detailed steps, see [ Creating a Role for SAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html#idp_saml_Create) in the *IAM User Guide*\. 

1. Create an IAM policy that you can attach to the IAM role that you created for SAML 2\.0 federation on the IAM console\. For detailed steps, see [Creating IAM Policies \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. For an Azure AD example, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\. 

**To set up JDBC for authentication to Ping Identity**
+ Configure your database client to connect to your cluster through JDBC using Ping Identity SSO\. 

  You can use any client that uses a JDBC driver to connect using Ping Identity SSO or use a language like Java to connect using a script\. For installation and configuration information, see [Configuring a JDBC connection](configure-jdbc-connection.md)\.

  For example, you can use SQLWorkbench/J as the client\. When you configure SQLWorkbench/J, the URL of your database uses the following format\.

  ```
  jdbc:redshift:iam://cluster-identifier:us-west-1/dev
  ```

  If you use SQLWorkbench/J as the client, take the following steps:

  1. Start SQL Workbench/J\. In the **Select Connection Profile** page, add a **Profile Group**, for example **Ping**\.

  1. For **Connection Profile**, enter ***your\-connection\-profile\-name***, for example **Ping**\.

  1. Choose **Manage Drivers**, and choose **Amazon Redshift**\. Choose the **Open Folder** icon next to **Library**, then choose the appropriate JDBC \.jar file\. 

  1. On the **Select Connection Profile** page, add information to the connection profile as follows:
     + For **User**, enter your PingOne user name\. This is the user name of the PingOne account that you are using for single sign\-on that has permission to the cluster that you are trying to authenticate using\.
     + For **Password**, enter your PingOne password\.
     + For **Drivers**, choose **Amazon Redshift \(com\.amazon\.redshift\.jdbc\.Driver\)**\.
     + For **URL**, enter **jdbc:redshift:iam//*your\-cluster\-identifier*:*your\-cluster\-region*/*your\-database\-name***\.

  1. Choose **Extended Properties** and do one of the following:
     + For **login\_url**, enter ***your\-ping\-sso\-login\-url***\. This value specifies to the URL to use SSO as the authentication to log in\. 
     + For Ping Identity, for **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.PingCredentialsProvider**\. This value specifies to the driver to use Ping Identity SSO as the authentication method\. 
     + For Ping Identity with SSO, for **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.BrowserSamlCredentialsProvider**\. This value specifies to the driver to use Ping Identity PingOne with SSO as the authentication method\. 

**To set up ODBC for authentication to Ping Identity**
+ Configure your database client to connect to your cluster through ODBC using Ping Identity PingOne SSO\. 

  Amazon Redshift provides ODBC drivers for Linux, Windows, and macOS operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. 

  Also install and configure the latest Amazon Redshift OBDC driver for your operating system as follows:
  + For Windows, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.
  + For macOS, see [Install the Amazon Redshift ODBC driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.
  + For Linux, see [Install the Amazon Redshift ODBC driver on Linux](configure-odbc-connection.md#install-odbc-driver-linux)\.

  On Windows, in the **Amazon Redshift ODBC Driver DSN Setup** page, under **Connection Settings**, enter the following information: 
  + For **Data Source Name**, enter ***your\-DSN***\. This specifies the data source name used as the ODBC profile name\. 
  + For **Auth type**, do one of the following:
    + For Ping Identity configuration, choose **Identity Provider: Ping Federate**\. This is the authentication method that the ODBC driver uses to authenticate using Ping Identity SSO\.
    + For Ping Identity with SSO configuration, choose **Identity Provider: Browser SAML**\. This is the authentication method that the ODBC driver uses to authenticate using Ping Identity with SSO\.
  + For **Cluster ID**, enter ***your\-cluster\-identifier***\. 
  + For **Region**, enter ***your\-cluster\-region***\.
  + For **Database**, enter ***your\-database\-name***\.
  + For **User**, enter ***your\-ping\-username***\. This is the user name for the PingOne account that you are using for SSO that has permission to the cluster that you're trying to authenticate using\. Use this only for **Auth type** is **Identity Provider: PingFederate**\.
  + For **Password**, enter ***your\-ping\-password***\. Use this only for **Auth type** is **Identity Provider: PingFederate**\. 
  + For **Listen Port**, enter ***your\-listen\-port***\. This is the port that local server is listening to\. The default is 7890\. This applies only to the Browser SAML plugin\. 
  +  For **Response Timeout**, enter ***the\-number\-of\-seconds***\. This is the number of seconds to wait before timing out when the IdP server sends back a response\. The minimum number of seconds must be 10\. If establishing the connection takes longer than this threshold, then the connection is aborted\. This applies only to the Browser SAML plugin\.
  + For **Login URL**, enter ***your\-login\-url***\. This applies only to the Browser SAML plugin\.

  On macOS and Linux, edit the `odbc.ini` file as follows: 
**Note**  
All entries are case\-insensitive\.
  + For **clusterid**, enter ***your\-cluster\-identifier***\. This is the name of the created Amazon Redshift cluster\.
  + For **region**, enter ***your\-cluster\-region***\. This is the AWS Region of the created Amazon Redshift cluster\.
  + For **database**, enter ***your\-database\-name***\. This is the name of the database that you're trying to access on the Amazon Redshift cluster\.
  + For **locale**, enter **en\-us**\. This is the language that error messages display in\.
  + For **iam**, enter **1**\. This value specifies to the driver to authenticate using IAM credentials\.
  + For **plugin\_name**, do one of the following:
    + For Ping Identity configuration, enter **BrowserSAML**\. This is the authentication method that the ODBC driver uses to authenticate to Ping Identity\. 
    + For Ping Identity with SSO configuration, enter **Ping**\. This is the authentication method that the ODBC driver uses to authenticate using Ping Identity with SSO\. 
  + For **uid**, enter ***your\-ping\-username***\. This is the user name of the Microsoft Azure account you are using for SSO that has permission to the cluster you are trying to authenticate against\. Use this only for **plugin\_name** is **Ping**\.
  + For **pwd**, enter ***your\-ping\-password***\. Use this only for **plugin\_name** is **Ping**\. 
  + For **login\_url**, enter ***your\-login\-url***\. This is the Initiate SSO URL that returns the SAML Response\. This applies only to the Browser SAML plugin\.
  + For **idp\_response\_timeout**, enter ***the\-number\-of\-seconds***\. This is the specified period of time in seconds to wait for response from PingOne Identity\. This applies only to the Browser SAML plugin\.
  + For **listen\_port**, enter ***your\-listen\-port***\. This is the port that local server is listening to\. The default is 7890\. This applies only to the Browser SAML plugin\.

  On macOS and Linux, also edit the profile settings to add the following exports\.

  ```
  export ODBCINI=/opt/amazon/redshift/Setup/odbc.ini
  ```

  ```
  export ODBCINSTINI=/opt/amazon/redshift/Setup/odbcinst.ini
  ```

## Setting up JDBC or ODBC Single Sign\-on authentication with Okta<a name="setup-okta-identity-provider"></a>

You can use Okta as an identity provider \(IdP\) to access your Amazon Redshift cluster\. Following, you can find a procedure that describes how to set up a trust relationship for this purpose\. For more information about configuring AWS as a service provider for Okta, see [Configuring Your SAML 2\.0 IdP with Relying Party Trust and Adding Claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html#saml_relying-party) in the *IAM User Guide*\.

**To set up Okta and your AWS account to trust each other**

1. Create or use an existing Amazon Redshift cluster for your Okta users to connect to\. To configure the connection, certain properties of this cluster are needed, such as the cluster identifier\. For more information, see [Creating a Cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster)\.

1. Add Amazon Redshift as a new application on the Okta portal\. For detailed steps, see the [Okta documentation](https://developer.okta.com/docs/)\. 
   + Choose **Add Application**\.
   + Under **Add Application**, choose **Create New App**\.
   + On the **Create a New Add Application Integration** page, for **Platform**, choose **Web**\.
   + For **Sign on method**, choose **SAML v2\.0**\.
   + On the **General Settings** page, for **App name**, enter ***your\-redshift\-saml\-sso\-name***\. This is the name of your application\.
   + On the **SAML Settings** page, for **Single sign on URL**, enter ***your\-redshift\-local\-host\-url***\. This is the local host and port that the SAML assertion redirects to, for example `http://localhost:7890/redshift`\.

1. Use the **Single sign on URL** value as the **Recipient URL** and **Destination URL**\.

1. For **Signing**, choose **Sign Assertion**\.

1. For **Audience URI \(SP Entity ID\)**, enter **urn:amazon:webservices** for the claims, as shown in the following table\. 

1. In the **Advanced Settings** section, for **SAML Issuer ID**, enter ***your\-Identity\-Provider\-Issuer\-ID***, which you can find in the **View Setup Instructions** section\.

1. In the **Attribute Statements** section, create the claims as shown in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html)

1. In the **App Embed Link** section, find the URL that you can use as the login URL for the Browser SAML plugin\.

1. Create an IAM SAML identity provider on the IAM console\. The metadata document that you provide is the federation metadata XML file that you saved when you set up Okta\. For detailed steps, see [ Creating and Managing an IAM Identity Provider \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml.html#idp-manage-identityprovider-console) in the *IAM User Guide*\. 

1. Create an IAM role for SAML 2\.0 federation on the IAM console\. For detailed steps, see [ Creating a Role for SAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html#idp_saml_Create) in the *IAM User Guide*\. 

1. Create an IAM policy that you can attach to the IAM role that you created for SAML 2\.0 federation on the IAM console\. For detailed steps, see [Creating IAM Policies \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the *IAM User Guide*\. For an Azure AD example, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](#setup-azure-ad-identity-provider)\. 

**To set up JDBC for authentication to Okta**
+ Configure your database client to connect to your cluster through JDBC using Okta SSO\. 

  You can use any client that uses a JDBC driver to connect using Okta SSO or use a language like Java to connect using a script\. For installation and configuration information, see [Configuring a JDBC connection](configure-jdbc-connection.md)\.

  For example, you can use SQLWorkbench/J as the client\. When you configure SQLWorkbench/J, the URL of your database uses the following format\.

  ```
  jdbc:redshift:iam://cluster-identifier:us-west-1/dev
  ```

  If you use SQLWorkbench/J as the client, take the following steps:

  1. Start SQL Workbench/J\. In the **Select Connection Profile** page, add a **Profile Group**, for example **Okta**\.

  1. For **Connection Profile**, enter ***your\-connection\-profile\-name***, for example **Okta**\.

  1. Choose **Manage Drivers**, and choose **Amazon Redshift**\. Choose the **Open Folder** icon next to **Library**, then choose the appropriate JDBC \.jar file\. 

  1. On the **Select Connection Profile** page, add information to the connection profile as follows:
     + For **User**, enter your Okta user name\. This is the user name of the Okta account that you are using for single sign\-on that has permission to the cluster that you are trying to authenticate using\.
     + For **Password**, enter your Okta password\.
     + For **Drivers**, choose **Amazon Redshift \(com\.amazon\.redshift\.jdbc\.Driver\)**\.
     + For **URL**, enter **jdbc:redshift:iam//*your\-cluster\-identifier*:*your\-cluster\-region*/*your\-database\-name***\.

  1. Choose **Extended Properties** and do one of the following:
     + For **login\_url**, enter ***your\-okta\-sso\-login\-url***\. This value specifies to the URL to use SSO as the authentication to log in to Okta\. 
     + For Okta SSO, for **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.OktaCredentialsProvider**\. This value specifies to the driver to use Okta SSO as the authentication method\. 
     + For Okta SSO with MFA, for **plugin\_name**, enter **com\.amazon\.redshift\.plugin\.BrowserSamlCredentialsProvider**\. This value specifies to the driver to use Okta SSO with MFA as the authentication method\. 

**To set up ODBC for authentication to Okta**
+ Configure your database client to connect to your cluster through ODBC using Okta SSO\. 

  Amazon Redshift provides ODBC drivers for Linux, Windows, and macOS operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. 

  Also install and configure the latest Amazon Redshift OBDC driver for your operating system as follows:
  + For Windows, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.
  + For macOS, see [Install the Amazon Redshift ODBC driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.
  + For Linux, see [Install the Amazon Redshift ODBC driver on Linux](configure-odbc-connection.md#install-odbc-driver-linux)\.

  On Windows, in the **Amazon Redshift ODBC Driver DSN Setup** page, under **Connection Settings**, enter the following information: 
  + For **Data Source Name**, enter ***your\-DSN***\. This specifies the data source name used as the ODBC profile name\. 
  + For **Auth type**, do one of the following:
    + For Okta SSO configuration, choose **Identity Provider: Okta**\. This is the authentication method that the ODBC driver uses to authenticate using Okta SSO\.
    + For Okta SSO with MFA configuration, choose **Identity Provider: Browser SAML**\. This is the authentication method that the ODBC driver uses to authenticate using Okta SSO with MFA\.
  + For **Cluster ID**, enter ***your\-cluster\-identifier***\. 
  + For **Region**, enter ***your\-cluster\-region***\.
  + For **Database**, enter ***your\-database\-name***\.
  + For **User**, enter ***your\-okta\-username***\. This is the user name for the Okta account that you are using for SSO that has permission to the cluster that you're trying to authenticate using\. Use this only for **Auth type** is **Identity Provider: Okta**\.
  + For **Password**, enter ***your\-okta\-password***\. Use this only for **Auth type** is **Identity Provider: Okta**\. 

  On macOS and Linux, edit the `odbc.ini` file as follows: 
**Note**  
All entries are case\-insensitive\.
  + For **clusterid**, enter ***your\-cluster\-identifier***\. This is the name of the created Amazon Redshift cluster\.
  + For **region**, enter ***your\-cluster\-region***\. This is the AWS Region of the created Amazon Redshift cluster\.
  + For **database**, enter ***your\-database\-name***\. This is the name of the database that you're trying to access on the Amazon Redshift cluster\.
  + For **locale**, enter **en\-us**\. This is the language that error messages display in\.
  + For **iam**, enter **1**\. This value specifies to the driver to authenticate using IAM credentials\.
  + For **plugin\_name**, do one of the following:
    + For Okta SSO with MFA configuration, enter **BrowserSAML**\. This is the authentication method that the ODBC driver uses to authenticate to Okta SSO with MFA\. 
    + For Okta SSO configuration, enter **Okta**\. This is the authentication method that the ODBC driver uses to authenticate using Okta SSO\. 
  + For **uid**, enter ***your\-okta\-username***\. This is the user name of the Okta account you are using for SSO that has permission to the cluster you are trying to authenticate against\. Use this only for **plugin\_name** is **Okta**\.
  + For **pwd**, enter ***your\-okta\-password***\. Use this only for **plugin\_name** is **Okta**\. 
  + For **login\_url**, enter ***your\-login\-url***\. This is the Initiate SSO URL that returns the SAML Response\. This applies only to the Browser SAML plugin\.
  + For **idp\_response\_timeout**, enter ***the\-number\-of\-seconds***\. This is the specified period of time in seconds to wait for response from PingOne\. This applies only to the Browser SAML plugin\.
  + For **listen\_port**, enter ***your\-listen\-port***\. This is the port that local server is listening to\. The default is 7890\. This applies only to the Browser SAML plugin\.

  On macOS and Linux, also edit the profile settings to add the following exports\.

  ```
  export ODBCINI=/opt/amazon/redshift/Setup/odbc.ini
  ```

  ```
  export ODBCINSTINI=/opt/amazon/redshift/Setup/odbcinst.ini
  ```