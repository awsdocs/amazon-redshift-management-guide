# Using an authentication profile to connect to Amazon Redshift<a name="connecting-with-authentication-profiles"></a>

If you have many connections to Amazon Redshift, it can be difficult to manage settings for all of them\. Often, each JDBC or ODBC connection uses specific configuration options\. By using an authentication profile, you can store connection options together\. This way, your users can choose a profile to connect with and avoid managing settings for individual options\. Profiles can apply to various scenarios and user types\.

After you create an authentication profile, users can add the ready\-to\-use profile to a connection string\. By doing this, they can connect to Amazon Redshift with the right settings for each role and use case\.

For Amazon Redshift API information, see [CreateAuthenticationProfile](https://docs.aws.amazon.com/redshift/latest/APIReference/redshift-api.pdf#API_CreateAuthenticationProfile)\.  

## Creating an authentication profile<a name="connecting-with-authentication-profiles-creating"></a>

Using the AWS CLI, you create an authentication profile with the `create-authentication-profile` command\. This assumes that you have an existing Amazon Redshift cluster and an existing database\. Your credentials must have permission to connect to the Amazon Redshift database and rights to fetch the authentication profile\. You provide the configuration options as a JSON string, or reference a file containing your JSON string\. 

```
create-authentication-profile --authentication-profile-name<value: String> --authentication-profile-content<value: String>
```

 The following example creates a profile called `ExampleProfileName`\. Here, you can add keys and values that define your cluster name and other option settings, as a JSON string\. 

```
create-authentication-profile --authentication-profile-name "ExampleProfileName" --authentication-profile-content "{\"AllowDBUserOverride\":\"1\",\"Client_ID\":\"ExampleClientID\",\"App_ID\":\"ExampleAppID\",\"AutoCreate\":false,\"enableFetchRingBuffer\":true,\"databaseMetadataCurrentDbOnly\":true}"
}
```

 This command creates the profile with the specified JSON settings\. The following is returned, which indicates that the profile is created\. 

 `{"AuthenticationProfileName": "ExampleProfileName", "AuthenticationProfileContent": "{\"AllowDBUserOverride\":\"1\",\"Client_ID\":\"ExampleClientID\",\"App_ID\":\"ExampleAppID\",\"AutoCreate\":false,\"enableFetchRingBuffer\":true,\"databaseMetadataCurrentDbOnly\":true}" } `  

### Limitations and quotas for creating an authentication profile<a name="connecting-with-authentication-profiles-limitations"></a>

Each customer has a quota of ten \(10\) authentication profiles\.

Certain errors can occur with authentication profiles\. Examples are if you create a new profile with an existing name, or if you exceed your profile quota\. For more information, see [CreateAuthenticationProfile](https://docs.aws.amazon.com/redshift/latest/APIReference/redshift-api.pdf#API_CreateAuthenticationProfile)\. 

You can't store certain option keys and values for JDBC, ODBC, and Python connection strings in the authentication profile store: 
+ `AccessKeyID`
+ `access_key_id`
+ `SecretAccessKey`
+ `secret_access_key_id`
+ `PWD`
+ `Password`
+ `password`

You can't store the key or value `AuthProfile` in the profile store, for JDBC or ODBC connection strings\. For Python connections, you can’t store `auth_profile`\. 

Authentication profiles are stored in Amazon DynamoDB and managed by AWS\.

## Working with authentication profiles<a name="connecting-with-authentication-profiles-using"></a>

After you create an authentication profile, you can include the profile name as a connection option for JDBC version 2\.0 `AuthProfile`\. Using this connection option retrieves the stored settings\.

```
jdbc:redshift:iam://endpoint:port/database?AuthProfile=<Profile-Name>&AccessKeyID=<Caller-Access-Key>&SecretAccessKey=<Caller-Secret-Key>
```

The following is an example JDBC URL string\.

```
jdbc:redshift:iam://examplecluster:us-west-2/dev?AuthProfile="ExampleProfile"&AccessKeyID="AKIAIOSFODNN7EXAMPLE"&SecretAccessKey="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

Specify both the `AccessKeyID` and `SecretAccessKey` in the JDBC URL, along with the authentication profile name\.

You can also separate the configuration options with semicolon delimiters, such as in the following example, which includes options for logging\.

```
jdbc:redshift:iam://my_redshift_end_point:5439/dev?LogLevel=6;LogPath=/tmp;AuthProfile=my_profile;AccessKeyID="AKIAIOSFODNN7EXAMPLE";SecretAccessKey="wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

**Note**  
 Don't add confidential information to the authentication profile\. For example, don't store an `AccessKeyID` or `SecretAccessKey` value in an authentication profile\. The authentication profile store has rules to prohibit storage of secret keys\. You get an error if you try to store a key and value associated with sensitive information\. 

## Getting authentication profiles<a name="connecting-with-authentication-profiles-getting"></a>

To list existing authentication profiles, call the following command\.

```
describe-authentication-profiles --authentication-profile-name <value: String>
```

The following example shows two retrieved profiles\. All profiles are returned if you don't specify a profile name\.

\{ "AuthenticationProfiles": \[ \{ "AuthenticationProfileName": "testProfile1", "AuthenticationProfileContent": "\{\\"AllowDBUserOverride\\":\\"1\\",\\"Client\_ID\\":\\"ExampleClientID\\",\\"App\_ID\\":\\"ExampleAppID\\",\\"AutoCreate\\":false,\\"enableFetchRingBuffer\\":true,\\"databaseMetadataCurrentDbOnly\\":true\}" \}, \{ "AuthenticationProfileName": "testProfile2", "AuthenticationProfileContent": "\{\\"AllowDBUserOverride\\":\\"1\\",\\"Client\_ID\\":\\"ExampleClientID\\",\\"App\_ID\\":\\"ExampleAppID\\",\\"AutoCreate\\":false,\\"enableFetchRingBuffer\\":true,\\"databaseMetadataCurrentDbOnly\\":true\}" \} \] \} 