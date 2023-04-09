# Configuring security options for connections<a name="connecting-ssl-support"></a>

Amazon Redshift supports Secure Sockets Layer \(SSL\) connections to encrypt data and server certificates to validate the server certificate that the client connects to\. 

## Connect using SSL<a name="connect-using-ssl"></a>

To support SSL connections, Amazon Redshift creates and installs an [AWS Certificate Manager \(ACM\)](https://aws.amazon.com/certificate-manager/) issued SSL certificate on each cluster\. ACM certificates are publicly trusted by most operating systems, web browsers, and clients\. You might need to download a certificate bundle if your SQL clients or applications connect to Amazon Redshift using SSL with the `sslmode` connection option set to `require`, `verify-ca`, or `verify-full`\. If your client needs a certificate, Amazon Redshift provides a bundle certificate as follows:
+ Download the bundle from [https://s3\.amazonaws\.com/redshift\-downloads/amazon\-trust\-ca\-bundle\.crt](https://s3.amazonaws.com/redshift-downloads/amazon-trust-ca-bundle.crt)\. 
  + The expected MD5 checksum number is 418dea9b6d5d5de7a8f1ac42e164cdcf\.
  + The sha256 checksum number is 36dba8e4b8041cd14b9d60158893963301bcbb92e1c456847784de2acb5bd550\.

  Don't use the previous certificate bundle that was located at `https://s3.amazonaws.com/redshift-downloads/redshift-ca-bundle.crt`\.  
+  In the China AWS Region, download the bundle from [https://s3\.cn\-north\-1\.amazonaws\.com\.cn/redshift\-downloads\-cn/amazon\-trust\-ca\-bundle\.crt](https://s3.cn-north-1.amazonaws.com.cn/redshift-downloads-cn/amazon-trust-ca-bundle.crt)\. 
  + The expected MD5 checksum number is 418dea9b6d5d5de7a8f1ac42e164cdcf\.
  + The sha256 checksum number is 36dba8e4b8041cd14b9d60158893963301bcbb92e1c456847784de2acb5bd550\.

  Don't use the previous certificate bundles that were located at `https://s3.cn-north-1.amazonaws.com.cn/redshift-downloads-cn/redshift-ca-bundle.crt`  and `https://s3.cn-north-1.amazonaws.com.cn/redshift-downloads-cn/redshift-ssl-ca-cert.pem`

**Important**  
Amazon Redshift has changed the way that SSL certificates are managed\. You might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Transitioning to ACM certificates for SSL connections](connecting-transitioning-to-acm-certs.md)\.

By default, cluster databases accept a connection whether it uses SSL or not\. To configure your cluster to require an SSL connection, set the `require_SSL` parameter to `true` in the parameter group that is associated with the cluster\. 

Amazon Redshift supports an SSL mode that is compliant with Federal Information Processing Standard \(FIPS\) 140\-2\. FIPS\-compliant SSL mode is disabled by default\. 

**Important**  
Enable FIPS\-compliant SSL mode only if your system is required to be FIPS\-compliant\.

To enable FIPS\-compliant SSL mode, set both the `use_fips_ssl` parameter and the `require_SSL` parameter to `true` in the parameter group that is associated with the cluster\. For information about modifying a parameter group, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\. 

 Amazon Redshift supports the Elliptic Curve Diffie—Hellman Ephemeral \(ECDHE\) key agreement protocol\. With ECDHE, the client and server each have an elliptic curve public\-private key pair that is used to establish a shared secret over an insecure channel\. You don't need to configure anything in Amazon Redshift to enable ECDHE\. If you connect from a SQL client tool that uses ECDHE to encrypt communication between the client and server, Amazon Redshift uses the provided cipher list to make the appropriate connection\. For more information, see [Elliptic curve diffie—hellman](https://en.wikipedia.org/wiki/Elliptic_curve_Diffie%E2%80%93Hellman) on Wikipedia and [Ciphers](https://www.openssl.org/) on the OpenSSL website\. 

## Using SSL and trust CA certificates in ODBC<a name="connecting-ssl-support-odbc"></a>

If you connect using the latest Amazon Redshift ODBC drivers \(version 1\.3\.7\.1000 or later\), you can skip this section\. To download the latest drivers, see [Configuring an ODBC connection](configure-odbc-connection.md)\. 

You might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Connect using SSL](#connect-using-ssl)\.

You can verify that the certificate that you downloaded matches the expected MD5 checksum number\. To do this, you can use the Md5sum program on Linux operating systems, or another tool on Windows and macOS X operating systems\.

 ODBC DSNs contain an `sslmode` setting that determines how to handle encryption for client connections and server certificate verification\. Amazon Redshift supports the following `sslmode` values from the client connection: 
+ `disable`

  SSL is disabled and the connection is not encrypted\.
+ `allow`

  SSL is used if the server requires it\.
+ `prefer`

  SSL is used if the server supports it\. Amazon Redshift supports SSL, so SSL is used when you set `sslmode` to `prefer`\.
+ `require`

  SSL is required\.
+ `verify-ca`

  SSL must be used and the server certificate must be verified\.
+ `verify-full`

  SSL must be used\. The server certificate must be verified and the server hostname must match the hostname attribute on the certificate\. 

You can determine whether SSL is used and server certificates are verified in a connection between the client and the server\. To do this, you need to review the `sslmode` setting for your ODBC DSN on the client and the `require_SSL` setting for the Amazon Redshift cluster on the server\. The following table describes the encryption result for the various client and server setting combinations: 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/connecting-ssl-support.html)

### Connect using the server certificate with ODBC on Microsoft Windows<a name="connecting-ssl-support-odbc-with-cert"></a>

 If you want to connect to your cluster using SSL and the server certificate, first download the certificate to your client computer or Amazon EC2 instance\. Then configure the ODBC DSN\. 

1.  Download the Amazon Redshift certificate authority bundle to your client computer at the `lib` folder in your driver installation directory, and save the file as `root.crt`\. For download information, see [Connect using SSL](#connect-using-ssl)\.

1.  Open **ODBC Data Source Administrator**, and add or edit the system DSN entry for your ODBC connection\. For **SSL Mode**, select `verify-full` unless you use a DNS alias\. If you use a DNS alias, select `verify-ca`\. Then choose **Save**\. 

    For more information about configuring the ODBC DSN, see [Configuring an ODBC connection](configure-odbc-connection.md)\. 

## Using SSL and server certificates in Java<a name="connecting-ssl-support-java"></a>

SSL provides one layer of security by encrypting data that moves between your client and cluster\. Using a server certificate provides an extra layer of security by validating that the cluster is an Amazon Redshift cluster\. It does so by checking the server certificate that is automatically installed on all clusters that you provision\. For more information about using server certificates with JDBC, go to [Configuring the client](https://jdbc.postgresql.org/documentation/ssl/#configuring-the-client) in the PostgreSQL documentation\.

### Connect using trust CA certificates in Java<a name="connecting-ssl-support-java-with-cert"></a>

**Important**  
Amazon Redshift has changed the way that SSL certificates are managed\. You might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Connect using SSL](#connect-using-ssl)\.

**To connect using trust CA certificates**

You can use the `redshift-keytool.jar` file to import CA certificates in the Amazon Redshift Certificate Authority bundle into a Java TrustStore or your private TrustStore\.

1. If you use the Java command line `-Djavax.net.ssl.trustStore` option, remove it from command line, if possible\.

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