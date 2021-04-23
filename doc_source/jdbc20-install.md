# Configuring a JDBC driver version 2\.0 connection<a name="jdbc20-install"></a>

You can use a JDBC driver version 2\.0 connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools\. To do this, you download a JDBC driver\. 

To use a JDBC connection, perform the following steps and use the following options\.

**Topics**
+ [Download the Amazon Redshift JDBC driver version 2\.0 driver](jdbc20-download-driver.md)
+ [Install the Amazon Redshift JDBC driver version 2\.0](jdbc20-install-driver.md)
+ [Configure authentication and SSL for your JDBC connection](#configure-authentication-ssl-jdbc20)
+ [Configure TCP keepalives for your JDBC connection](#configure-tcp-keepalives-jdbc20)
+ [Configure your JDBC connection with Apache Maven](#configure-jdbc20-connection-with-maven)
+ [Configure authentication and SSL](jdbc20-configure-authentication-ssl.md)
+ [Configure logging](jdbc20-configuring-logging.md)
+ [Convert data types](jdbc20-data-type-mapping.md)
+ [Use prepared statement support](jdbc20-prepared-statement-support.md)
+ [Differences between the 2\.x and 1\.x versions of the JDBC driver](jdbc20-jdbc10-driver-differences.md)
+ [JDBC driver version 2\.0 configuration options](jdbc20-configuration-options.md)
+ [Previous versions of JDBC driver version 2\.0](jdbc20-previous-driver-version-20.md)

## Configure authentication and SSL for your JDBC connection<a name="configure-authentication-ssl-jdbc20"></a>

Configure the Amazon Redshift JDBC driver to authenticate your connection according to the security requirements of the Amazon Redshift server that you are connecting to\.

To authenticate the connection, always provide your Amazon Redshift user name and password\. The password is transmitted using a salted MD5 hash of the password\. Depending on whether Secure Sockets Layer \(SSL\) is enabled and required on the server, you might also need to configure the driver to connect through SSL\. You might need to use one\-way SSL authentication so that the client \(the driver itself\) verifies the identity of the server\. 

## Configure TCP keepalives for your JDBC connection<a name="configure-tcp-keepalives-jdbc20"></a>

By default, the Amazon Redshift JDBC driver is configured to use TCP keepalives to prevent connections from timing out\. You can specify when the driver starts sending keepalive packets or disable the feature by setting the relevant properties in the connection URL\. For more information about the syntax of the connection URL, see [Building the connection URL](jdbc20-obtain-url.md#jdbc20-build-connection-url)\.


| Property | Description | 
| --- | --- | 
|  `TCPKeepAliveMinutes`  |  To specify when the driver sends keepalive packets, set the number of minutes of inactivity before the driver starts sending TCP keepalive packets\.  | 
|  `TCPKeepAlive`  |  To disable TCP keepalives, set this property to `FALSE`\.  | 

## Configure your JDBC connection with Apache Maven<a name="configure-jdbc20-connection-with-maven"></a>

Apache Maven is a software project management and comprehension tool\. The AWS SDK for Java supports Apache Maven projects\. For more information, see [Using the SDK with Apache Maven](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-project-maven.html) in the *AWS SDK for Java Developer Guide\.* 

If you use Apache Maven, you can configure and build your projects to use an Amazon Redshift JDBC driver to connect to your Amazon Redshift cluster\. To do this, add the JDBC driver as a dependency in your project's `pom.xml` file\. If you use Maven to build your project and want to use a JDBC connection, take the steps in the following section\. 

### Configuring the JDBC driver as a Maven dependency<a name="configure-jdbc20-connection-with-maven-dependency"></a>

**To configure the JDBC driver as a Maven dependency**

1. Add either the Amazon repository or the Maven Central repository to the repositories section of your `pom.xml` file\.
**Note**  
The URL in the following code example returns an error if used in a browser\. Use this URL only in the context of a Maven project\.

   A\. Amazon Maven Repository\.

   ```
   <repositories>
       <repository>
         <id>redshift</id>
         <url>http://redshift-maven-repository.s3-website-us-east-1.amazonaws.com/release</url>
       </repository>
   </repositories>
   ```

   To connect using SSL, add the following repository to your `pom.xml` file\.

   ```
   <repositories>
       <repository>
         <id>redshift</id>
         <url>https://s3.amazonaws.com/redshift-maven-repository/release</url>
       </repository>
   </repositories>
   ```

   B\. Maven Central Repository\. Add the following to your `pom.xml` file\.

   ```
   <repositories>
       <repository>
         <id>redshift</id>
         <url>https://repo1.maven.org/maven2</url>
       </repository>
   </repositories>
   ```

1. Declare the version of the driver that you want to use in the dependencies section of your `pom.xml` file\.

   Amazon Redshift offers drivers for tools that are compatible with the JDBC 4\.2 API\.  For information about the functionality supported by these drivers, see [Download an Amazon Redshift JDBC driver](configure-jdbc-connection.md#download-jdbc-driver)\. 

   Add a dependency for the driver from the following list\.

   Replace `driver-version` in the following example with your driver version\. For example, `2.0.0.0`\. 
   + JDBC 4\.2–compatible driver: 

     ```
     <dependency>
        <groupId>com.amazon.redshift</groupId>
        <artifactId>redshift-jdbc42</artifactId>
        <version>driver-version</version>
     </dependency>
     ```

     The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

The Amazon Redshift Maven drivers need the following optional dependencies when you use IAM database authentication\. 

```
<dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-core</artifactId>
      <version>1.11.118</version>
      <scope>runtime</scope>
      <optional>true</optional>
</dependency>
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-redshift</artifactId>
      <version>1.11.118</version>
      <scope>runtime</scope>
      <optional>true</optional>
</dependency>
<dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-sts</artifactId>
      <version>1.11.118</version>
      <scope>runtime</scope>
      <optional>true</optional>
</dependency>
```

If your tool requires a specific previous version of a driver, see [Use previous JDBC driver versions with Maven](configure-jdbc-connection.md#jdbc-previous-versions-maven)\.

### Upgrading the driver to the latest version<a name="configure-jdbc20-connection-with-maven-upgrading"></a>

To upgrade or change the Amazon Redshift JDBC driver to the latest version, first modify the version section of the dependency to the latest version of the driver\. Then clean your project with the Maven Clean Plugin, as shown following\. 

```
mvn clean
```