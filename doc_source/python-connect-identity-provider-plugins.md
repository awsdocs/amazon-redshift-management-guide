# Using identity provider plugins<a name="python-connect-identity-provider-plugins"></a>

For general information on how to use identity provider plugins, see [Options for providing IAM credentials](options-for-providing-iam-credentials.md)\.

## Authentication using the ADFS identity provider plugin<a name="python-connect-identity-provider-active-dir"></a>

Following is an example of using the Active Directory Federation Service \(ADFS\) identity provider plugin to authenticate a user connecting to an Amazon Redshift database\.

```
>>> con = redshift_connector.connect(
    iam=True,
    database='dev',
    host='my-testing-cluster.abc.us-east-2.redshift.amazonaws.com',
    cluster_identifier='my-testing-cluster',
    credentials_provider='AdfsCredentialsProvider',
    user='brooke@myadfshostname.com',
    password='Hunter2',
    idp_host='myadfshostname.com'
)
```

## Authentication using the Azure identity provider plugin<a name="python-connect-identity-provider-azure"></a>

Following is an example of authentication using the Azure identity provider plugin\. You can create values for a `client_id` and `client_secret` for an Azure Enterprise application as shown following\.  

```
>>>  con = redshift_connector.connect(
    iam=True,
    database='dev',
    host='my-testing-cluster.abc.us-east-2.redshift.amazonaws.com',
    cluster_identifier='my-testing-cluster',
    credentials_provider='AzureCredentialsProvider',
    user='brooke@myazure.org',
    password='Hunter2',
    idp_tenant='my_idp_tenant',
    client_id='my_client_id',
    client_secret='my_client_secret',
    preferred_role='arn:aws:iam:123:role/DataScientist'
)
```

## Authentication using Azure Browser identity provider plugin<a name="python-connect-identity-provider-azure-browser"></a>

Following is an example of using the Azure Browser identity provider plugin to authenticate a user connecting to an Amazon Redshift database\.

Multi\-factor authentication occurs in the browser, where the username and password are provided by the user\.

```
>>>con = redshift_connector.connect(
    iam=True,
    database='dev',
    host='my-testing-cluster.abc.us-east-2.redshift.amazonaws.com',
    cluster_identifier='my-testing-cluster',
    credentials_provider='BrowserAzureCredentialsProvider',
    idp_tenant='my_idp_tenant',
    client_id='my_client_id',
)
```

## Authentication using the Okta identity provider plugin<a name="python-connect-identity-provider-okta"></a>

Following is an example of authentication using the Okta identity provider plugin\. You can obtain the values for `idp_host`, `app_id` and `app_name` through the Okta application\.

```
>>> con = redshift_connector.connect(
    iam=True,
    database='dev',
    host='my-testing-cluster.abc.us-east-2.redshift.amazonaws.com',
    cluster_identifier='my-testing-cluster',
    credentials_provider='OktaCredentialsProvider',
    user='brooke@myazure.org',
    password='hunter2',
    idp_host='my_idp_host',
    app_id='my_first_appetizer',
    app_name='dinner_party'
)
```

## Authentication using JumpCloud with a generic SAML browser identity provider plugin<a name="python-connect-identity-provider-jumpcloud"></a>

Following is an example of using JumpCloud with a generic SAML browser identity provider plugin for authentication\.

The password parameter is required\. However, you don't have to enter this parameter because multi\-factor authentication occurs in the browser\.

```
>>> con = redshift_connector.connect(
    iam=True,
    database='dev',
    host='my-testing-cluster.abc.us-east-2.redshift.amazonaws.com',
    cluster_identifier='my-testing-cluster',
    credentials_provider='BrowserSamlCredentialsProvider',
    user='brooke@myjumpcloud.org',
    password='',
    login_url='https://sso.jumpcloud.com/saml2/plustwo_melody'
)
```