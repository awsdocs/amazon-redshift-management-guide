# Configure a JDBC Connection with Apache Maven<a name="configure-jdbc-connection-with-maven"></a>

[Apache Maven](https://maven.apache.org/) is a software project management and comprehension tool\. The AWS SDK for Java supports Apache Maven projects\. For more information, see [Using the SDK with Apache Maven](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-project-maven.html)\.

If you use Apache Maven, you can configure and build your projects to use an Amazon Redshift JDBC driver to connect to your Amazon Redshift cluster\. To do this, you need to add the JDBC driver as a dependency in your project’s `pom.xml` file\. Follow the steps in this section if you use Maven to build your project and want to use a JDBC connection\. 

## Configuring the JDBC Driver as a Maven Dependency<a name="configure-jdbc-connection-with-maven-dependency"></a>

**To configure the JDBC Driver as a Maven dependency**

1. Add the following repository to the repositories section of your `pom.xml` file\.
**Note**  
The URL in the following code will return an error if used in a browser\. The URL is intended to be used only within the context of a Maven project\.

   ```
   <repositories>
       <repository>
         <id>redshift</id>
         <url>http://redshift-maven-repository.s3-website-us-east-1.amazonaws.com/release</url>
       </repository>
   </repositories>
   ```

1. Declare the version of the driver you want to use in the dependencies section of your `pom.xml` file\.

   Amazon Redshift offers drivers for tools that are compatible with either the JDBC 4\.2 API, JDBC 4\.1 API, or JDBC 4\.0 API\. For information about the functionality supported by these drivers, see the [Amazon Redshift JDBC Driver Release Notes](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Release+Notes.pdf)\. 

   Add a dependency for the driver from the following list\.
**Note**  
For version 1\.2\.1\.1001 and later, you can use either the generic driver class name `com.amazon.redshift.jdbc.Driver` or the version\-specific class name listed with the driver in the list following; for example `com.amazon.redshift.jdbc42.Driver`\. For releases prior to 1\.2\.1001, only version specific class names are supported\.

   + JDBC 4\.2–compatible driver: 

     ```
     <dependency>
        <groupId>com.amazon.redshift</groupId>
        <artifactId>redshift-jdbc42</artifactId>
        <version>1.2.10.1009</version>
     </dependency>
     ```

     The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

   +  JDBC 4\.1–compatible driver: 

     ```
     <dependency>
       <groupId>com.amazon.redshift</groupId>
       <artifactId>redshift-jdbc41</artifactId>
       <version>1.2.10.1009</version>
     </dependency>
     ```

     The class name for this driver is `com.amazon.redshift.jdbc41.Driver`\.

   +  JDBC 4\.0–compatible driver: 

     ```
     <dependency>
       <groupId>com.amazon.redshift</groupId>
       <artifactId>redshift-jdbc4</artifactId>
       <version>1.2.10.1009</version>
     </dependency>
     ```

      The class name for this driver is `com.amazon.redshift.jdbc4.Driver`\.

1. Download and review the [Amazon Redshift JDBC Driver License Agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Driver+License+Agreement.pdf)\. 

The standard Amazon Redshift JDBC drivers include the AWS SDK that is required to use IAM database authentication\. We recommend using the standard drivers unless the size of the driver files is an issue for your application\. If you need smaller driver files and you do not use IAM database authentication, or if you already have AWS SDK for Java 1\.11\. 118 or later in your Java class path, then add a dependency for the driver from the following list\.

+ JDBC 4\.2–compatible driver: 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc42-no-awssdk</artifactId>
     <version>1.2.10.1009</version>
  </dependency>
  ```

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

+  JDBC 4\.1–compatible driver: 

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc41-no-awssdk</artifactId>
    <version>1.2.10.1009</version>
  </dependency>
  ```

  The class name for this driver is `com.amazon.redshift.jdbc41.Driver`\.

+  JDBC 4\.0–compatible driver: 

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc4-no-awssdk</artifactId>
    <version>1.2.10.1009</version>
  </dependency>
  ```

   The class name for this driver is `com.amazon.redshift.jdbc4.Driver`\.

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

If your tool requires a specific previous version of a driver, see [Previous JDBC Driver Versions Using Maven](jdbc-previous-versions.md#jdbc-previous-versions-maven)\.

If you need to distribute these drivers to your customers or other third parties, please send email to *redshift\-pm@amazon\.com* to arrange an appropriate license\. 

## Upgrading the Driver to the Latest Version<a name="configure-jdbc-connection-with-maven-upgrading"></a>

To upgrade or change the Amazon Redshift JDBC driver to the latest version, modify the version section of the dependency to the latest version of the driver and then clean your project with the Maven Clean Plugin, as shown following\. 

```
mvn clean
```