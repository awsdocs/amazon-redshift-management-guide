# Connect to your cluster by using the psql tool<a name="connecting-from-psql"></a>

After you create an Amazon Redshift cluster, you can use psql, a terminal\-based front end from PostgreSQL, to query the data in your cluster\. You can type the queries interactively or read them from a file\. To connect from psql, you must specify the cluster endpoint, database, and port\.

**Note**  
 Amazon Redshift does not provide the psql tool; it is installed with PostgreSQL\. For information about using psql, go to [https://www\.postgresql\.org/docs/8\.4/static/app\-psql\.html](https://www.postgresql.org/docs/8.4/static/app-psql.html)\. For information about installing the PostgreSQL client tools, select your operating system from the PostgreSQL binary downloads page at [https://www\.postgresql\.org/download/](https://www.postgresql.org/download/)\.  
If you have trouble connecting from a Microsoft Windows prompt due to an invalid `client_encoding`, set the `PGCLIENTENCODING` environment variable to UTF\-8 before running psql\.   

```
set PGCLIENTENCODING=UTF8
```

## Connect by using the psql defaults<a name="connecting-from-psql-default"></a>

By default, psql does not validate the Amazon Redshift service; it makes an encrypted connection by using Secure Sockets Layer \(SSL\)\.

**To connect by using psql defaults**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Choose one of the following steps depending on which Amazon Redshift console you are using:
   + \(New console\) In the navigation pane, choose **CLUSTERS**\. Then choose the cluster name from the list to open its details\. On the **Properties** tab, in the **Database configurations** section, record the **Database name** and **Port**\. View the **Connection details** section and record the **Endpoint** which is in the following form: 

     ```
     endpoint:port/databasename
     ```
   + \(Original console\) In the navigation pane, choose **Clusters**\. Choose your cluster to open it\. Under **Cluster Database Properties**, record the values of **Endpoint**, **Port**, and **Database Name**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-database-properties.png)

1. At a command prompt, specify the connection information by using either command line parameters or a connection information string\. To use parameters:

   ```
   psql -h <endpoint> -U <userid> -d <databasename> -p <port>
   ```

    Where: 
   +  *<endpoint>* is the **Endpoint** you recorded in the previous step\.
   +  *<userid>* is a user ID with permissions to connect to the cluster\.
   +  *<databasename>* is the **Database Name** you recorded in the previous step\.
   +  *<port>* is the **Port** you recorded in the previous step\.

   For example:

   ```
   psql -h examplecluster.<XXXXXXXXXXXX>.us-west-2.redshift.amazonaws.com -U masteruser -d dev -p 5439
   ```

1.  At the psql password prompt, enter the password for the *<userid>* user\.

 You are connected to the cluster, and you can interactively enter commands\.

## Connect by using a certificate<a name="connecting-from-psql-ssl"></a>

To control whether psql authenticates the service using a certificate, you must use a connection information string to specify connection information, and specify the `sslmode` keyword\. By default, psql operates with `sslmode=prefer`\. To specify that psql opens an encrypted connection and uses an Amazon Redshift certificate to verify the service, download an Amazon Redshift certificate to your computer\. Specify `verify-full` unless you use a DNS alias\. If you use a DNS alias, select v`erify-ca`\. Specify `sslrootcert` with the location of the certificate\. For more information about `sslmode`, see [Configuring security options for connections](connecting-ssl-support.md)\. 

 For more information about connection information string parameters, see [https://www\.postgresql\.org/docs/8\.4/static/libpq\-connect\.html](https://www.postgresql.org/docs/8.4/static/libpq-connect.html)\.

**To connect by using a certificate**

1.  Save the download the [Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt) a \.crt file to your computer\. If you do a **File\\Save as** using Internet Explorer, specify the file type as **Text file \(\*\.txt\)** and delete the \.txt extension\. For example, save it as the file `C:\MyDownloads\redshift-ca-bundle.crt`\.

1. Choose one of the following steps depending on which Amazon Redshift console you are using:
   + \(New console\) In the navigation pane, choose **CLUSTERS**\. Then choose the cluster name from the list to open its details\. On the **Properties** tab, in the **Database configurations** section, record the **Database name** and **Port**\. View the **Connection details** section and record the **Endpoint** which is in the following form: 

     ```
     endpoint:port/databasename
     ```
   + \(Original console\) In the navigation pane, choose **Clusters**\. Choose your cluster to open it\. Under **Cluster Database Properties**, record the values of **Endpoint**, **Port**, and **Database Name**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-database-properties.png)

1. At a command prompt, specify the connection information using a connection information string:

   ```
   psql "host=<endpoint> user=<userid> dbname=<databasename> port=<port> sslmode=verify-ca sslrootcert=<certificate>"
   ```

    Where: 
   +  *<endpoint>* is the **Endpoint** you recorded in the previous step\.
   +  *<userid>* is a user ID with permissions to connect to the cluster\.
   +  *<databasename>* is the **Database Name** you recorded in the previous step\.
   +  *<port>* is the **Port** you recorded in the previous step\.
   +  *<certificate>* is the full path to the certificate file\. On Windows systems, the certificate path must be specified using Linux\-style / separators instead of the Windows \\ separator\.

     On Linux and macOS X operating systems, the path is 

     ```
     ~/.postgresql/root.crt
     ```

     On Microsoft Windows, the path is 

     ```
     %APPDATA%/postgresql/root.crt
     ```

   For example:

   ```
   psql "host=examplecluster.<XXXXXXXXXXXX>.us-west-2.redshift.amazonaws.com user=masteruser dbname=dev port=5439 sslmode=verify-ca sslrootcert=C:/MyDownloads/redshift-ca-bundle.crt"
   ```

1.  At the psql password prompt, enter the password for the *<userid>* user\.

You are connected to the cluster, and you can interactively enter commands\.