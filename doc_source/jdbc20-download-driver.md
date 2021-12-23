# Download the Amazon Redshift JDBC driver, version 2\.1<a name="jdbc20-download-driver"></a>

Amazon Redshift offers drivers for tools that are compatible with the JDBC 4\.2 API\. The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

For detailed information about how to install the JDBC driver, reference the JDBC driver libraries, and register the driver class, see the following topics\. 

For each computer where you use the Amazon Redshift JDBC driver version 2\.1, make sure that the Java Runtime Environment \(JRE\) 8\.0 is installed\. 

If you use the Amazon Redshift JDBC driver for database authentication, make sure that you have AWS SDK for Java 1\.11\.118 or later in your Java class path\. If you don't have AWS SDK for Java installed, download the ZIP file with JDBC 4\.2–compatible driver and driver dependent libraries for the AWS SDK:
+ [JDBC 4\.2–compatible driver version 2\.1 and AWS SDK driver–dependent libraries](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/2.1.0.3/redshift-jdbc42-2.1.0.3.zip)\. 

  This ZIP file contains the JDBC 4\.2–compatible driver version 2\.1 and AWS SDK for Java 1\.x driver–dependent library files\. Unzip the dependent jar files to the same location as the JDBC driver\. Only the JDBC driver needs to be in CLASSPATH\.

  This ZIP file doesn't include the complete AWS SDK for Java 1\.x\. However, it includes the AWS SDK for Java 1\.x driver–dependent libraries that are required for AWS Identity and Access Management \(IAM\) database authentication\.

  Use this Amazon Redshift JDBC driver with the AWS SDK that is required for IAM database authentication\.

  To install the complete AWS SDK for Java 1\.x, see [AWS SDK for Java 1\.x](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/welcome.html) in the *AWS SDK for Java Developer Guide*\. 
+ [JDBC 4\.2–compatible driver version 2\.1 \(without the AWS SDK\)](https://s3.amazonaws.com/redshift-downloads/drivers/jdbc/2.1.0.3/redshift-jdbc42-2.1.0.3.jar)\. 

Review the JDBC driver version 2\.1 software license and change log file: 
+ [JDBC driver version 2\.1 license](https://github.com/aws/amazon-redshift-jdbc-driver/blob/master/LICENSE) 
+ [JDBC driver version 2\.1 change log](https://github.com/aws/amazon-redshift-jdbc-driver/blob/master/CHANGELOG.md)

JDBC drivers version 1\.2\.27\.1051 and later support Amazon Redshift stored procedures\. For more information, see [Creating stored procedures in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/stored-procedure-overview.html) in the *Amazon Redshift Database Developer Guide*\. 

If your tool requires a specific previous version of a JDBC driver version 2\.1, see [Previous versions of JDBC driver version 2\.1 ](jdbc20-previous-driver-version-20.md)\.