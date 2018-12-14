# Step 5: Configure a JDBC or ODBC Connection to Use IAM Credentials<a name="generating-iam-credentials-configure-jdbc-odbc"></a>

You can configure your SQL client with an Amazon Redshift JDBC or ODBC driver that manages the process of creating database user credentials and establishing a connection between your SQL client and your Amazon Redshift database\. <a name="to-configure-a-jdbc-connection"></a>

**To configure a JDBC connection to use IAM credentials**

1. Download the latest Amazon Redshift JDBC driver from the [Configure a JDBC Connection](configure-jdbc-connection.md) page\.
**Important**  
The Amazon Redshift JDBC driver must be version 1\.2\.7\.1003 or later\.

1. Create a JDBC URL with the IAM credentials options in one of the following formats\. To use IAM authentication, add iam: to the Amazon Redshift JDBC URL following jdbc:redshift: as shown in the following example\.

   ```
   jdbc:redshift:iam://
   ```

   Replace *cluster\-name*, *region*, and *dbname* with values for your cluster name, region, and database name\. The JDBC driver uses your IAM account information and cluster name to retrieve the cluster ID, region, and port number\. To do so, your IAM user or role must have permission to call the redshift:DescribeClusters action with the specified cluster\.

   ```
   jdbc:redshift:iam://cluster-name:region/dbname
   ```

   If your IAM user or role doesn't have permission to call the `redshift:DescribeClusters` action, include the cluster ID, region, and port as shown in the following example\. The port number is optional\. The default port is 5439\.

   ```
   jdbc:redshift:iam://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev
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

1. Add JDBC options that the JDBC driver uses to call the GetClusterCredentials API action\. Don't include these options if you call the GetClusterCredentials API action programmatically\. For more details, see [Configure a JDBC or ODBC Connection to Use IAM Credentials](#generating-iam-credentials-configure-jdbc-odbc)\.

   The following example includes the JDBC GetClusterCredentials options\.

   ```
   jdbc:redshift:iam://examplecluster:us-west-2/dev?Profile=user2&DbUser=newuser&AutoCreate=true&DbGroups=group1,group2
   ```<a name="to-configure-an-odbc-connection"></a>

**To configure an ODBC connection to use IAM credentials**

In this topic, you can find steps only to configure IAM authentication\. For steps to use standard authentication, using a database user name and password, see [Configure an ODBC Connection](configure-odbc-connection.md)\.

1. 1\. Install and configure the latest Amazon Redshift OBDC driver for your operating system\. For more information, see [Configure an ODBC Connection](configure-odbc-connection.md) page\.
**Important**  
The Amazon Redshift ODBC driver must be version 1\.3\.6\.1000 or later\.

1. Follow the steps for your operating system to configure connection settings\.

   For more information, see one of the following topics:
   + [Install and Configure the Amazon Redshift ODBC Driver on Microsoft Windows Operating Systems](install-odbc-driver-windows.md)
   + [Configure the ODBC Driver on Linux and Mac OS X Operating Systems](odbc-driver-configure-linux-mac.md) 

1. On Microsoft Windows Operation Systems, access the Amazon Redshift ODBC Driver DSN Setup window\.

   1. Under **Connection Settings**, type the following information:
      + **Data Source Name** 
      + **Server** \(optional\) 
      + **Port** \(optional\) 
      + **Database** 

      If your IAM user or role has permission to call the `redshift:DescribeClusters` action, only **Data Source Name** and **Database** are required\. Amazon Redshift uses **ClusterId** and **Region** to get the server and port by calling the `DescribeCluster` action\. 

      If your IAM user or role doesn't have permission to call the `redshift:DescribeClusters` action, specify **Server** and **Port**\. The default port is 5439\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings.png)

   1. Under **Authentication**, choose an **Auth Type**\.

      For each authentication type, specific fields appear as shown following\. 

      **AWS Profile** 

      Enter the following information: 
      + **ClusterID** 
      + **Region** 

      Optionally, provide details for options that the ODBC driver uses to call the GetClusterCredentials API action\. 
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 

      For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 
      + **Profile name** 

        Type the name of a profile in an AWS config file that contains values for the ODBC connection options\. For more information, see [Using a Configuration Profile](options-for-providing-iam-credentials.md#using-configuration-profile)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings-profile.png)

      **AWS IAM Credentials**

      Enter the following information: 
      + **ClusterID** 
      + **Region** 

      Provide details for options that the ODBC driver uses to call the GetClusterCredentials API action\. 
      + **DbUser** \(required\) 
      + **User AutoCreate** \(optional\) 
      + **DbGroups** \(optional\) 

      For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 
      + **AccessKeyID** and **SecretAccessKey** 

        The access key ID and secret access key for the IAM role or IAM user configured for IAM database authentication\. 
      + **SessionToken** 

        **SessionToken** is required for an IAM role with temporary credentials\. For more information, see [ Temporary Security Credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings-iam-creds.png)

      **Identity Provider: AD FS**

      For Windows Integrated Authentication with AD FS, leave **User** and **Password** empty\.

      Optionally, provide details for options that the ODBC driver uses to call the GetClusterCredentials API action\. 
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 

      For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 

      Provide IdP details\.
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port** \(optional\)

        The port used by identity provider\. The default is 443\. 
      + **Preferred Role** 

        A role Amazon Resource Name \(ARN\) from the AttributeValue elements for the Role attribute in the SAML assertion\. Work with your IdP administrator to find the appropriate value for the preferred role\. For more information, see [Configure SAML Assertions for Your IdP](configuring-saml-assertions.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings-adfs.png)

      **Identity Provider: PingFederate**

      For **User** and **Password**, type your IdP user name and password\.

      Optionally, provide details for options that the ODBC driver uses to call the GetClusterCredentials API action\. 
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 

      For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 

      Provide IdP details\.
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port** \(optional\)

        The port used by identity provider\. The default is 443\. 
      + **Preferred Role** 

        A role Amazon Resource Name \(ARN\) from the AttributeValue elements for the Role attribute in the SAML assertion\. Work with your IdP administrator to find the appropriate value for the preferred role\. For more information, see [Configure SAML Assertions for Your IdP](configuring-saml-assertions.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings-ping.png)

      **Identity Provider: Okta**

      For **User** and **Password**, type your IdP user name and password\.

      Optionally, provide details for options that the ODBC driver uses to call the GetClusterCredentials API action\. 
      + **DbUser** 
      + **User AutoCreate** 
      + **DbGroups** 

      For more information, see [JDBC and ODBC Options for Creating Database User Credentials](jdbc-and-odbc-options-for-database-credentials.md)\. 

      Provide IdP details\.
      + **IdP Host** 

        The name of the corporate identity provider host\. This name should not include any slashes \( / \)\.
      + **IdP Port ** 

        IdP Port is not used by Okta\. 
      + **Preferred Role** 

        A role Amazon Resource Name \(ARN\) from the AttributeValue elements for the Role attribute in the SAML assertion\. Work with your IdP administrator to find the appropriate value for the preferred role\. For more information, see [Configure SAML Assertions for Your IdP](configuring-saml-assertions.md)\.
      + **Okta App ID** 

        An ID for an Okta application\. The value for App ID follows "amazon\_aws" in the Okta Application Embed Link\. Work with your IdP administrator to get this value\. The following is an example of an application embed link\.

        ```
        https://example.okta.com/home/amazon_aws/0oa2hylwrpM8UGehd1t7/272
        ```  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/CreatingTemporaryCredentials-odbc-connection-settings-okta.png)