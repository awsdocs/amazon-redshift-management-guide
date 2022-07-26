# Configuring authentication<a name="odbc20-authentication-ssl"></a>

To protect data from unauthorized access, Amazon Redshift data stores require all connections to be authenticated using user credentials\.

The following table illustrates the required and optional connection options for each authentication method that can be used to connect to the Amazon Redshift ODBC driver version 2\.x:


**ODBC authentication method required and optional connection options**  

| Authentication Method | Required | Optional | 
| --- | --- | --- | 
|  Standard  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |   | 
|  IAM Profile  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  IAM Credentials  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  AD FS  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  Azure AD  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  JWT  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  | 
|  Okta  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  Ping Federate  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  Browser Azure AD  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  Browser SAML  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 
|  Auth Profile  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |   | 
|  Browser Azure AD OAUTH2  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/odbc20-authentication-ssl.html)  **ClusterID** and **Region** must be set in **Host** if they are not set separately\.   | 

## Using an external credentials service<a name="odbc20-authentication-external"></a>

In addition to built\-in support for AD FS, Azure AD, and Okta, the Windows version of the Amazon Redshift ODBC driver also provides support for other credentials services\. The driver can authenticate connections using any SAML\-based credential provider plugin of your choice\. 

To configure an external credentials service on Windows:

1. Create an IAM profile that specifies the credential provider plugin and other authentication parameters as needed\. The profile must be ASCII\-encoded, and must contain the following key\-value pair, where `PluginPath` is the full path to the plugin application: 

   ```
   plugin_name = PluginPath
   ```

   For example:

   ```
   plugin_name = C:\Users\kjson\myapp\CredServiceApp.exe 
   ```

   For information on how to create a profile, see [ Using a Configuration Profile ](https://docs.aws.amazon.com/redshift/latest/mgmt/options-for-providing-iam-credentials.html#using-configuration-profile) in the Amazon Redshift Cluster Management Guide\.

1. Configure the driver to use this profile\. The driver detects and uses the authentication settings specified in the profile\.