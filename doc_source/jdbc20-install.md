# Configuring a connection for JDBC driver version 2\.1 for Amazon Redshift<a name="jdbc20-install"></a>

You can use a JDBC driver version 2\.1 connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools\. The Amazon Redshift Python connector provides an open source solution\. You can browse the source code, request enhancements, report issues, and provide contributions\. 

To use a JDBC connection, refer to the following sections\.

**Topics**
+ [Download the Amazon Redshift JDBC driver, version 2\.1](jdbc20-download-driver.md)
+ [Installing the Amazon Redshift JDBC driver, version 2\.1](jdbc20-install-driver.md)
+ [Getting the JDBC URL](jdbc20-obtain-url.md)
+ [Building the connection URL](jdbc20-build-connection-url.md)
+ [Configuring TCP keepalives for your JDBC connection](#configure-tcp-keepalives-jdbc20)
+ [Configuring your JDBC connection with Apache Maven](configure-jdbc20-connection-with-maven.md)
+ [Configuring authentication and SSL](jdbc20-configure-authentication-ssl.md)
+ [Configuring logging](jdbc20-configuring-logging.md)
+ [Converting data types](jdbc20-data-type-mapping.md)
+ [Using prepared statement support](jdbc20-prepared-statement-support.md)
+ [Differences between the 2\.1 and 1\.x versions of the JDBC driver](jdbc20-jdbc10-driver-differences.md)
+ [Creating initialization \(\.ini\) files for JDBC driver version 2\.1](jdbc20-ini-file.md)
+ [Options for JDBC driver version 2\.1 configuration](jdbc20-configuration-options.md)
+ [Previous versions of JDBC driver version 2\.1](jdbc20-previous-driver-version-20.md)

## Configuring TCP keepalives for your JDBC connection<a name="configure-tcp-keepalives-jdbc20"></a>

By default, the Amazon Redshift JDBC driver is configured to use TCP keepalives to prevent connections from timing out\. You can specify when the driver starts sending keepalive packets or turn off the feature by setting the relevant properties in the connection URL\. For more information about the syntax of the connection URL, see [Building the connection URL](jdbc20-build-connection-url.md)\.


| Property | Description | 
| --- | --- | 
|  `TCPKeepAliveMinutes`  |  To specify when the driver sends keepalive packets, set the number of minutes of inactivity before the driver starts sending TCP keepalive packets\.  | 
|  `TCPKeepAlive`  |  To turn off TCP keepalives, set this property to `FALSE`\.  | 