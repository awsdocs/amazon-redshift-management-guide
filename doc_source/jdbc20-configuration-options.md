# Options for JDBC driver version 2\.0 configuration<a name="jdbc20-configuration-options"></a>

Following, you can find descriptions for the options that you can specify for version 2\.0 of the Amazon Redshift JDBC driver\. 

You can set configuration properties using the connection URL\. For more information, see [Building the connection URL](jdbc20-build-connection-url.md)\.

**Topics**
+ [AccessKeyID](#jdbc20-accesskeyid-option)
+ [AllowDBUserOverride](#jdbc20-allowdbuseroverride-option)
+ [App\_ID](#jdbc20-app-id-option)
+ [App\_Name](#jdbc20-app-name-option)
+ [AuthProfile](#jdbc20-authprofile-option)
+ [AutoCreate](#jdbc20-autocreate-option)
+ [Client\_ID](#jdbc20-client_id-option)
+ [Client\_Secret](#jdbc20-client_secret-option)
+ [ClusterID](#jdbc20-clusterid-option)
+ [databaseMetadataCurrentDbOnly](#jdbc20-databasemetadatacurrentdbonly-option)
+ [DbUser](#jdbc20-dbuser-option)
+ [DbGroups](#jdbc20-dbgroups-option)
+ [defaultRowFetchSize](#jdbc20-defaultrowfetchsize-option)
+ [DisableIsValidQuery](#jdbc20-disableisvalidquery-option)
+ [enableFetchRingBuffer](#jdbc20-enablefetchringbuffer-option)
+ [enableMultiSqlSupport](#jdbc20-enablemultisqlsupport-option)
+ [fetchRingBufferSize](#jdbc20-fetchringbuffersize-option)
+ [ForceLowercase](#jdbc20-forcelowercase-option)
+ [IAMDisableCache](#jdbc20-iamdisablecache-option)
+ [IAMDuration](#jdbc20-iamduration-option)
+ [IdP\_Host](#jdbc20-idp_host-option)
+ [IdP\_Port](#jdbc20-idp_port-option)
+ [IdP\_Tenant](#jdbc20-idp_tenant-option)
+ [IdP\_Response\_Timeout](#jdbc20-idp_response_timeout-option)
+ [IniFile](#jdbc20-inifile-option)
+ [IniSection](#jdbc20-inisection-option)
+ [Login\_URL](#jdbc20-login_url-option)
+ [LoginTimeout](#jdbc20-logintimeout-option)
+ [loginToRp](#jdbc20-logintorp-option)
+ [LogLevel](#jdbc20-loglevel-option)
+ [LogPath](#jdbc20-logpath-option)
+ [Partner\_SPID](#jdbc20-partner_spid-option)
+ [Password](#jdbc20-password-option)
+ [Plugin\_Name](#jdbc20-plugin_name-option)
+ [Preferred\_Role](#jdbc20-preferred_role-option)
+ [Profile](#jdbc20-profile-option)
+ [PWD](#jdbc20-pwd-option)
+ [queryGroup](#jdbc20-querygroup-option)
+ [ReadOnly](#jdbc20-readonly-option)
+ [Region](#jdbc20-region-option)
+ [reWriteBatchedInsertsSize](#jdbc20-rewritebatchedinsertssize-option)
+ [roleArn](#jdbc20-rolearn-option)
+ [roleSessionName](#jdbc20-roleaessionname-option)
+ [SecretAccessKey](#jdbc20-secretaccesskey-option)
+ [SessionToken](#jdbc20-sessiontoken-option)
+ [SocketTimeout](#jdbc20-sockettimeout-option)
+ [SSL](#jdbc20-ssl-option)
+ [SSL\_Insecure](#jdbc20-ssl_insecure-option)
+ [SSLCert](#jdbc20-sslcert-option)
+ [SSLFactory](#jdbc20-sslfactory-option)
+ [SSLKey](#jdbc20-sslkey-option)
+ [SSLMode](#jdbc20-sslmode-option)
+ [SSLPassword](#jdbc20-sslpassword-option)
+ [SSLRootCert](#jdbc20-sslrootcert-option)
+ [StsEndpointUrl](#jdbc20-stsendpointurl-option)
+ [TCPKeepAlive](#jdbc20-tcpkeepalive-option)
+ [UID](#jdbc20-uid-option)
+ [User](#jdbc20-user-option)
+ [webIdentityToken](#jdbc20-webidentitytoken-option)

## AccessKeyID<a name="jdbc20-accesskeyid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

You can specify this parameter to enter the IAM access key for the IAM user or role\. You can usually locate the key by looking at and existing string or user profile\. If you specify this parameter, you must also specify the `SecretAccessKey` parameter\. 

This parameter is optional\.

## AllowDBUserOverride<a name="jdbc20-allowdbuseroverride-option"></a>
+ **Default Value** – 0
+ **Data Type** – String

This option specifies whether the driver uses the `DbUser` value from the SAML assertion or the value that is specified in the `DbUser` connection property in the connection URL\. 

This parameter is optional\.

**1**  
The driver uses the `DbUser` value from the SAML assertion\.  
If the SAML assertion doesn't specify a value for `DBUser`, the driver uses the value specified in the `DBUser` connection property\. If the connection property also doesn't specify a value, the driver uses the value specified in the connection profile\.

**0**  
The driver uses the `DBUser` value specified in the `DBUser` connection property\.  
If the `DBUser` connection property doesn't specify a value, the driver uses the value specified in the connection profile\. If the connection profile also doesn't specify a value, the driver uses the value from the SAML assertion\.

## App\_ID<a name="jdbc20-app-id-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The Okta\-provided unique ID associated with your Amazon Redshift application\. 

This parameter is required if authenticating through the Okta service\.

## App\_Name<a name="jdbc20-app-name-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the Okta application that you use to authenticate the connection to Amazon Redshift\. 

This parameter is optional\.

## AuthProfile<a name="jdbc20-authprofile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the authentication profile to use for connecting to Amazon Redshift\. 

This parameter is optional\.

## AutoCreate<a name="jdbc20-autocreate-option"></a>
+ **Default Value** – false
+ **Data Type** – Boolean

This option specifies whether the driver causes a new user to be created when the specified user doesn't exist\. 

This parameter is optional\.

**true**  
If the user specified by either `DBUser` or unique ID \(UID\) doesn't exist, a new user with that name is created\.

**false**  
The driver doesn't cause new users to be created\. If the specified user doesn't exist, the authentication fails\.

## Client\_ID<a name="jdbc20-client_id-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The client ID to use when authenticating the connection using the Azure AD service\. 

This parameter is required if authenticating through the Azure AD service\.

## Client\_Secret<a name="jdbc20-client_secret-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The Client Secret to use when authenticating the connection using the Azure AD service\. 

This parameter is required if authenticating through the Azure AD service\.

## ClusterID<a name="jdbc20-clusterid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the Amazon Redshift cluster that you want to connect to\. 

This parameter is optional\.

## databaseMetadataCurrentDbOnly<a name="jdbc20-databasemetadatacurrentdbonly-option"></a>
+ **Default Value** – true
+ **Data Type** – Boolean

This option specifies whether the metadata API retrieves data from all accessible databases or only from the connected database\. 

This parameter is optional\.

You can specify the following values:

**true**  
The application retrieves metadata from a single database\.

**false**  
The application retrieves metadata from all accessible databases\.

## DbUser<a name="jdbc20-dbuser-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The user ID to use with your Amazon Redshift account\. You can use an ID that doesn't currently exist if you have enabled the AutoCreate property\. 

This parameter is optional\.

## DbGroups<a name="jdbc20-dbgroups-option"></a>
+ **Default Value** – PUBLIC
+ **Data Type** – String

A comma\-separated list of existing database group names that `DBUser` joins for the current session\. 

This parameter is optional\.

## defaultRowFetchSize<a name="jdbc20-defaultrowfetchsize-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

This option specifies a default value for getFetchSize\. 

This parameter is optional\.

You can specify the following values:

**0**  
Fetch all rows in a single operation\.

**Positive integer**  
Number of rows to fetch from the database for each fetch iteration of the ResultSet\.

## DisableIsValidQuery<a name="jdbc20-disableisvalidquery-option"></a>
+ **Default Value** – False
+ **Data Type** – Boolean

This option specifies whether the driver submits a new database query when using the Connection\.isValid\(\) method to determine whether the database connection is active\. 

This parameter is optional\.

**true**  
The driver doesn't submit a query when using Connection\.isValid\(\) to determine whether the database connection is active\. This may cause the driver to incorrectly identify the database connection as active if the database server has shut down unexpectedly\.

**false**  
The driver submits a query when using Connection\.isValid\(\) to determine whether the database connection is active\.

## enableFetchRingBuffer<a name="jdbc20-enablefetchringbuffer-option"></a>
+ **Default Value** – true
+ **Data Type** – Boolean

This option specifies that the driver fetches rows using a ring buffer on a separate thread\. The fetchRingBufferSize parameter specifies the ring buffer size\. 

This parameter is optional\.

## enableMultiSqlSupport<a name="jdbc20-enablemultisqlsupport-option"></a>
+ **Default Value** – true
+ **Data Type** – Boolean

This option specifies whether to process multiple SQL commands separated by semicolons in a Statement\. 

This parameter is optional\.

You can specify the following values:

**true**  
The driver processes multiple SQL commands, separated by semicolons, in a Statement object\.

**false**  
The driver returns an error for multiple SQL commands in a single Statement\.

## fetchRingBufferSize<a name="jdbc20-fetchringbuffersize-option"></a>
+ **Default Value** – 1G
+ **Data Type** – String

This option specifies the size of the ring buffer used while fetching the result set\. You can specify a size in bytes, for example 1K for 1 KB, 5000 for 5,000 bytes, 1M for 1 MB, 1G for 1 GB, and so on\. You can also specify a percentage of heap memory\. The driver stops fetching rows upon reaching the limit\. Fetching resumes when the application reads rows and frees space in the ring buffer\. 

This parameter is optional\.

## ForceLowercase<a name="jdbc20-forcelowercase-option"></a>
+ **Default Value** – false
+ **Data Type** – Boolean

This option specifies whether the driver lowercases all database groups \(DbGroups\) sent from the identity provider to Amazon Redshift when using SSO authentication\. 

This parameter is optional\.

**true**  
The driver lowercases all database groups that are sent from the identity provider\.

**false**  
The driver doesn't alter database groups\.

## IAMDisableCache<a name="jdbc20-iamdisablecache-option"></a>
+ **Default Value** – false
+ **Data Type** – Boolean

This option specifies whether the IAM credentials are cached\.

This parameter is optional\.

**true**  
The IAM credentials aren't cached\.

**false**  
The IAM credentials are cached\. This improves performance when requests to the API gateway are throttled, for instance\.

## IAMDuration<a name="jdbc20-iamduration-option"></a>
+ **Default Value** – 900
+ **Data Type** – Integer

The length of time, in seconds, until the temporary IAM credentials expire\. 
+ **Minimum value** – 900
+ **Maximum value ** – 3,600

This parameter is optional\.

## IdP\_Host<a name="jdbc20-idp_host-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The IdP \(identity provider\) host you are using to authenticate into Amazon Redshift\. This can be specified in either the connection string or in a profile\. 

This parameter is optional\.

## IdP\_Port<a name="jdbc20-idp_port-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The port used by an IdP \(identity provider\)\. This can be specified in either the connection string or in a profile\. 

This parameter is optional\.

## IdP\_Tenant<a name="jdbc20-idp_tenant-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The Azure AD tenant ID for your Amazon Redshift application\. 

This parameter is required if authenticating through the Azure AD service\.

## IdP\_Response\_Timeout<a name="jdbc20-idp_response_timeout-option"></a>
+ **Default Value** – 120
+ **Data Type** – Integer

The amount of time, in seconds, that the driver waits for the SAML response from the identity provider when using the SAML or Azure AD services through a browser plugin\. 

This parameter is optional\.

## IniFile<a name="jdbc20-inifile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The full path of the \.ini file, including file name\. For example:

```
IniFile="C:\tools\rsjdbc.ini"
```

For information about the \.ini file, see [Creating initialization \(\.ini\) files for JDBC driver version 2\.0](jdbc20-ini-file.md)\.

This parameter is optional\.

## IniSection<a name="jdbc20-inisection-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of a section in the \.ini file containing the configuration options\. For information about the \.ini file, see [Creating initialization \(\.ini\) files for JDBC driver version 2\.0](jdbc20-ini-file.md)\. 

The following example specifies the \[Prod\] section of the \.ini file:

```
IniSection="Prod"
```

This parameter is optional\.

## Login\_URL<a name="jdbc20-login_url-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The URL for the resource on the identity provider's website when using the SAML or Azure AD services through a browser plugin\. 

This parameter is required if authenticating with the SAML or Azure AD services through a browser plugin\.

## LoginTimeout<a name="jdbc20-logintimeout-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The number of seconds to wait before timing out when connecting and authenticating to the server\. If establishing the connection takes longer than this threshold, then the connection is aborted\. 

When this property is set to 0, connections don't time out\.

This parameter is optional\.

## loginToRp<a name="jdbc20-logintorp-option"></a>
+ **Default Value** – `urn:amazon:webservices`
+ **Data Type** – String

The relying party trust that you want to use for the AD FS authentication type\. 

This parameter is optional\.

## LogLevel<a name="jdbc20-loglevel-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

Use this property to turn on or turn off logging in the driver and to specify the amount of detail included in log files\. 

Enable logging only long enough to capture an issue\. Logging decreases performance and can consume a large quantity of disk space\.

This parameter is optional\.

Set the parameter to one of the following values:

**0**  
Disable all logging\.

**1**  
Enable logging on the FATAL level, which logs very severe error events that will lead the driver to abort\.

**2**  
Enable logging on the ERROR level, which logs error events that might still allow the driver to continue running\.

**3**  
Enable logging on the WARNING level, which logs events that might result in an error if action is not taken\.

**4**  
Enable logging on the INFO level, which logs general information that describes the progress of the driver\.

**5**  
Enable logging on the DEBUG level, which logs detailed information that is useful for debugging the driver\.

**6**  
Enable logging on the TRACE level, which logs all driver activity\.

When logging is enabled, the driver produces the following log files in the location specified in the `LogPath` property:
+ ** `redshift_jdbc.log`** – File that logs driver activity that is not specific to a connection\.
+ **`redshift_jdbc_connection_[Number].log`** – File for each connection made to the database, where `[Number]` is a number that distinguishes each log file from the others\. This file logs driver activity that is specific to the connection\. 

If the LogPath value is invalid, the driver sends the logged information to the standard output stream, `System.out`\.

## LogPath<a name="jdbc20-logpath-option"></a>
+ **Default Value** – The current working directory\.
+ **Data Type** – String

The full path to the folder where the driver saves log files when the DSILogLevel property is enabled\. 

To be sure that the connection URL is compatible with all JDBC applications, we recommend that you escape the backslashes \(\\\) in your file path by typing another backslash\.

This parameter is optional\.

## Partner\_SPID<a name="jdbc20-partner_spid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The partner SPID \(service provider ID\) value to use when authenticating the connection using the PingFederate service\. 

This parameter is optional\.

## Password<a name="jdbc20-password-option"></a>
+ **Default Value** – None
+ **Data Type** – String

When connecting using IAM authentication through an IDP, this is the password for the IDP\_Host server\. When using standard authentication, this can be used for the Amazon Redshift database password instead of PWD\. 

This parameter is optional\.

## Plugin\_Name<a name="jdbc20-plugin_name-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The fully qualified class name to implement a specific credentials provider plugin\. 

This parameter is optional\.

The following provider options are supported:
+ **`AdfsCredentialsProvider`** – Active Directory Federation Service
+ **`AzureCredentialsProvider`** – Microsoft Azure Active Directory \(AD\) Service
+ **`BasicJwtCredentialsProvider`** – JSON Web Tokens \(JWT\) Service
+ **`BasicSamlCredentialsProvider`** – Security Assertion Markup Language \(SAML\) credentials which you can use with many SAML service providers\.
+ **`BrowserAzureCredentialsProvider`** – Browser Microsoft Azure Active Directory \(AD\) Service
+ **`BrowserSamlCredentialsProvider`** – Browser SAML for SAML services such as Okta, Ping, or ADFS
+ **`OktaCredentialsProvider`** – Okta Service
+ **`PingCredentialsProvider`** – PingFederate Service

## Preferred\_Role<a name="jdbc20-preferred_role-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The IAM role that you want to assume during the connection to Amazon Redshift\. 

This parameter is optional\.

## Profile<a name="jdbc20-profile-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The name of the profile to use for IAM authentication\. This profile contains any additional connection properties not specified in the connection string\. 

This parameter is optional\.

## PWD<a name="jdbc20-pwd-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The password corresponding to the Amazon Redshift user name that you provided using the property UID\. 

This parameter is optional\.

## queryGroup<a name="jdbc20-querygroup-option"></a>
+ **Default Value** – null
+ **Data Type** – String

This option assigns a query to a queue at runtime by assigning your query to the appropriate query group\. The query group is set for the session\. All queries that execute on the connection belong to this query group\. 

This parameter is optional\.

## ReadOnly<a name="jdbc20-readonly-option"></a>
+ **Default Value** – false
+ **Data Type** – Boolean

This property specifies whether the driver is in read\-only mode\. 

This parameter is optional\.

**true**  
The connection is in read\-only mode and cannot write to the data store\.

**false**  
The connection is not in read\-only mode and can write to the data store\.

## Region<a name="jdbc20-region-option"></a>
+ **Default Value** – null
+ **Data Type** – String

This option specifies the AWS Region where the cluster is located\. If you specify the StsEndPoint option, the Region option is ignored\. The Redshift `GetClusterCredentials` API operation also uses the Region option\. 

This parameter is optional\.

## reWriteBatchedInsertsSize<a name="jdbc20-rewritebatchedinsertssize-option"></a>
+ **Default Value** – 128
+ **Data Type** – Integer

This option enables optimization to rewrite and combine compatible INSERT statements that are batched\. This value must increase exponentially by the power of 2\. 

This parameter is optional\.

## roleArn<a name="jdbc20-rolearn-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The Amazon Resource Name \(ARN\) of role\. Make sure to specify this parameter when you specify BasicJwtCredentialsProvider for the Plugin\_Name option\. You specify the ARN in the following format: 

`arn:partition:service:region:account-id:resource-id`

This parameter is required if you specify BasicJwtCredentialsProvider for the Plugin\_Name option\.

## roleSessionName<a name="jdbc20-roleaessionname-option"></a>
+ **Default Value** – jwt\_redshift\_session
+ **Data Type** – String

An identifier for the assumed role session\. Typically, you pass the name or identifier that is associated with the user of your application\. The temporary security credentials that your application uses are associated with that user\. You can specify this parameter when you specify BasicJwtCredentialsProvider for the Plugin\_Name option\. 

This parameter is optional\.

## SecretAccessKey<a name="jdbc20-secretaccesskey-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The IAM access key for the user or role\. If this is specified, then AccessKeyID must also be specified\. 

This parameter is optional\.

## SessionToken<a name="jdbc20-sessiontoken-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The temporary IAM session token associated with the IAM role you are using to authenticate\. 

This parameter is optional\.

## SocketTimeout<a name="jdbc20-sockettimeout-option"></a>
+ **Default Value** – 0
+ **Data Type** – Integer

The number of seconds to wait during socket read operations before timing out\. If the operation takes longer than this threshold, then the connection is closed\. When this property is set to 0, the connection doesn't time out\. 

This parameter is optional\.

## SSL<a name="jdbc20-ssl-option"></a>
+ **Default Value** – TRUE
+ **Data Type** – String

Use this property to turn on or turn off SSL for the connection\. 

This parameter is optional\.

You can specify the following values:

**TRUE**  
The driver connects to the server through SSL\.

**FALSE**  
The driver connects to the server without using SSL\. This option is not supported with IAM authentication\.

Alternatively, you can configure the AuthMech property\.

## SSL\_Insecure<a name="jdbc20-ssl_insecure-option"></a>
+ **Default Value** – true
+ **Data Type** – String

This property indicates whether the IDP hosts server certificate should be verified\.

This parameter is optional\.

You can specify the following values:

**true**  
The driver doesn't check the authenticity of the IDP server certificate\.

**false**  
The driver checks the authenticity of the IDP server certificate\.

## SSLCert<a name="jdbc20-sslcert-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The full path of a \.pem or \.crt file containing additional trusted CA certificates for verifying the Amazon Redshift server instance when using SSL\. 

This parameter is required if SSLKey is specified\.

## SSLFactory<a name="jdbc20-sslfactory-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The SSL factory to use when connecting to the server through TLS/SSL without using a server certificate\. 

## SSLKey<a name="jdbc20-sslkey-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The full path of the \.der file containing the PKCS8 key file for verifying the certificates specified in SSLCert\. 

This parameter is required if SSLCert is specified\.

## SSLMode<a name="jdbc20-sslmode-option"></a>
+ **Default Value** – verify\-ca
+ **Data Type** – String

Use this property to specify how the driver validates certificates when TLS/SSL is enabled\. 

This parameter is optional\.

You can specify the following values:

**verify\-ca**  
The driver verifies that the certificate comes from a trusted certificate authority \(CA\)\.

**verify\-full**  
The driver verifies that the certificate comes from a trusted CA and that the host name in the certificate matches the host name specified in the connection URL\.

## SSLPassword<a name="jdbc20-sslpassword-option"></a>
+ **Default Value** – 0
+ **Data Type** – String

The password for the encrypted key file specified in SSLKey\. 

This parameter is required if SSLKey is specified and the key file is encrypted\.

## SSLRootCert<a name="jdbc20-sslrootcert-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The full path of a \.pem or \.crt file containing the root CA certificate for verifying the Amazon Redshift Server instance when using SSL\. 

## StsEndpointUrl<a name="jdbc20-stsendpointurl-option"></a>
+ **Default Value** – Null
+ **Data Type** – String

You can specify an AWS Security Token Service \(AWS STS\) endpoint\. If you specify this option, the Region option is ignored\. You can only specify a secure protocol \(HTTPS\) for this endpoint\. 

## TCPKeepAlive<a name="jdbc20-tcpkeepalive-option"></a>
+ **Default Value** – TRUE
+ **Data Type** – String

Use this property to turn on or turn off TCP keepalives\. 

This parameter is optional\.

You can specify the following values:

**TRUE**  
The driver uses TCP keepalives to prevent connections from timing out\.

**FALSE**  
The driver doesn't use TCP keepalives\.

## UID<a name="jdbc20-uid-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The user name that you use to access the database\. 

This parameter is required\.

## User<a name="jdbc20-user-option"></a>
+ **Default Value** – None
+ **Data Type** – String

When connecting using IAM authentication through an IDP, this is the user name for the idp\_host server\. When using standard authentication this can be used for the Amazon Redshift database user name\. 

This parameter is optional\.

## webIdentityToken<a name="jdbc20-webidentitytoken-option"></a>
+ **Default Value** – None
+ **Data Type** – String

The OAuth 2\.0 access token or OpenID Connect ID token that is provided by the identity provider\. Your application must get this token by authenticating the user of your application with a web identity provider\. Make sure to specify this parameter when you specify BasicJwtCredentialsProvider for the Plugin\_Name option\. 

This parameter is required if you specify BasicJwtCredentialsProvider for the Plugin\_Name option\.