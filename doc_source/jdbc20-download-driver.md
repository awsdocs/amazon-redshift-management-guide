# Download the Amazon Redshift JDBC driver version 2\.0 driver<a name="jdbc20-download-driver"></a>

Amazon Redshift offers drivers for tools that are compatible with the JDBC 4\.2 API\. 

For detailed information about how to install the JDBC driver, reference the JDBC driver libraries, and register the driver class, see the following topics\. 

JDBC drivers version 1\.2\.27\.1051 and later support Amazon Redshift stored procedures\. For more information, see [Creating stored procedures in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/stored-procedure-overview.html) in the *Amazon Redshift Database Developer Guide*\. 

For each computer where you use the Amazon Redshift JDBC driver version 2\.0, make sure that Java Runtime Environment \(JRE\) 8\.0 is installed\. 

If you use the Amazon Redshift JDBC driver for database authentication, make sure that you have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, download the ZIP file with JDBC 4\.2–compatible driver \(without the AWS SDK\) and driver dependent libraries for the AWS SDK:
+ [JDBC 4\.2–compatible driver \(without the AWS SDK\) and driver dependent libraries for AWS SDK files version 2\.0](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/2.0.0.4/redshift-jdbc42-2.0.0.4.zip)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

  This ZIP file contains the JDBC 4\.2–compatible driver \(without the AWS SDK\) and its dependent library files\. Unzip the dependent jar files to the same location as the JDBC driver\. Only the JDBC driver needs to be in the CLASSPATH because the driver manifest file contains all dependent library file names that are located in the same directory as the JDBC driver\. 

  Use this Amazon Redshift JDBC driver with the AWS SDK that is required for AWS Identity and Access Management \(IAM\) database authentication\.
+ [JDBC 4\.2–compatible driver \(without the AWS SDK\) version 2\.0](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/2.0.0.4/redshift-jdbc42-2.0.0.4.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

Review the JDBC driver version 2\.0 software license and change log file: 
+ [JDBC driver version 2\.0 license](https://github.com/aws/amazon-redshift-jdbc-driver/blob/master/LICENSE) 
+ [JDBC driver version 2\.0 change log](https://github.com/aws/amazon-redshift-jdbc-driver/blob/master/CHANGELOG.md) 

If your tool requires a specific previous version of a JDBC driver version 2\.0, see [Previous versions of JDBC driver version 2\.0 ](jdbc20-previous-driver-version-20.md)\.