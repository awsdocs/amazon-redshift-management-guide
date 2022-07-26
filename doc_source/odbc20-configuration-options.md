# Configuring ODBC driver options<a name="odbc20-configuration-options"></a>

You can use driver configuration options to control the behavior of the Amazon Redshift ODBC driver\. Driver options are not case sensitive\.

In Microsoft Windows, you typically set driver options when you configure a data source name \(DSN\)\. You can also set driver options in the connection string when you connect programmatically, or by adding or changing registry keys in `HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI\your_DSN`\. For more information about configuring a DSN, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows)\.

In Linux, you set driver configuration options in your `odbc.ini` and `amazon.redshiftodbc.ini` files, as described in [Use an ODBC driver manager to configure the driver on Linux and macOS X operating systems](configure-odbc-connection.md#odbc-driver-configure-linux-mac)\. Configuration options set in an `amazon.redshiftodbc.ini` file apply to all connections\. In contrast, configuration options set in an `odbc.ini` file are specific to a connection\. Configuration options set in `odbc.ini` take precedence over configuration options set in `amazon.redshiftodbc.ini`\.

Following are descriptions for the options that you can specify for the Amazon Redshift ODBC version 2\.x driver:

## AccessKeyID<a name="odbc20-accesskeyid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

 The IAM access key for the user or role\. If you set this parameter, you must also specify **SecretAccessKey**\.

This parameter is optional\.

## app\_id<a name="odbc20-app-id-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The Okta\-provided unique ID associated with your Amazon Redshift application\.

This parameter is optional\.

## app\_name<a name="odbc20-app-name-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the Okta application that you use to authenticate the connection to Amazon Redshift\.

This parameter is optional\.

## AuthProfile<a name="odbc20-authprofile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The authentication profile used to manage the connection settings\. If you set this parameter, you must also set **AccessKeyID** and **SecretAccessKey**\. 

This parameter is optional\.

## AuthType<a name="odbc20-authtype-option"></a>
+ **Default Value** – Standard
+ **Data Type** – String

This option specifies the authentication mode that the driver uses when you configure a DSN using the Amazon Redshift ODBC Driver DSN Setup dialog box: 
+  Standard: Standard authentication using your Amazon Redshift user name and password\. 
+  AWS Profile: IAM authentication using a profile\.
+  AWS IAM Credentials: IAM authentication using IAM credentials\. 
+  Identity Provider: AD FS: IAM authentication using Active Directory Federation Services \(AD FS\)\. 
+  Identity Provider: Azure AD: IAM authentication using an Azure AD portal\. 
+  Identity Provider: JWT: IAM authentication using a JSON Web Token \(JWT\)\. 
+  Identity Provider: Okta: IAM authentication using Okta\. 
+  Identity Provider: PingFederate: IAM authentication using PingFederate\. 

This option is available only when you configure a DSN using the Amazon Redshift ODBC Driver DSN Setup dialog box in the Windows driver\. When you configure a connection using a connection string or a non\-Windows machine, the driver automatically determines whether to use Standard, AWS Profile, or AWS IAM Credentials authentication based on your specified credentials\. To use an identity provider, you must set the **plugin\_name** property\. 

This parameter is required\.

## AutoCreate<a name="odbc20-autocreate-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver creates a new user when the specified user does not exist\. 
+  1 \| TRUE: If the user specified by the **UID** does not exist, the driver creates a new user\. 
+  0 \| FALSE: The driver does not create a new user\. If the specified user does not exist, the authentication fails\. 

This parameter is optional\.

## CaFile<a name="odbc20-cafile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The file path to the CA certificate file used for some forms of IAM authentication\. 

 This parameter is only available on Linux\.

This parameter is optional\.

## client\_id<a name="odbc20-client-id-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The client ID associated with your Amazon Redshift application in Azure AD\. 

This parameter is required if authenticating through the Azure AD service\.

## client\_ secret<a name="odbc20-client-secret-option"></a>
+ **Default Value** – None
+ **Data Type** – String

 The secret key associated with your Amazon Redshift application in Azure AD\. 

This parameter is required if authenticating through the Azure AD service\.

## ClusterId<a name="odbc20-clusterid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the Amazon Redshift cluster you want to connect to\. It is used in IAM authentication\. The Cluster ID is not specified in the **Server** parameter\.

This parameter is optional\.

## Database<a name="odbc20-database-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the Amazon Redshift database that you want to access\.

This parameter is required\.

## DatabaseMetadataCurrentDbOnly<a name="odbc20-database-metadata-option"></a>
+ **Default Value** – 1
+ **Data Type** – Boolean

A boolean specifying whether the driver returns metadata from multiple databases and clusters\.
+ 1 \| TRUE: The driver only returns metadata from the current database\. 
+  0 \| FALSE\. The driver returns metadata across multiple Amazon Redshift databases and clusters\. 

This parameter is optional\.

## dbgroups\_filter<a name="odbc20-dbgroups-filter-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The regular expression you can specify to filter out DbGroups that are received from the SAML response to Amazon Redshift when using Azure, Browser Azure, and Browser SAML authentication types\. 

This parameter is optional\.

## Driver<a name="odbc20-driver-option"></a>
+ **Default Value** – Amazon Redshift ODBC Driver \(x64\)
+ **Data Type** – String

The name of the driver\. The only supported value is **Amazon Redshift ODBC Driver \(x64\)**\.

This parameter is required if you do not set **DSN**\.

## DSN<a name="odbc20-dsn-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the driver data source name\. The application specifies the DSN in the SQLDriverConnect API\.

This parameter is required if you do not set **Driver\.**\.

## EndpointUrl<a name="odbc20-endpointurl-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The overriding endpoint used to communicate with the Amazon Redshift Coral Service for IAM authentication\.

This parameter is optional\.

## ForceLowercase<a name="odbc20-forcelowercase-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver lowercases all DbGroups sent from the identity provider to Amazon Redshift when using SSO authentication\. 
+  1 \| TRUE: The driver lowercases all DbGroups that are sent from the identity provider\. 
+  0 \| FALSE: The driver does not alter DbGroups\. 

This parameter is optional\.

## https\_proxy\_host<a name="odbc20-https-proxy-host-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The host name or IP address of the proxy server through which you want to pass IAM authentication processes\.

This parameter is optional\.

## https\_proxy\_password<a name="odbc20-https-proxy-password-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The password that you use to access the proxy server\. It’s used for IAM authentication\.

This parameter is optional\.

## https\_proxy\_port<a name="odbc20-https-proxy-port-option"></a>
+ **Default Value** – None
+ **Data Type** – Integer

The number of the port that the proxy server uses to listen for client connections\. It’s used for IAM authentication\.

This parameter is optional\.

## https\_proxy\_username<a name="odbc20-https-proxy-username-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The user name that you use to access the proxy server\. It’s used for IAM authentication\.

This parameter is optional\.

## IAM<a name="odbc20-iam-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver uses an IAM authentication method to authenticate the connection\. 
+  1 \| TRUE: The driver uses one of the IAM authentication methods \(using an access key and secret key pair, or a profile, or a credentials service\)\. 
+  0 \| FALSE\. The driver uses standard authentication \(using your database user name and password\)\. 

This parameter is optional\.

## idp\_host<a name="odbc20-idp-host-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The IdP \(identity provider\) host you are using to authenticate into Amazon Redshift\.

This parameter is optional\.

## idp\_port<a name="odbc20-idp-port-option"></a>
+ **Default Value** – None
+ **Data Type** – Integer

The port for an IdP \(identity provider\) you are using to authenticate into Amazon Redshift\. The default port is 5439\.

This parameter is optional\.

## idp\_response\_timeout<a name="odbc20-idp-response-timeout-option"></a>
+ **Default Value** – 120
+ **Data Type** – Integer

The number of seconds that the driver waits for the SAML response from the identity provider when using SAML or Azure AD services through a browser plugin\. 

This parameter is optional\.

## idp\_tenant<a name="odbc20-idp-tenant-option"></a>
+ **Default Value** – None
+ **Data Type** – String

 The Azure AD tenant ID associated with your Amazon Redshift application\.

This parameter is required if authenticating through the Azure AD service\.

## idp\_use\_https\_proxy<a name="odbc20-idp-use-https-proxy-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver passes the authentication processes for identity providers \(IdP\) through a proxy server\. 
+  1 \| TRUE: The driver passes IdP authentication processes through a proxy server\. 
+  0 \| FALSE\. The driver does not pass IdP authentication processes through a proxy server\. 

This parameter is optional\.

## InstanceProfile<a name="odbc20-instanceprofile-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver uses the Amazon EC2 instance profile, when configured to use a profile for authentication\.
+  1 \| TRUE: The driver uses the Amazon EC2 instance profile\. 
+  0 \| FALSE\. The driver uses the chained roles profile specified by the Profile Name option \(**Profile**\) instead\. 

This parameter is optional\.

## KeepAlive<a name="odbc20-keepalive-option"></a>
+ **Default Value** – 1
+ **Data Type** – Boolean

A boolean specifying whether the driver uses TCP keepalives to prevent connections from timing out\.
+  1 \| TRUE: The driver uses TCP keepalives to prevent connections from timing out\. 
+  0 \| FALSE\. The driver does not use TCP keepalives\. 

This parameter is optional\.

## KeepAliveCount<a name="odbc20-keepalivecount-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The number of TCP keepalive packets that can be lost before the connection is considered broken\. When this parameter is set to 0, the driver uses the system default for this setting\. 

This parameter is optional\.

## KeepAliveInterval<a name="odbc20-keepaliveinterval-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The number of seconds between each TCP keepalive retransmission\. When this parameter is set to 0, the driver uses the system default for this setting\. 

This parameter is optional\.

## KeepAliveTime<a name="odbc20-keepalivetime-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The number of seconds of inactivity before the driver sends a TCP keepalive packet\. When this parameter is set to 0, the driver uses the system default for this setting\. 

This parameter is optional\.

## listen\_port<a name="odbc20-listen-port-option"></a>
+ **Default Value** – 7890
+ **Data Type** – Integer

The port that the driver uses to receive the SAML response from the identity provider when using SAML or Azure AD services through a browser plugin\. 

This parameter is optional\.

## login\_url<a name="odbc20-login-url-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The URL for the resource on the identity provider's website when using the generic Browser SAML plugin\.

This parameter is required if authenticating with the SAML or Azure AD services through a browser plugin\.

## loginToRp<a name="odbc20-logintorp-option"></a>
+ **Default Value** – urn:amazon:webservices
+ **Data Type** – String

The relying party trust that you want to use for the AD FS authentication type\.

This string is optional\.

## LogLevel<a name="odbc20-loglevel-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

Use this property to enable or disable logging in the driver and to specify the amount of detail included in log\. files\. We recommend you only enable logging long enough to capture an issue, as logging decreases performance and can consume a large quantity of disk space\.

 Set the property to one of the following values:
+  0: OFF\. Disable all logging\. 
+  1: ERROR\. Logs error events that might allow the driver to continue running but produce an error\. 
+  2: API\_CALL\. Logs ODBC API function calls with function argument values\. 
+  3: INFO\. Logs general information that describes the progress of the driver\. 
+  4: MSG\_PROTOCOL\. Logs detailed information of the driver's message procotol\. 
+  5: DEBUG\. Logs all driver activity 
+  6: DEBUG\_APPEND\. Keep appending logs for all driver activities\. 

When logging is enabled, the driver produces the following log files at the location you specify in the **LogPath** property: 
+  A `redshift_odbc.log.1` file that logs driver activity that takes place during handshake of a connection\. 
+  A `redshift_odbc.log` file for all driver activities after a connection is made to the database\. 

This parameter is optional\.

## LogPath<a name="odbc20-logpath-option"></a>
+ **Default Value** – The OS\-specific TEMP directory
+ **Data Type** – String

The full path to the folder where the driver saves log files when **LogLevel** is higher than 0\.

This parameter is optional\.

## Min\_TLS<a name="odbc20-min-tls-option"></a>
+ **Default Value** – 1\.0
+ **Data Type** – String

 The minimum version of TLS/SSL that the driver allows the data store to use for encrypting connections\. For example, if TLS 1\.1 is specified, TLS 1\.0 cannot be used to encrypt connections\.

Min\_TLS accepts the following values:
+  1\.0: The connection must use at least TLS 1\.0\. 
+  1\.1: The connection must use at least TLS 1\.1\. 
+  1\.2: The connection must use at least TLS 1\.2\. 

This parameter is optional\.

## partner\_spid<a name="odbc20-partner-spid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The partner SPID \(service provider ID\) value to use when authenticating the connection using the PingFederate service\.

This parameter is optional\.

## Password \| PWS<a name="odbc20-password-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The password corresponding to the user name that you provided in the User field \(**UID** \| **User** \| **LogonID**\)\. 

This parameter is optional\.

## plugin\_name<a name="odbc20-plugin-name-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The credentials provider plugin name that you want to use for authentication\. 

 The following values are supported: 
+  `ADFS`: Use Active Directory Federation Services for authentication\. 
+  `AzureAD`: Use Microsoft Azure Active Directory \(AD\) Service for authentication\. 
+  `BrowserAzureAD`: Use a browser plugin for the Microsoft Azure Active Directory \(AD\) Service for authentication\. 
+  `BrowserSAML`: Use a browser plugin for SAML services such as Okta or Ping for authentication\. 
+  `JWT`: Use a JSON Web Token \(JWT\) for authentication\. 
+  `Ping`: Use the PingFederate service for authentication\. 
+  `Okta`: Use the Okta service for authentication\. 

This parameter is optional\.

## Port \| PortNumber<a name="odbc20-port-option"></a>
+ **Default Value** – 5439
+ **Data Type** – Integer

The number of the TCP port that the Amazon Redshift server uses to listen for client connections\. 

This parameter is optional\.

## preferred\_role<a name="odbc20-preferred-role-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The role you want to assume during the connection to Amazon Redshift\. It’s used for IAM authentication\.

This parameter is optional\.

## Profile<a name="odbc20-profile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the user AWS profile used to authenticate into Amazon Redshift\.
+  If the Use Instance Profile parameter \(the **InstanceProfile** property\) is set to 1 \| TRUE, that setting takes precedence and the driver uses the Amazon EC2 instance profile instead\. 
+  The default location for the credentials file that contains profiles is `~/.aws/Credentials`\. The `AWS_SHARED_CREDENTIALS_FILE` environment variable can be used to point to a different credentials file\. 

This parameter is optional\.

## provider\_name<a name="odbc20-provider-name-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The authentication provider created by the user using the CREATE IDENTITY PROVIDER query\. It’s used in native Amazon Redshift authentication\.

This parameter is optional\.

## ProxyHost<a name="odbc20-proxyhost-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The host name or IP address of the proxy server that you want to connect through\.

This parameter is optional\.

## ProxyPort<a name="odbc20-proxyport-option"></a>
+ **Default Value** – None
+ **Data Type** – Integer

The number of the port that the proxy server uses to listen for client connections\.

This parameter is optional\.

## ProxyPwd<a name="odbc20-proxypwd-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The password that you use to access the proxy server\. 

This parameter is optional\.

## ProxyUid<a name="odbc20-proxyuid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The user name that you use to access the proxy server\.

This parameter is optional\.

## ReadOnly<a name="odbc20-readonly-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver is in read\-only mode\. 
+  1 \| TRUE: The connection is in read\-only mode, and cannot write to the data store\. 
+  0 \| FALSE: The connection is not in read\-only mode, and can write to the data store\. 

This parameter is optional\.

## region<a name="odbc20-region-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The AWS region that your cluster is in\. 

This parameter is optional\.

## SecretAccessKey<a name="odbc20-secretaccesskey-option"></a>
+ **Default Value** – None
+ **Data Type** – String

 The IAM secret key for the user or role\. If you set this parameter, you must also set **AccessKeyID**\. 

This parameter is optional\.

## SessionToken<a name="odbc20-sessiontoken-option"></a>
+ **Default Value** – None
+ **Data Type** – String

 The temporary IAM session token associated with the IAM role that you are using to authenticate\. 

This parameter is optional\.

## Server \| HostName \| Host<a name="odbc20-server-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The endpoint server to connect to\.

This parameter is required\.

## ssl\_insecure<a name="odbc20-ssl-insecure-option"></a>
+ **Default Value** – 0
+ **Data Type** – Boolean

A boolean specifying whether the driver checks the authenticity of the IdP server certificate\.
+  1 \| TRUE: The driver does not check the authenticity of the IdP server certificate\. 
+  0 \| FALSE: The driver checks the authenticity of the IdP server certificate 

This parameter is optional\.

## SSLMode<a name="odbc20-sslmode-option"></a>
+ **Default Value** – `verify-ca`
+ **Data Type** – String

The SSL certificate verification mode to use when connecting to Amazon Redshift\. The following values are possible: 
+  `verify-full`: Connect only using SSL, a trusted certificate authority, and a server name that matches the certificate\. 
+  `verify-ca`: Connect only using SSL and a trusted certificate authority\. 
+  `require`: Connect only using SSL\. 
+  `prefer`: Connect using SSL if available\. Otherwise, connect without using SSL\. 
+  `allow`: By default, connect without using SSL\. If the server requires SSL connections, then use SSL\. 
+  `disable`: Connect without using SSL\. 

This parameter is optional\.

## StsConnectionTimeout<a name="odbc20-stsconnectiontimeout-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The maximum wait time for IAM connections, in seconds\. If set to 0 or not specified, the driver waits 60 seconds for each AWS STS call\. 

This parameter is optional\.

## StsEndpointUrl<a name="odbc20-stsendpointurl-option"></a>
+ **Default Value** – None
+ **Data Type** – String

This option specifies the overriding endpoint used to communicate with the AWS Security Token Service \(AWS STS\)\. 

This parameter is optional\.

## UID \| User \| LogonID<a name="odbc20-uid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The user name that you use to access the Amazon Redshift server\.

This parameter is required if you use database authentication\.

## web\_identity\_token<a name="odbc20-web-identity-token-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The OAUTH token that is provided by the identity provider\. It’s used in the JWT plugin\.

This parameter is required if you set the **plugin\_name** parameter to BasicJwtCredentialsProvider\.