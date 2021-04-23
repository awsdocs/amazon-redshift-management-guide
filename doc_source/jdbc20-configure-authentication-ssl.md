# Configure authentication and SSL<a name="jdbc20-configure-authentication-ssl"></a>

To protect data from unauthorized access, Amazon Redshift data stores require all connections to be authenticated using user credentials\. Some data stores also require connections to be made over the Secure Sockets Layer \(SSL\) protocol, either with or without one\-way authentication\.

The Amazon Redshift JDBC driver version 2\.0 provides full support for these authentication protocols\. 

The SSL version that the driver supports depends on the JVM version that you are using\. For information about the SSL versions that are supported by each version of Java, see [Diagnosing TLS, SSL, and HTTPS](https://blogs.oracle.com/java-platform-group/diagnosing-tls,-ssl,-and-https) on the Java Platform Group Product Management Blog\. 

The SSL version used for the connection is the highest version that is supported by both the driver and the server, which is determined at connection time\.

Configure the Amazon Redshift JDBC driver version 2\.0 to authenticate your connection according to the security requirements of the Amazon Redshift server that you are connecting to\. 

You must always provide your Amazon Redshift user name and password to authenticate the connection\. Depending on whether SSL is enabled and required on the server, you might also need to configure the driver to connect through SSL\. Or you might use one\-way SSL authentication so that the client \(the driver itself\) verifies the identity of the server\. 

You provide the configuration information to the driver in the connection URL\. For more information about the syntax of the connection URL, see [Building the connection URL](jdbc20-obtain-url.md#jdbc20-build-connection-url)\. 

*SSL* indicates TLS/SSL, both Transport Layer Security and Secure Sockets Layer\. The driver supports industry\-standard versions of TLS/SSL\.  

## Using user name and password only<a name="jdbc20-authentication-username-password"></a>

If the server you are connecting to does not use SSL, then you only need to provide your user name and password to authenticate the connection\. 

**To configure authentication using your user name and password only**

1. Set the `UID` property to your user name for accessing the Amazon Redshift server\.

1. Set the PWD property to the password corresponding to your user name\.

## Using SSL without identity verification<a name="jdbc20-use-ssl-without-identity-verification"></a>

If the server you are connecting to uses SSL but does not require identity verification, then you can configure the driver to use a non\-validating SSL factory\. 

**To configure an SSL connection without identity verification**

1. Set the `UID` property to your user name for accessing the Amazon Redshift server\.

1. Set the `PWD` property to the password corresponding to your user name\.

1. Set the `SSLFactory` property to `com.amazon.redshift.ssl.NonValidatingFactory`\.

## Using one\-way SSL authentication<a name="jdbc20-use-one-way-SSL-authentication"></a>

If the server you are connecting to uses SSL and has a certificate, then you can configure the driver to verify the identity of the server using one\-way authentication\. 

One\-way authentication requires a signed, trusted SSL certificate for verifying the identity of the server\. You can configure the driver to use a specific certificate or access a TrustStore that contains the appropriate certificate\. If you do not specify a certificate or TrustStore, then the driver uses the default Java TrustStore \(typically either `jssecacerts` or `cacerts`\)\. 

**To configure one\-way SSL authentication**

1. Set the UID property to your user name for accessing the Amazon Redshift server\.

1. Set the PWD property to the password corresponding to your user name\.

1. Set the SSL property to true\.

1. Set the SSLRootCert property to the location of your root CA certificate\.

1. If you are not using one of the default Java TrustStores, then do one of the following:
   + To specify a server certificate, set the SSLRootCert property to the full path of the certificate\.
   + Or, to specify a TrustStore, do the following:

     1. Use the keytool program to add the server certificate to the TrustStore that you want to use\.

     1. Specify the TrustStore and password to use when starting the Java application using the driver\. For example:

        ```
        -Djavax.net.ssl.trustStore=[TrustStoreName]
        -Djavax.net.ssl.trustStorePassword=
        [TrustStorePassword]
        ```

1. Choose one:
   + To validate the certificate, set the SSLMode property to verify\-ca\.
   + Or, to validate the certificate and verify the host name in the certificate, set the SSLMode property to verify\-full\.

## Configuring IAM authentication<a name="jdbc20-configure-iam-authentication"></a>

If you are connecting to a Amazon Redshift server using IAM authentication, set the following properties as part of your data source connection string\. 

  For more information on IAM Authentication, see [Identity and access management in Amazon Redshift](redshift-iam-authentication-access-control.md)\.

To use IAM Authentication, use one of the following connection string formats:


| Connection String | Description | 
| --- | --- | 
|  `jdbc:redshift:iam:// [host]:[port]/[db]`  |  A regular connection string\. The driver infers the ClusterID and Region from the host\.  | 
|  `jdbc:redshift:iam:// [cluster-id]: [region]/[db]`  |  The driver retrieves host information, given the ClusterID and Region\.  | 
|  `jdbc:redshift:iam:// [host]/[db]`  |  The driver defaults to port 5439, and infers ClusterID and Region from the host\.  | 

## Specifying profiles<a name="jdbc20-aws-credentials-profiles"></a>

If you are using IAM authentication, you have the option to specify any additional required or optional connection properties under a profile name\. This enables you to avoid putting certain information directly in the connection string\. You specify the profile name in your connection string using the Profile property\. 

Profiles can be added to the AWS Credentials file\. The default location for this file is: `~/.aws/credentials` 

You can change the default value by setting the path in the following environment variable: `AWS_CREDENTIAL_PROFILES_FILE` 

 For more information about profiles see [Working with AWS Credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) in the *AWS SDK for Java*\. 

## Using instance profile credentials<a name="jdbc20-instance-profile-credentials"></a>

If you are running an application on an EC2 instance that is associated with an IAM role, you can connect using the instance profile credentials\. 

To do this, use one of the IAM connection string formats in the preceding table, and set the dbuser connection property to the Amazon Redshift user name that you are connecting as\. 

For more information about instance profiles see [Access Management](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\. 

## Using credential providers<a name="jdbc20-aws-credentials-provider"></a>

The driver also supports credential provider plugins from the following services: 
+ AD FS
+ Azure AD
+ Okta
+ PingFederate

If you use one of these services, the connection URL needs to specify the following properties: 
+ **Plugin\_Name** – The fully\-qualified class path for your credentials provider plugin class\.
+ **IdP\_Host:** – The host for the service you are using to authenticate into Amazon Redshift\.
+ **IdP\_Port** – The port that the host for the authentication service listens at\. Not required for Okta\.
+ **User** – The user name for the idp\_host server\.
+ **Password** – The password associated with the idp\_host user name\.
+ **DbUser** – The Amazon Redshift user name you are connecting as\.
+ **SSL\_Insecure** – Indicates whether the IDP server certificate should be verified\.
+ **Client\_ID** – The client ID associated with the user name in the Azure AD portal\. Only used for Azure AD\.
+ **Client\_Secret** – The client secret associated with the client ID in the Azure AD portal\. Only used for Azure AD\.
+ **IdP\_Tenant** – The Azure AD tenant ID for your Amazon Redshift application\. Only used for Azure AD\.
+ **App\_ID** – The Okta app ID for your Amazon Redshift application\. Only used for Okta\.
+ **App\_Name** – The optional Okta app name for your Amazon Redshift application\. Only used for Okta\.
+ **Partner\_SPID** – The optional partner SPID \(service provider ID\) value\. Only used for PingFederate\.

If you are using a browser plugin for one of these services, the connection URL can also include: 
+ **Login\_URL** –The URL for the resource on the identity provider's website when using the SAML or Azure AD services through a browser plugin\. This parameter is required if you are using a browser plugin\.
+ **Listen\_Port** – The port that the driver uses to get the SAML response from the identity provider when using the SAML or Azure AD services through a browser plugin \.
+ **IdP\_Response\_Timeout** – The amount of time, in seconds, that the driver waits for the SAML response from the identity provider when using the SAML or Azure AD services through a browser plugin\.

For information on additional connection string properties, see [JDBC driver version 2\.0 configuration options](jdbc20-configuration-options.md)\. 