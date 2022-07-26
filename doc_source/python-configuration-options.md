# Configuration options for the Amazon Redshift Python connector<a name="python-configuration-options"></a>

Following, you can find descriptions for the options that you can specify for the Amazon Redshift Python connector\.

## access\_key\_id<a name="python-access-key-id-option"></a>
+ **Default value** – None
+ **Data type** – String

The access key for the IAM role or IAM user configured for IAM database authentication\. 

This parameter is optional\.

## allow\_db\_user\_override<a name="python-allow-db-user-override-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

True  
Specifies that the connector uses the `DbUser` value from the Security Assertion Markup Language \(SAML\) assertion\.

False  
Specifies that the value in the `DbUser` connection parameter is used\.

This parameter is optional\.

## app\_name<a name="python-app-name-option"></a>
+ **Default value** – None
+ **Data type** – String

The name of the identity provider \(IdP\) application used for authentication\. 

This parameter is optional\.

## auth\_profile<a name="python-auth-profile-option"></a>
+ **Default value** – None
+ **Data type** – String

The name of an Amazon Redshift authentication profile having connection properties as JSON\. For more information about naming connection parameters, see the `RedshiftProperty` class\. The `RedshiftProperty` class stores connection parameters provided by the end user and, if applicable, generated during the IAM authentication process \(for example, temporary IAM credentials\)\. For more information, see the [RedshiftProperty class](https://github.com/aws/amazon-redshift-python-driver/blob/master/redshift_connector/redshift_property.py#L9)\. 

This parameter is optional\.

## auto\_create<a name="python-auto-create-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

A value that indicates whether to create the user if the user doesn't exist\. 

This parameter is optional\.

## client\_id<a name="python-client-id-option"></a>
+ **Default value** – None
+ **Data type** – String

The client ID from Azure IdP\. 

This parameter is optional\.

## client\_secret<a name="python-client-secret-option"></a>
+ **Default value** – None
+ **Data type** – String

The client secret from Azure IdP\. 

This parameter is optional\.

## cluster\_identifier<a name="python-cluster-identifier-option"></a>
+ **Default value** – None
+ **Data type** – String

The cluster identifier of the Amazon Redshift cluster\. 

This parameter is optional\.

## credentials\_provider<a name="python-credential-provider-option"></a>
+ **Default value** – None
+ **Data type** – String

The IdP that is used for authenticating with Amazon Redshift\. Following are valid values: 
+ `OktaCredentialsProvider`
+ `AzureCredentialsProvider`
+ `BrowserAzureCredentialsProvider`
+ `PingCredentialsProvider`
+ `BrowserSamlCredentialsProvider`
+ `AdfsCredentialsProvider`

This parameter is optional\.

## database<a name="python-database-option"></a>
+ **Default value** – None
+ **Data type** – String

The name of the database to which you want to connect\. 

This parameter is optional\.

## database\_metadata\_current\_db\_only<a name="python-database-metadata-current-db-only-option"></a>
+ **Default value** – True
+ **Data type** – Boolean

A value that indicates whether an application supports multidatabase datashare catalogs\. The default value of True indicates that the application doesn't support multidatabase datashare catalogs for backward compatibility\. 

This parameter is optional\.

## db\_groups<a name="python-db-groups-option"></a>
+ **Default value** – None
+ **Data type** – String

A comma\-separated list of existing database group names that the user indicated by DbUser joins for the current session\. 

This parameter is optional\.

## db\_user<a name="python-db-user-option"></a>
+ **Default value** – None
+ **Data type** – String

The user ID to use with Amazon Redshift\. 

This parameter is optional\.

## endpoint\_url<a name="python-endpoint-url-option"></a>
+ **Default value** – None
+ **Data type** – String

The Amazon Redshift endpoint URL\. This option is only for AWS internal use\. 

This parameter is required\.

## group\_federation<a name="python-group-federation-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

This option specifies whether to use Amazon Redshift IDP groups\.

This parameter is optional\.

**true**  
Use Amazon Redshift Identity Provider \(IDP\) groups\.

**false**  
Use STS API and GetClusterCredentials for user federation and specify **db\_groups** for the connection\.

## host<a name="python-host-option"></a>
+ **Default value** – None
+ **Data type** – String

The hostname of Amazon Redshift cluster\. 

This parameter is optional\.

## iam<a name="python-iam-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

IAM authentication is enabled\. 

This parameter is required\.

## iam\_disable\_cache<a name="python-iam-disable-cache-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

This option specifies whether the IAM credentials are cached\. By default, the IAM credentials are cached\. This improves performance when requests to the API gateway are throttled\. 

This parameter is optional\.

## idpPort<a name="python-idp-port-option"></a>
+ **Default value** – 7890
+ **Data type** – Integer

The listen port to which IdP sends the SAML assertion\. 

This parameter is required\.

## idp\_response\_timeout<a name="python-idp-response-timeout-option"></a>
+ **Default value** – 120
+ **Data type** – Integer

The timeout for retrieving SAML assertion from IdP\. 

This parameter is required\.

## idp\_tenant<a name="python-idp-tenant-option"></a>
+ **Default value** – None
+ **Data type** – String

The IdP tenant\. 

This parameter is optional\.

## listen\_port<a name="python-listen-port-option"></a>
+ **Default value** – 7890
+ **Data type** – Integer

The listen port to which the IdP sends the SAML assertion\. 

This parameter is optional\.

## login\_url<a name="python-login-url-option"></a>
+ **Default value** – None
+ **Data type** – String

The SSO Url for the IdP\. 

This parameter is optional\.

## max\_prepared\_statements<a name="python-max-prepared-statements-option"></a>
+ **Default value** – 1000
+ **Data type** – Integer

The maximum number of prepared statements that can be open concurrently\. 

This parameter is required\.

## numeric\_to\_float<a name="python-numeric-to-float-option"></a>
+ **Default value** – False
+ **Data type** – Boolean

This option specifies if the connector converts numeric data type values from decimal\.Decimal to float\. By default, the connector receives numeric data type values as decimal\.Decimal and does not convert them\. 

We don't recommend enabling numeric\_to\_float for use cases that require precision, as results may be rounded\. 

For more information on decimal\.Decimal and the tradeoffs between it and float, see [decimal — Decimal fixed point and floating point arithmetic](https://docs.python.org/3/library/decimal.html) on the Python website\. 

This parameter is optional\.

## partner\_sp\_id<a name="python-partner-sp-id-option"></a>
+ **Default value** – None
+ **Data type** – String

The Partner SP ID used for authentication with Ping\. 

This parameter is optional\.

## password<a name="python-password-option"></a>
+ **Default value** – None
+ **Data type** – String

The password to use for authentication\. 

This parameter is optional\.

## port<a name="python-port-option"></a>
+ **Default value** – 5439
+ **Data type** – Integer

The port number of the Amazon Redshift cluster\. 

This parameter is required\.

## preferred\_role<a name="python-preferred-role-option"></a>
+ **Default value** – None
+ **Data type** – String

The IAM role preferred for the current connection\. 

This parameter is optional\.

## principal\_arn<a name="python-principal-arn-option"></a>
+ **Default value** – None
+ **Data type** – String

The Amazon Resource Name \(ARN\) of the IAM user or role for which you are generating a policy\. 

This parameter is optional\.

## profile<a name="python-profile-option"></a>
+ **Default value** – None
+ **Data type** – String

The name of a profile in an AWS credentials file that contains AWS credentials\. 

This parameter is optional\.

## provider\_name<a name="python-provider_name-option"></a>
+ **Default value** – None
+ **Data type** – String

The name of the Redshift Native Authentication Provider\. 

This parameter is optional\.

## region<a name="python-region-option"></a>
+ **Default value** – None
+ **Data type** – String

The AWS Region where the cluster is located\. 

This parameter is optional\.

## role\_arn<a name="python-role-arn-option"></a>
+ **Default value** – None
+ **Data type** – String

The Amazon Resource Name \(ARN\) of the role that the caller is assuming\. This parameter is used by the provider indicated by `JwtCredentialsProvider`\. 

For the `JwtCredentialsProvider` provider, this parameter is mandatory\. Otherwise, this parameter is optional\.

## role\_session\_name<a name="python-role-session-name-option"></a>
+ **Default value** – jwt\_redshift\_session
+ **Data type** – String

An identifier for the assumed role session\. Typically, you pass the name or identifier that is associated with the user who is using your application\. The temporary security credentials that your application uses are associated with that user\. This parameter is used by the provider indicated by `JwtCredentialsProvider`\. 

This parameter is optional\.

## scope<a name="python-scope-option"></a>
+ **Default value** – None
+ **Data type** – String

A space\-separated list of scopes to which the user can consent\. You specify this parameter so that your application can get consent for APIs that you want to call\. You can specify this parameter when you specify BrowserAzureOAuth2CredentialsProvider for the Plugin\_Name option\.

This parameter is required for the BrowserAzureOAuth2CredentialsProvider plug\-in\.

## secret\_access\_key\_id<a name="python-secret-access-key-id-option"></a>
+ **Default value** – None
+ **Data type** – String

The secret access key for the IAM role or user configured for IAM database authentication\. 

This parameter is optional\.

## session\_token<a name="python-session-token-option"></a>
+ **Default value** – None
+ **Data type** – String

The access key for the IAM role or user configured for IAM database authentication\. This parameter is required if temporary AWS credentials are being used\. 

This parameter is optional\.

## serverless\_acct\_id<a name="python-serverless-acct-id-option"></a>
+ **Default value** – None
+ **Data type** – String

The Amazon Redshift Serverless account ID\.

This parameter is optional\.

## serverless\_work\_group<a name="python-serverless-work-group-option"></a>
+ **Default value** – None
+ **Data type** – String

The Amazon Redshift Serverless workgroup name\.

This parameter is optional\.

## ssl<a name="python-ssl-option"></a>
+ **Default value** – True
+ **Data type** – Boolean

Secure Sockets Layer \(SSL\) is enabled\. 

This parameter is required\.

## ssl\_insecure<a name="python-ssl-insecure-option"></a>
+ **Default value** – True
+ **Data type** – Boolean

A value that specifies whether the IdP hosts server certificate is to be verified\. 

This parameter is optional\.

## sslmode<a name="python-sslmode-option"></a>
+ **Default value** – verify\-ca
+ **Data type** – String

The security of the connection to Amazon Redshift\. You can specify either of the following: 
+ verify\-ca
+ verify\-full

This parameter is required\.

## timeout<a name="python-timeout-option"></a>
+ **Default value** – None
+ **Data type** – Integer

The number of seconds before the connection to the server times out\. 

This parameter is optional\.

## user<a name="python-user-option"></a>
+ **Default value** – None
+ **Data type** – String

The user name to use for authentication\. 

This parameter is optional\.

## web\_identity\_token<a name="python-web-identity-token-option"></a>
+ **Default value** – None
+ **Data type** – String

The OAuth 2\.0 access token or OpenID Connect ID token that is provided by the identity provider\. Make sure that your application gets this token by authenticating the user who is using your application with a web identity provider\. The provider indicated by `JwtCredentialsProvider` uses this parameter\. 

For the `JwtCredentialsProvider` provider, this parameter is mandatory\. Otherwise, this parameter is optional\.