# Previous JDBC Driver Versions<a name="jdbc-previous-versions"></a>

Download a previous version of the Amazon Redshift JDBC driver only if your tool requires a specific version of the driver\. For information about the functionality supported in previous versions of the drivers, download [Amazon Redshift JDBC Driver Release Notes](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Release+Notes.pdf)\. 

For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, use Amazon Redshift JDBC driver version 1\.2\.8\.1005 or later\.

**Important**  
Amazon Redshift has changed the way that we manage SSL certificates\. If you must use a driver version earlier than 1\.2\.8\.1005, you might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Transitioning to ACM Certificates for SSL Connections](connecting-transitioning-to-acm-certs.md)\.

If you use the Amazon Redshift JDBC driver for database authentication, you must have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, you can use a driver that includes the AWS SDK\. For more information, see [Previous JDBC Driver Versions With the AWS SDK for Java](#jdbc-previous-versions-with-sdk)

**Important**  
We strongly recommend using an Amazon Redshift JDBC driver that doesn't include the AWS SDK if possible\. The drivers in the following list don't include the AWS SDK\.

 

 These are previous JDBC 4\.2–compatible drivers: 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC42\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/1.2.16.1027/RedshiftJDBC42-no-awssdk-1.2.16.1027.jar) 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC42\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC42-no-awssdk-1.2.15.1025.jar) 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC42\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC42-no-awssdk-1.2.12.1017.jar)\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-no-awssdk-1.2.10.1009.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-no\-awssdk\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-no-awssdk-1.2.8.1005.jar)
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-no\-awssdk\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-no-awssdk-1.2.7.1003.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.2\.1\.1001\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.2.1.1001.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.1\.17\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.1.17.1017.jar)\. 

 These are previous JDBC 4\.1–compatible drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC41\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC41-no-awssdk-1.2.16.1027.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC41\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC41-no-awssdk-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC41\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC41-no-awssdk-1.2.12.1017.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-no-awssdk-1.2.10.1009.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-no\-awssdk\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-no-awssdk-1.2.8.1005.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-no\-awssdk\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-no-awssdk-1.2.7.1003.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.10\.1010\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.10.1010.jar) 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.9\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.9.1009.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.7\.1007\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.7.1007.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.6\.1006\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.6.1006.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.2\.0002\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.2.0002.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.1\.0001\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.1.0001.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.0\.0000\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.0.0000.jar)

These are previous JDBC 4\.0–compatible drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.16\.1027/RedshiftJDBC4\-no\-awssdk\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC4-no-awssdk-1.2.16.1027.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.15\.1025/RedshiftJDBC4\-no\-awssdk\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC4-no-awssdk-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jbdc/1\.2\.12\.1017/RedshiftJDBC4\-no\-awssdk\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC4-no-awssdk-1.2.12.1017.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-no-awssdk-1.2.10.1009.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-no\-awssdk\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-no-awssdk-1.2.8.1005.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-no\-awssdk\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-no-awssdk-1.2.7.1003.jar)
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.2\.1\.1001\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.2.1.1001.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.17\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.17.1017.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.10\.1010\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.10.1010.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.9\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.9.1009.jar) 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.7\.1007\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.7.1007.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.6\.1006\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.6.1006.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.2\.0002\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.2.0002.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.1\.0001\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.1.0001.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.1\.0\.0000\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.1.0.0000.jar)

## Previous JDBC Driver Versions With the AWS SDK for Java<a name="jdbc-previous-versions-with-sdk"></a>

If you use the JDBC driver for database authentication, you must have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, you can use one of the following drivers that include the AWS SDK\. The latest supported Amazon Redshift JDBC driver that includes the AWS SDK is 1\.2\.20\.1043\. The earliest Amazon Redshift JDBC driver that includes the AWS SDK is 1\.2\.8\.1005\.

