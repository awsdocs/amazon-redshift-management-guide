# Configuring a JDBC driver version 2\.0 connection<a name="jdbc20-install"></a>

You can use a JDBC driver version 2\.0 connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools\. To do this, you download a JDBC driver\. 

To use a JDBC connection, refer to the following sections\.

**Topics**
+ [Download the Amazon Redshift JDBC driver version 2\.0 driver](jdbc20-download-driver.md)
+ [Install the Amazon Redshift JDBC driver version 2\.0](jdbc20-install-driver.md)
+ [Referencing the JDBC driver libraries](jdbc20-driver-libraries.md)
+ [Registering the driver class](jdbc20-register-driver-class.md)
+ [Getting the JDBC URL](jdbc20-obtain-url.md)
+ [Building the connection URL](jdbc20-build-connection-url.md)
+ [Configure TCP keepalives for your JDBC connection](#configure-tcp-keepalives-jdbc20)
+ [Configure your JDBC connection with Apache Maven](configure-jdbc20-connection-with-maven.md)
+ [Configure authentication and SSL](jdbc20-configure-authentication-ssl.md)
+ [Configure logging](jdbc20-configuring-logging.md)
+ [Convert data types](jdbc20-data-type-mapping.md)
+ [Use prepared statement support](jdbc20-prepared-statement-support.md)
+ [Differences between the 2\.x and 1\.x versions of the JDBC driver](jdbc20-jdbc10-driver-differences.md)
+ [JDBC driver version 2\.0 configuration options](jdbc20-configuration-options.md)
+ [Previous versions of JDBC driver version 2\.0](jdbc20-previous-driver-version-20.md)

## Configure TCP keepalives for your JDBC connection<a name="configure-tcp-keepalives-jdbc20"></a>

By default, the Amazon Redshift JDBC driver is configured to use TCP keepalives to prevent connections from timing out\. You can specify when the driver starts sending keepalive packets or disable the feature by setting the relevant properties in the connection URL\. For more information about the syntax of the connection URL, see [Building the connection URL](jdbc20-build-connection-url.md)\.


| Property | Description | 
| --- | --- | 
|  `TCPKeepAliveMinutes`  |  To specify when the driver sends keepalive packets, set the number of minutes of inactivity before the driver starts sending TCP keepalive packets\.  | 
|  `TCPKeepAlive`  |  To disable TCP keepalives, set this property to `FALSE`\.  | 