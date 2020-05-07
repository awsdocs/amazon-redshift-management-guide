# Transitioning to ACM certificates for SSL connections<a name="connecting-transitioning-to-acm-certs"></a>

Amazon Redshift is replacing the SSL certificates on your clusters with [AWS Certificate Manager \(ACM\)](https://aws.amazon.com/certificate-manager/) issued certificates\. ACM is a trusted public certificate authority \(CA\) that is trusted by most current systems\. You might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. 

This change affects you only if all of the following apply:
+  Your SQL clients or applications connect to Amazon Redshift clusters using SSL with the `sslMode` connection option set to `require`, `verify-ca`, or `verify-full` configuration option\. 
+  Your clusters are in any AWS Region except the China \(Beijing\) Region or the China \(Ningxia\) Region\. 
+ You aren't using the Amazon Redshift ODBC or JDBC drivers, or you use Amazon Redshift drivers before ODBC version 1\.3\.7\.1000 or JDBC version 1\.2\.8\.1005\. 

If this change affects you on commercial Amazon Redshift Regions, then you must update your current trust root CA certificates before October 23, 2017\. Amazon Redshift will transition your clusters to use ACM certificates between now and October 23, 2017\. The change should have very little or no effect on your cluster's performance or availability\.

If this change affects you on AWS GovCloud \(US\) Regions, then you must update your current trust root CA certificates before April 1, 2020 to avoid service interruption\. Beginning on this date, clients connecting to Amazon Redshift clusters using SSL encrypted connections need an additional trusted certificate authority \(CA\)\. Clients use trusted certificate authorities to confirm the identity of the Amazon Redshift cluster when they connect to it\. Your action is required to update your SQL clients and applications to use an updated certificate bundle that includes the new trusted CA\. 

To update your current trust root CA certificates, identify your use case and then follow the steps in that section\. 
+ [Using the latest Amazon Redshift ODBC or JDBC drivers](#connecting-transitioning-to-acm-latest-odbc-jdbc)
+ [Using earlier Amazon Redshift ODBC or JDBC drivers](#connecting-transitioning-to-acm-earlier-odbc-jdbc)
+ [Using other SSL connection types](#connecting-transitioning-to-acm-other-ssl-types)

## Using the latest Amazon Redshift ODBC or JDBC drivers<a name="connecting-transitioning-to-acm-latest-odbc-jdbc"></a>

The preferred method is to use the latest Amazon Redshift ODBC or JDBC drivers\. Amazon Redshift drivers beginning with ODBC version 1\.3\.7\.1000 and JDBC version 1\.2\.8\.1005 automatically manage the transition from an Amazon Redshift self\-signed certificate to an ACM certificate\. To download the latest drivers, see [Configuring an ODBC connection](configure-odbc-connection.md) or [Configuring a JDBC connection](configure-jdbc-connection.md)\. 

If you use the latest Amazon Redshift JDBC driver, it's best not to use `-Djavax.net.ssl.trustStore` in JVM options\. If you must use `-Djavax.net.ssl.trustStore`, import the [Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt) into the truststore it points to\. For more information, see [Importing the Amazon Redshift certificate authority bundle into a TrustStore](#importing-the-acm-bundle-to-truststore)\.

## Using earlier Amazon Redshift ODBC or JDBC drivers<a name="connecting-transitioning-to-acm-earlier-odbc-jdbc"></a>

If you must use an Amazon Redshift ODBC driver before version 1\.3\.7\.1000, then download the [Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt) and overwrite the old certificate file\. 
+ If your ODBC DSN is configured with `SSLCertPath`, overwrite the certificate file in the specified path\.
+ If `SSLCertPath` is not set, then overwrite the certificate file named `root.crt` in the driver DLL location\. 

If you must use an Amazon Redshift JDBC driver before version 1\.2\.8\.1005, then do one of the following:
+ If your JDBC connection string uses the `sslCert` option, remove the `sslCert` option\. Then import the [Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt) to your Java TrustStore\. For more information, see [Importing the Amazon Redshift certificate authority bundle into a TrustStore](#importing-the-acm-bundle-to-truststore)\. 
+ If you use the Java command line `-Djavax.net.ssl.trustStore` option, remove it from command line, if possible\. Then import the [Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt) to your Java TrustStore\. For more information, see [Importing the Amazon Redshift certificate authority bundle into a TrustStore](#importing-the-acm-bundle-to-truststore)\.

### Importing the Amazon Redshift certificate authority bundle into a TrustStore<a name="importing-the-acm-bundle-to-truststore"></a>

You can use `redshift-keytool.jar` to import CA certificates in the Amazon Redshift Certificate Authority bundle into a Java TrustStore or your private truststore\.

**To import the Amazon Redshift certificate authority bundle into a TrustStore**

1. Download [redshift\-keytool\.jar](https://s3.amazonaws.com/redshift-downloads/redshift-keytool.jar)\.

1. Do one of the following:
   + To import the Amazon Redshift Certificate Authority bundle into a Java TrustStore, run the following command\. 

     ```
     java -jar redshift-keytool.jar -s
     ```
   + To import the Amazon Redshift Certificate Authority bundle into your private TrustStore, run the following command: 

     ```
     java -jar redshift-keytool.jar -k <your_private_trust_store> -p <keystore_password> 
     ```

## Using other SSL connection types<a name="connecting-transitioning-to-acm-other-ssl-types"></a>

Follow the steps in this section if you connect using any of the following:
+  Open source ODBC driver 
+  Open source JDBC driver 
+  The psql command line interface 
+  Any language bindings based on libpq, such as psycopg2 \(Python\) and ruby\-pg \(Ruby\) 

**To use ACM certificates with other SSL connection types:**

1.  Download the [Amazon Redshift certificate authority bundle](https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt)\. 

1. Place the certificates from the bundle in your `root.crt` file\. 
   + On Linux and macOS X operating systems, the file is `~/.postgresql/root.crt`\.
   + On Microsoft Windows, the file is `%APPDATA%\postgresql\root.crt`\.