# Step 5: Configure a JDBC or ODBC connection to use IAM credentials<a name="generating-iam-credentials-configure-jdbc-odbc"></a>

You can configure your SQL client with an Amazon Redshift JDBC or ODBC driver\. This driver manages the process of creating database user credentials and establishing a connection between your SQL client and your Amazon Redshift database\. 

If you use an identity provider for authentication, specify the name of a credential provider plugin\. The Amazon Redshift JDBC and ODBC drivers include plugins for the following SAML\-based identity providers: 
+ Active Directory Federation Services \(AD FS\) 
+ PingOne
+ Okta
+ Microsoft Azure AD

  For the steps to set up Microsoft Azure AD as an identity provider, see [Setting up JDBC or ODBC single sign\-on authentication with Microsoft Azure AD](options-for-providing-iam-credentials.md#setup-azure-ad-identity-provider)\. <a name="to-configure-a-jdbc-connection"></a>

**To configure a JDBC connection to use IAM credentials**

1. Download the latest Amazon Redshift JDBC driver from the [Configuring a JDBC connection](configure-jdbc-connection.md) page\.
**Important**  
The Amazon Redshift JDBC driver must be version 1\.2\.7\.1003 or later\.

1. Create a JDBC URL with the IAM credentials options in one of the following formats\. To use IAM authentication, add `iam:` to the Amazon Redshift JDBC URL following `jdbc:redshift:` as shown in the following example\.

   ```
   jdbc:redshift:iam://
   ```

   Replace *`region`*, *`account-id`*, and *`cluster-name`* with the values for your AWS Region, account, and cluster\. The JDBC driver uses your IAM account information and cluster name to retrieve the cluster ID, AWS Region, and port number\. To do so, your IAM user or role must have permission to call the `redshift:DescribeClusters` operation with the specified cluster\.

   ```
   jdbc:redshift:iam://cluster-name:region/dbname
   ```

   If your IAM user or role doesn't have permission to call the `redshift:DescribeClusters` operation, include the cluster ID, AWS Region, and port as shown in the following example\. The port number is optional\. The default port is 5439\.

   ```
   jdbc:redshift:iam://cluster-name:port/dbname
   ```

1. Add JDBC options to provide IAM credentials\. You use different combinations of JDBC options to provide IAM credentials\. For details, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.

   The following URL specifies AccessKeyID and SecretAccessKey for an IAM user\.

   ```
   jdbc:redshift:iam://examplecluster:us-west-2/dev?AccessKeyID=AKIAIOSFODNN7EXAMPLE&SecretAccessKey=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```

    The following example specifies a named profile that contains the IAM credentials\.

   ```
   jdbc:redshift:iam://examplecluster:us-west-2/dev?Profile=user2
   ```

1. Add JDBC options that the JDBC driver uses to call the `GetClusterCredentials` API operation\. Don't include these options if you call the `GetClusterCredentials` API operation programmatically\.

   The following example includes the JDBC `GetClusterCredentials` options\.

   ```
   jdbc:redshift:iam://examplecluster:us-west-2/dev?plugin_name=com.amazon.redshift.plugin.AzureCredentialsProvider&UID=user&PWD=password&idp_tenant=my_tenant&client_secret=my_secret&client_id=my_id
   ```<a name="to-configure-an-odbc-connection"></a>

**To configure an ODBC connection to use IAM credentials**

In the following procedure, you can find steps only to configure IAM authentication\. For steps to use standard authentication, using a database user name and password, see [Configuring an ODBC connection](configure-odbc-connection.md)\.

1. Install and configure the latest Amazon Redshift OBDC driver for your operating system\. For more information, see [Configuring an ODBC connection](configure-odbc-connection.md) page\.
**Important**  
The Amazon Redshift ODBC driver must be version 1\.3\.6\.1000 or later\.

1. Follow the steps for your operating system to configure connection settings\.

   For more information, see one of the following:
   + [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)
   + [Configure the ODBC driver on Linux and macOS X operating systems](configure-odbc-connection.md#odbc-driver-configure-linux-mac) 

1. On Microsoft Windows operating systems, access the Amazon Redshift ODBC Driver DSN Setup window\.

   1. Under **Connection Settings**, enter the following information:
      + **Data Source Name** 
      + **Server** \(optional\) 
      + **Port** \(optional\) 
      + **Database** 

      If your IAM user or role has permission to call the `redshift:DescribeClusters` operation, only **Data Source Name** and **Database** are required\. Amazon Redshift uses **ClusterId** and **Region** to get the server and port by calling the `DescribeCluster` operation\. 

      If your IAM user or role doesn't have permission to call the `redshift:DescribeClusters` operation, specify **Server** and **Port**\. The default port is 5439\.

   1. Under **Authentication**, choose a value for **Auth Type**\.

      For each authentication type, enter values as listed following:  
AWS Profile  
Enter the following information:   
      + **ClusterID** 
      + **Region** 
      + **Profile name** 

        Enter the name of a profile in an AWS config file that contains values for the ODBC connection options\. For more information, see [Using a Configuration Profile](options-for-providing-iam-credentials.md#using-configuration-profile)\. 
\(Optional\) Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:   
      + **DbUser**
      + **User AutoCreate**
      + **DbGroups**

        For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.  
IAM Credentials  
Enter the following information:   
      + **ClusterID** 
      + **Region** 
      + **AccessKeyID** and **SecretAccessKey** 

        The access key ID and secret access key for the IAM role or IAM user configured for IAM database authentication\. 
      + **SessionToken** 

        **SessionToken** is required for an IAM role with temporary credentials\. For more information, see [Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\. 
Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:  
      + **DbUser** \(required\) 
      + **User AutoCreate** \(optional\) 
      + **DbGroups** \(optional\) 

        For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.   
Identity Provider: AD FS  
For Windows Integrated Authentication with AD FS, leave **User** and **Password** empty\.  
Provide IdP details:  
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port** \(optional\)

        The port used by identity provider\. The default is 443\. 
      + **Preferred Role** 

        An Amazon Resource Name \(ARN\) for the IAM role from the multi\-valued `AttributeValue` elements for the `Role` attribute in the SAML assertion\. To find the appropriate value for the preferred role, work with your IdP administrator\. For more information, see [Configure SAML assertions for your IdP](configuring-saml-assertions.md)\.
\(Optional\) Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:   
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 
For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.   
Identity Provider: PingFederate  
For **User** and **Password**, enter your IdP user name and password\.  
Provide IdP details:  
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port** \(optional\)

        The port used by identity provider\. The default is 443\. 
      + **Preferred Role** 

        An Amazon Resource Name \(ARN\) for the IAM role from the multi\-valued `AttributeValue` elements for the `Role` attribute in the SAML assertion\. To find the appropriate value for the preferred role, work with your IdP administrator\. For more information, see [Configure SAML assertions for your IdP](configuring-saml-assertions.md)\.
\(Optional\) Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:   
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 
For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.   
Identity Provider: Okta  
For **User** and **Password**, enter your IdP user name and password\.  
Provide IdP details:  
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port ** 

        This value is not used by Okta\. 
      + **Preferred Role** 

        An Amazon Resource Name \(ARN\) for the IAM role from the `AttributeValue` elements for the `Role` attribute in the SAML assertion\. To find the appropriate value for the preferred role, work with your IdP administrator\. For more information, see [Configure SAML assertions for your IdP](configuring-saml-assertions.md)\.
      + **Okta App ID** 

        An ID for an Okta application\. The value for App ID follows "amazon\_aws" in the Okta application embed link\. Work with your IdP administrator to get this value\. 
\(Optional\) Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:   
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 
For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\.   
Identity Provider: Azure AD  
For **User** and **Password**, enter your IdP user name and password\.  
For **Cluster ID** and ** Region**, enter the cluster ID and AWS Region of your Amazon Redshift cluster\.   
For **Database**, enter the database that you created for your Amazon Redshift cluster\.  
Provide IdP details:  
      + **IdP Tenant** 

        The tenant used for Azure AD\.
      + **Azure Client Secret**

        The client secret of the Amazon Redshift enterprise app in Azure\. 
      + **Azure Client ID** 

        The client ID \(application ID\) of the Amazon Redshift enterprise app in Azure\.
\(Optional\) Provide details for options that the ODBC driver uses to call the `GetClusterCredentials` API operation:   
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 
For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 