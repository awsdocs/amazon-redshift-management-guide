# Configuring a JDBC connection<a name="configure-jdbc-connection"></a>

You can use a JDBC connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools\. To do this, you download a JDBC driver\. 

If you want to use a JDBC connection, take the steps following\.

**Topics**
+ [Download an Amazon Redshift JDBC driver](#download-jdbc-driver)
+ [Obtain the JDBC URL](#obtain-jdbc-url)
+ [Configure authentication and SSL for JDBC connection](#configure-authentication-ssl-jdbc)
+ [Configure TCP keepalives for JDBC connection](#configure-tcp-keepalives-jdbc)
+ [Configure logging for JDBC connection](#configure-logging-jdbc)
+ [Configure JDBC connection with Apache Maven](#configure-jdbc-connection-with-maven)
+ [Configure JDBC driver options](#configure-jdbc-options)
+ [Use previous JDBC driver versions in certain cases](#jdbc-previous-versions)

## Download an Amazon Redshift JDBC driver<a name="download-jdbc-driver"></a>

Amazon Redshift offers drivers for tools that are compatible with the JDBC 4\.2 API\.  For information about the functionality supported by these drivers, see the [Amazon Redshift JDBC driver release notes](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Release+Notes.pdf)\. 

For detailed information about how to install the JDCBC driver, reference the JDBC driver libraries, and register the driver class, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

JDBC drivers version 1\.2\.27\.1051 and later support Amazon Redshift stored procedures\. For more information, see [Creating stored procedures in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/stored-procedure-overview.html) in the *Amazon Redshift Database Developer Guide*\.

JDBC drivers version 1\.2\.8\.1005 and later support database authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials\. For more information, see [Using IAM authentication to generate database user credentials](generating-user-credentials.md)\.

For each computer where you use the Amazon Redshift JDBC driver, make sure that Java Runtime Environment \(JRE\) 8\.0 is installed\.  

If you use the Amazon Redshift JDBC driver for database authentication, make sure that you have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, you can use a driver that includes the AWS SDK\.
+ [JDBC4\.2–compatible driver \(without the AWS SDK\) and driver dependent libraries for AWS SDK files version 1\.2\.47](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/RedshiftJDBC42-1.2.47.1071.zip)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

  This ZIP file contains the JDBC4\.2–compatible driver \(without the AWS SDK\) and its dependent library files\. Unzip the dependent jar files to the same location as the JDBC driver\. Only the JDBC driver needs to be in the CLASSPATH because the driver manifest file contains all dependent library file names which are located in the same directory as the JDBC driver\. For more information about how to install the JDBC driver, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

  Use this Amazon Redshift JDBC driver with the AWS SDK that is required for IAM database authentication\.
+ [JDBC 4\.2–compatible driver \(without the AWS SDK\) version 1\.2\.47]( https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/RedshiftJDBC42-no-awssdk-1.2.47.1071.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

For more information about previous driver versions, see [Use previous JDBC driver versions with the AWS SDK for Java](#jdbc-previous-versions-with-sdk)\.

Then download and review the [Amazon Redshift ODBC and JDBC driver license agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+and+JDBC+Driver+License+Agreement.pdf)\. 

If your tool requires a specific previous version of a driver, see [Use previous JDBC driver versions in certain cases](#jdbc-previous-versions)\.

## Obtain the JDBC URL<a name="obtain-jdbc-url"></a>

Before you can connect to your Amazon Redshift cluster from a SQL client tool, you need to know the JDBC URL of your cluster\. The JDBC URL has the following format: `jdbc:redshift://endpoint:port/database`\.

**Note**  
A JDBC URL specified with the former format of `jdbc:postgresql://endpoint:port/database` still works\.

The fields of the format shown preceding have the following values\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html)

The following is an example JDBC URL: `jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev` 

For information about how to get your JDBC connection, see [Finding your cluster connection string](configuring-connections.md#connecting-connection-string)\. 

 If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting connection issues in Amazon Redshift](troubleshooting-connections.md)\. 

## Configure authentication and SSL for JDBC connection<a name="configure-authentication-ssl-jdbc"></a>

Configure the Amazon Redshift JDBC driver to authenticate your connection according to the security requirements of the Amazon Redshift server that you are connecting to\.

To authenticate the connection, always provide your Amazon Redshift user name and password\. The password is transmitted using a salted MD5 hash of the password\. Depending on whether SSL is enabled and required on the server, you might also need to configure the driver to connect through SSL\. You might need to use one\-way SSL authentication so that the client \(the driver itself\) verifies the identity of the server\. 

For information about configuring the JDBC driver to authenticate the connection, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

## Configure TCP keepalives for JDBC connection<a name="configure-tcp-keepalives-jdbc"></a>

By default, the Amazon Redshift JDBC driver is configured to use TCP keepalives to prevent connections from timing out\. You can specify when the driver starts sending keepalive packets or disable the feature by setting the relevant properties in the connection URL\.

For information about configuring TCP keepalives for the JDBC driver, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

## Configure logging for JDBC connection<a name="configure-logging-jdbc"></a>

To help troubleshoot issues, you can enable logging in the JDBC driver\.

For information about configuring logging for JDBC connection, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

## Configure JDBC connection with Apache Maven<a name="configure-jdbc-connection-with-maven"></a>

 Apache Maven is a software project management and comprehension tool\. The AWS SDK for Java supports Apache Maven projects\. For more information, see [Using the SDK with Apache Maven](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-project-maven.html) in the *AWS SDK for Java Developer Guide\.* 

If you use Apache Maven, you can configure and build your projects to use an Amazon Redshift JDBC driver to connect to your Amazon Redshift cluster\. To do this, add the JDBC driver as a dependency in your project's `pom.xml` file\. If you use Maven to build your project and want to use a JDBC connection, take the steps in the following section\. 

### Configuring the JDBC driver as a Maven dependency<a name="configure-jdbc-connection-with-maven-dependency"></a>

**To configure the JDBC driver as a Maven dependency**

1. Add the following repository to the repositories section of your `pom.xml` file\.
**Note**  
The URL in the following code example returns an error if used in a browser\. Use this URL only in the context of a Maven project\.

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

1. Declare the version of the driver that you want to use in the dependencies section of your `pom.xml` file\.

   Amazon Redshift offers drivers for tools that are compatible with the JDBC 4\.2 API\.  For information about the functionality supported by these drivers, see [Download an Amazon Redshift JDBC driver](#download-jdbc-driver)\.  

   Add a dependency for the driver from the following list\.
**Note**  
For version 1\.2\.1\.1001 and later, you can use either the generic driver class name `com.amazon.redshift.jdbc.Driver` or the version\-specific class name listed with the driver in the list following, for example `com.amazon.redshift.jdbc42.Driver`\. For releases before 1\.2\.1\.1001, only version\-specific class names are supported\.

   Replace the *driver\-version* in the following example with your driver version\. For example, `1.2.45.1069`\. 
   + JDBC 4\.2–compatible driver: 

     ```
     <dependency>
        <groupId>com.amazon.redshift</groupId>
        <artifactId>redshift-jdbc42</artifactId>
        <version>driver-version</version>
     </dependency>
     ```

     The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

1. Download and review the [Amazon Redshift ODBC and JDBC driver license agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+and+JDBC+Driver+License+Agreement.pdf)\. 

The standard Amazon Redshift JDBC drivers include the AWS SDK that is required to use IAM database authentication\. We recommend using the standard drivers unless the size of the driver files is an issue for your application\. If you need smaller driver files and you do not use IAM database authentication, or if you already have AWS SDK for Java 1\.11\. 118 or later in your Java class path, then add a dependency for the driver from the following list\.

Replace the *driver\-version* in the following example with your driver version\. For example, `1.2.45.1069`\. 
+ JDBC 4\.2–compatible driver: 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc42-no-awssdk</artifactId>
     <version>driver-version</version>
  </dependency>
  ```

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

The Amazon Redshift Maven drivers with no SDKs include the following optional dependencies that you can include in your project as needed\. 

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

If your tool requires a specific previous version of a driver, see [Use previous JDBC driver versions with Maven](#jdbc-previous-versions-maven)\.

### Upgrading the driver to the latest version<a name="configure-jdbc-connection-with-maven-upgrading"></a>

To upgrade or change the Amazon Redshift JDBC driver to the latest version, first modify the version section of the dependency to the latest version of the driver\. Then clean your project with the Maven Clean Plugin, as shown following\. 

```
mvn clean
```

## Configure JDBC driver options<a name="configure-jdbc-options"></a>

To control the behavior of the Amazon Redshift JDBC driver, you can append configuration options to the JDBC URL\. For example, the following JDBC URL connects to your cluster using Secure Socket Layer \(SSL\), user \(UID\), and password \(PWD\)\. 

```
jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev?ssl=true&UID=your_username&PWD=your_password 
```

For more information about SSL options, see [Connect using SSL](connecting-ssl-support.md#connect-using-ssl)\.

For information about how to set up JDBC driver configuration options, see [Amazon Redshift JDBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/Amazon+Redshift+JDBC+Driver+Install+Guide.pdf)\. 

## Use previous JDBC driver versions in certain cases<a name="jdbc-previous-versions"></a>

Download a previous version of the Amazon Redshift JDBC driver only if your tool requires a specific version of the driver\. For information about the functionality supported in these versions of the drivers, see [Download an Amazon Redshift JDBC driver](#download-jdbc-driver)\. 

For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, use Amazon Redshift JDBC driver version 1\.2\.8\.1005 or later\.

**Important**  
Amazon Redshift has changed the way that SSL certificates are managed\. If you must use a driver version earlier than 1\.2\.8\.1005, you might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Transitioning to ACM certificates for SSL connections](connecting-transitioning-to-acm-certs.md)\.

If you use the Amazon Redshift JDBC driver for database authentication, make sure that you have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, you can use a driver that includes the AWS SDK\. For more information, see [Use previous JDBC driver versions with the AWS SDK for Java](#jdbc-previous-versions-with-sdk)\.

 These are JDBC 4\.2–compatible drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.47\.1071/RedshiftJDBC42\-no\-awssdk\-1\.2\.47\.1071\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.47.1071/RedshiftJDBC42-no-awssdk-1.2.47.1071.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.45\.1069/RedshiftJDBC42\-no\-awssdk\-1\.2\.45\.1069\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.45.1069/RedshiftJDBC42-no-awssdk-1.2.45.1069.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.43\.1067/RedshiftJDBC42\-no\-awssdk\-1\.2\.43\.1067\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.43.1067/RedshiftJDBC42-no-awssdk-1.2.43.1067.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.41\.1065/RedshiftJDBC42\-no\-awssdk\-1\.2\.41\.1065\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.41.1065/RedshiftJDBC42-no-awssdk-1.2.41.1065.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC42\-no\-awssdk\-1\.2\.37\.1061\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC42-no-awssdk-1.2.37.1061.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC42\-no\-awssdk\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC42-no-awssdk-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC42\-no\-awssdk\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC42-no-awssdk-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC42\-no\-awssdk\-1\.2\.32\.1056\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC42-no-awssdk-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC42\-no\-awssdk\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC42-no-awssdk-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC42\-no\-awssdk\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC42-no-awssdk-1.2.20.1043.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC42\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC42-no-awssdk-1.2.16.1027.jar)\.  
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC42\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC42-no-awssdk-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC42\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC42-no-awssdk-1.2.12.1017.jar)\.

 These are previous JDBC 4\.1–compatible drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.43\.1065/RedshiftJDBC41\-no\-awssdk\-1\.2\.43\.1067\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.43.1067/RedshiftJDBC41-no-awssdk-1.2.43.1067.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.41\.1065/RedshiftJDBC41\-no\-awssdk\-1\.2\.41\.1065\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.41.1065/RedshiftJDBC41-no-awssdk-1.2.41.1065.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC41\-no\-awssdk\-1\.2\.37\.1061\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC41-no-awssdk-1.2.37.1061.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC41\-no\-awssdk\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC41-no-awssdk-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC41\-no\-awssdk\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC41-no-awssdk-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC41\-no\-awssdk\-1\.2\.32\.1056\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC41-no-awssdk-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC41\-no\-awssdk\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC41-no-awssdk-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC41\-no\-awssdk\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC41-no-awssdk-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC41\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC41-no-awssdk-1.2.16.1027.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC41\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC41-no-awssdk-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC41\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC41-no-awssdk-1.2.12.1017.jar)\.

These are previous JDBC 4\.0–compatible drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.43\.1067/RedshiftJDBC4\-no\-awssdk\-1\.2\.43\.1067\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.43.1067/RedshiftJDBC4-no-awssdk-1.2.43.1067.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.41\.1065/RedshiftJDBC4\-no\-awssdk\-1\.2\.41\.1065\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.41.1065/RedshiftJDBC4-no-awssdk-1.2.41.1065.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC4\-no\-awssdk\-1\.2\.37\.1061\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC4-no-awssdk-1.2.37.1061.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC4\-no\-awssdk\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC4-no-awssdk-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC4\-no\-awssdk\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC4-no-awssdk-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC4\-no\-awssdk\-1\.2\.32\.1056\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC4-no-awssdk-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC4\-no\-awssdk\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC4-no-awssdk-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.20\.1043/RedshiftJDBC4\-no\-awssdk\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC4-no-awssdk-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.16\.1027/RedshiftJDBC4\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC4-no-awssdk-1.2.16.1027.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.15\.1025/RedshiftJDBC4\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC4-no-awssdk-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.12\.1017/RedshiftJDBC4\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC4-no-awssdk-1.2.12.1017.jar)\.

### Use previous JDBC driver versions with the AWS SDK for Java<a name="jdbc-previous-versions-with-sdk"></a>

If you use the JDBC driver for database authentication, make sure that you have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, you can use one of the following drivers that include the AWS SDK\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.43\.1067/RedshiftJDBC42\-1\.2\.43\.1067\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.43.1067/RedshiftJDBC42-1.2.43.1067.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.41\.1065/RedshiftJDBC42\-1\.2\.41\.1065\.jar ](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.41.1065/RedshiftJDBC42-1.2.41.1065.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC42\-1\.2\.37\.1061\.jar ](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC42-1.2.37.1061.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC41\-1\.2\.37\.1061\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC41-1.2.37.1061.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.37\.1061/RedshiftJDBC4\-1\.2\.37\.1061\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.37.1061/RedshiftJDBC4-1.2.37.1061.jar )\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC42\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC42-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC41\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC41-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.36\.1060/RedshiftJDBC4\-1\.2\.36\.1060\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.36.1060/RedshiftJDBC4-1.2.36.1060.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC42\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC42-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC41\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC41-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.34\.1058/RedshiftJDBC4\-1\.2\.34\.1058\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.34.1058/RedshiftJDBC4-1.2.34.1058.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC42\-1\.2\.32\.1056\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC42-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC41\-1\.2\.32\.1056\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC41-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.32\.1056/RedshiftJDBC4\-1\.2\.32\.1056\.jar ](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.32.1056/RedshiftJDBC4-1.2.32.1056.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC42\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC42-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC41\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC41-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC4\-1\.2\.27\.1051\.jar ](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC4-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC42\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC42-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC41\-1\.2\.27\.1051\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC41-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.27\.1051/RedshiftJDBC4\-1\.2\.27\.1051\.jar ](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.27.1051/RedshiftJDBC4-1.2.27.1051.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC42\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC42-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC41\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC41-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC4\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC4-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC42\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC42-1.2.16.1027.jar)\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC42\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC42-1.2.15.1025.jar)\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC42\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC42-1.2.12.1017.jar)\.

### Use previous JDBC driver versions with Maven<a name="jdbc-previous-versions-maven"></a>

Add a previous version of the Amazon Redshift JDBC driver to your project only if your tool requires a specific version of the driver\. For information about the functionality supported in these driver versions, see [Download an Amazon Redshift JDBC driver](#download-jdbc-driver)\. For information about configuring with Maven, see [Configure JDBC connection with Apache Maven](#configure-jdbc-connection-with-maven)\.  