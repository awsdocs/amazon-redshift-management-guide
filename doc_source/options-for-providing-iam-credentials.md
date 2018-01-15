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

  You can provide the plugin name and related values in the form of JDBC or ODBC settings or by using a profile\. 

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

You can supply the IAM credentials options and GetClusterCredentials options as settings in named profiles in your AWS configuration file\. Provide the profile name by using the Profile JDBC option\. 

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

For more information, see [Named Profiles](http://docs.aws.amazon.com/cli/latest/userguide/cli-multiple-profiles.html) in the AWS Command Line Interface User Guide\. 