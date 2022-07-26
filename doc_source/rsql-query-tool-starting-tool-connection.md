# Connect to a cluster with Amazon Redshift RSQL<a name="rsql-query-tool-starting-tool-connection"></a>

## Connecting without a DSN<a name="rsql-query-tool-starting-tool-connection-dsn-less-example"></a>

1. On the Amazon Redshift console, choose the cluster you want to connect to and note the endpoint, database, and port\.

1. At a command prompt, specify the connection information by using command\-line parameters\.

   ```
   rsql -h <endpoint> -U <username> -d <databasename> -p <port>
   ```

    Here, the following apply: 
   +  *<endpoint>* is the **Endpoint** you recorded in the previous step\.
   +  *<username>* is the name of a user with permissions to connect to the cluster\.
   +  *<databasename>* is the **Database Name** you recorded in the previous step\.
   +  *<port>* is the **Port** you recorded in the previous step\. *<port>* is an optional parameter which has a default value of 5439\.

   An example follows\.

   ```
   rsql -h testcluster.example.amazonaws.com -U user1 -d dev -p 5439
   ```

1.  At the password prompt, enter the password for the *<username>* user\.

   A successful connection response looks like the following\.

   ```
   % rsql -h testcluster.example.com -d dev -U user1 -p 5349
   Password for user user1:
   DSN-less Connected
   DBMS Name: Amazon Redshift
   Driver Name: Amazon Redshift ODBC Driver
   Driver Version: 1.4.27.1000
   Rsql Version: 1.0.1
   Redshift Version: 1.0.29306
   Type "help" for help.
   
   (testcluster) user1@dev=#
   ```

The command to connect has the same parameters on Linux, Mac OS, and Windows\.

## Connecting using a DSN<a name="rsql-query-tool-starting-tool-connection-dsn-example"></a>

You can connect RSQL to Amazon Redshift by using a data source name \(DSN\) to simplify the organization of connection properties\. For more information, see [Configuring connection features](configure-odbc-connection.md#connection-config-features)\. This topic includes instructions for ODBC\-driver installation and descriptions for DSN properties\.

### Using a DSN connection with a password<a name="rsql-query-tool-starting-tool-connection-dsn-example-password"></a>

The following shows an example of a DSN\-connection configuration that uses a password\. The default `<path to driver>` for Mac OSX is `/opt/amazon/redshift/lib/libamazonredshiftodbc.dylib` and for Linux is `/opt/amazon/redshiftodbc/lib/64/libamazonredshiftodbc64.so`\.

```
[testuser]
Driver=/opt/amazon/redshiftodbc/lib/64/libamazonredshiftodbc64.so
SSLMode=verify-ca
Min_TLS=1.2
boolsaschar=0
Host=<server endpoint>
Port=<database port>
Database=<dbname>
UID=<username>
PWD=<password>
sslmode=prefer
```

The following output results from a successful connection\.

```
% rsql -D testuser
DSN Connected
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
Redshift Version: 1.0.29306
Type "help" for help.

(testcluster) user1@dev=#
```

### Using Single sign\-on DSN<a name="rsql-query-tool-starting-tool-connection-dsn"></a>

You can configure a DSN for single sign\-on \(SSO\) authentication\. The following shows an example of a DSN\-connection configuration that uses Okta SSO\.

```
[testokta]
Driver=<path to driver>
SSLMode=verify-ca
Min_TLS=1.2
boolsaschar=0
Host=<server endpoint>
clusterid=<cluster id>
region=<region name>
Database=<dbname>
locale=en-US
iam=1
plugin_name=<plugin name>
uid=<okta username>
pwd=<okta password>
idp_host=<idp endpoint>
app_id=<app id>
app_name=<app name>
preferred_role=<role arn>
```

Sample output from a successful connection\.

```
% rsql -D testokta 
DSN Connected
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
Redshift Version: 1.0.29306
Type "help" for help.

(testcluster) user1@dev=#
```

The following example shows an example of a DSN\-connection configuration that uses Azure SSO\.

```
[testazure]
Driver=<path to driver>
SSLMode=verify-ca
Min_TLS=1.2
boolsaschar=0
Host=<server endpoint>
Port=<cluster port>
clusterid=<cluster id>
region=<region name>
Database=<dbname>
locale=en-us
iam=1
plugin_name=<plugin name>
uid=<azure username>
pwd=<azure password>
idp_tenant=<Azure idp tenant uuid>
client_id=<Azure idp client uuid>
client_secret=<Azure idp client secret>
```

### Using a DSN connection with an IAM profile<a name="rsql-query-tool-starting-tool-connection-dsn-iam"></a>

You can connect to Amazon Redshift using your configured IAM profile\. The IAM profile must have privileges to call `GetClusterCredentials`\. The following example shows the DSN properties to use\. The `ClusterID` and `Region` parameters are required only if the `Host` is not an Amazon provided endpoint like `examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com`\.

```
[testiam]
Driver=Default
Host=testcluster.example.com
Database=dev
DbUser=testuser
ClusterID=rsqltestcluster
Region=us-east-1
IAM=1
Profile=default
```

The value for the `Profile` key is the named profile you choose from your AWS CLI credentials\. This example shows the credentials for the profile named `default`\.

```
$ cat .aws/credentials
[default]
aws_access_key_id = ASIAIOSFODNN7EXAMPLE 
aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

The following shows the connection response\.

```
$ rsql -D testiam
DSN Connected
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
Redshift Version: 1.0.29306
Type "help" for help.

(testcluster) testuser@dev=>
```

### Using a DSN connection with an Instance profile<a name="rsql-query-tool-starting-tool-connection-dsn-instance"></a>

You can connect to Amazon Redshift using your Amazon EC2 instance profile\. The instance profile must have privileges to call `GetClusterCredentials`\. See example below for the DSN properties to use\. The `ClusterID` and `Region` parameters are required only if the `Host` is not an Amazon provided endpoint like `examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com`\.

```
[testinstanceprofile]
Driver=Default
Host=testcluster.example.com
Database=dev
DbUser=testuser
ClusterID=rsqltestcluster
Region=us-east-1
IAM=1
Instanceprofile=1
```

The following shows the connection response\.

```
$ rsql -D testinstanceprofile
DSN Connected
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
Redshift Version: 1.0.29306
Type "help" for help.

(testcluster) testuser@dev=>
```

### Using a DSN connection with the default credential provider chain<a name="rsql-query-tool-starting-tool-connection-dsn-provider-chain"></a>

To connect using the default credential provider chain, specify only the IAM property, and Amazon Redshift RSQL will attempt to acquire credentials in the order described in [Working with AWS Credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) in the AWS SDK for Java\. At least one of the providers in the chain must have `GetClusterCredentials` permission\. This is useful for connecting from ECS containers, for example\.

```
[iamcredentials]
Driver=Default
Host=testcluster.example.com
Database=dev
DbUser=testuser
ClusterID=rsqltestcluster
Region=us-east-1
IAM=1
```