**Important**  
We strongly recommend using an Amazon Redshift JDBC driver that doesn't include the AWS SDK if possible\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC42\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC42-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC41\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC41-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.20\.1043/RedshiftJDBC4\-1\.2\.20\.1043\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.20.1043/RedshiftJDBC4-1.2.20.1043.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC42\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC42-1.2.16.1027.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC42\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC42-1.2.15.1025.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC42\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC42-1.2.12.1017.jar)
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.2.10.1009.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.2.8.1005.jar)\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.2.7.1003.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.2\.1\.1001\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.2.1.1001.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.1\.17\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.1.17.1017.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC41\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC41-1.2.16.1027.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017RedshiftJDBC41\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC41-1.2.15.1025.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC41\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC41-1.2.12.1017.jar)
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.2.10.1009.jar) 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.2.7.1003.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.2.8.1005.jar)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.16\.1027/RedshiftJDBC4\-1\.2\.16\.1027\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.16.1027/RedshiftJDBC4-1.2.16.1027.jar) 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.15\.1025/RedshiftJDBC4\-1\.2\.15\.1025\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.15.1025/RedshiftJDBC4-1.2.15.1025.jar) 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/jdbc/1\.2\.12\.1017/RedshiftJDBC4\-1\.2\.12\.1017\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/1.2.12.1017/RedshiftJDBC4-1.2.12.1017.jar)\.
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.2.10.1009.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.2\.8\.1005\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.2.8.1005.jar)\. 
+  [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.2\.7\.1003\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.2.7.1003.jar)\. 

## Previous JDBC Driver Versions Using Maven<a name="jdbc-previous-versions-maven"></a>

Add a previous version of the Amazon Redshift JDBC driver to your project only if your tool requires a specific version of the driver\. For information about the functionality supported in previous versions of the drivers, download [Amazon Redshift JDBC Driver Release Notes](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Release+Notes.pdf)\. 

 

 These are previous JDBC 4\.2–compatible drivers: 
+ JDBC42\-1\.2\.8\.1005

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc42</artifactId>
    <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC42\-1\.2\.8\.1005 no SDK

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc42-no-awssdk</artifactId>
    <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC42\-1\.2\.7\.1003

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc42</artifactId>
    <version>1.2.7.1003</version>
  </dependency>
  ```
+ JDBC42\-1\.2\.1\.1001

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc42</artifactId>
    <version>1.2.1.1001</version>
  </dependency>
  ```
+ JDBC42\-1\.1\.17\.1017

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc42</artifactId>
    <version>1.1.17.1017</version>
  </dependency>
  ```

These are previous JDBC 4\.1–compatible drivers: 
+ JDBC41\-1\.2\.8\.1005 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC41\-1\.2\.8\.1005 no SDK

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc41-no-awssdk</artifactId>
    <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC41\-1\.2\.7\.1003 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.2.7.1003</version>
  </dependency>
  ```
+ JDBC41\-1\.2\.1\.1001 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.2.1.1001</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.17\.1017 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.17.1017</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.10\.1010 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.10.1010</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.9\.1009 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.9.1009</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.7\.1007 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.7.1007</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.6\.1006 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.6.1006</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.2\.0002 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.2.0002</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.1\.0001 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.1.0001</version>
  </dependency>
  ```
+ JDBC41\-1\.1\.0\.0000 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc41</artifactId>
     <version>1.1.0.0000</version>
  </dependency>
  ```

These are previous JDBC 4\.0–compatible drivers: 
+ JDBC4\-1\.2\.8\.1005 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC4\-1\.2\.8\.1005 no SDK

  ```
  <dependency>
    <groupId>com.amazon.redshift</groupId>
    <artifactId>redshift-jdbc4-no-awssdk</artifactId>
    <version>1.2.8.1005</version>
  </dependency>
  ```
+ JDBC4\-1\.2\.7\.1003 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.2.7.1003</version>
  </dependency>
  ```
+ JDBC4\-1\.2\.1\.1001 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.2.1.1001</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.17\.1017 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.17.1017</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.10\.1010 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.10.1010</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.9\.1009 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.9.1009</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.7\.1007 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.7.1007</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.6\.1006 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.6.1006</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.2\.0002 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.2.0002</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.1\.0001 

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.1.0001</version>
  </dependency>
  ```
+ JDBC4\-1\.1\.0\.0000

  ```
  <dependency>
     <groupId>com.amazon.redshift</groupId>
     <artifactId>redshift-jdbc4</artifactId>
     <version>1.1.0.0000</version>
  </dependency>
  ```