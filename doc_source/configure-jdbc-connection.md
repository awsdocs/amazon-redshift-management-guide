# Configure a JDBC Connection<a name="configure-jdbc-connection"></a>

You can use a JDBC connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools\. To do this, you need to download a JDBC driver\. Follow the steps in this section if you want to use a JDBC connection\.


+ [Download the Amazon Redshift JDBC Driver](#download-jdbc-driver)
+ [Obtain the JDBC URL](#obtain-jdbc-url)
+ [JDBC Driver Configuration Options](configure-jdbc-options.md)
+ [Configure a JDBC Connection with Apache Maven](configure-jdbc-connection-with-maven.md)
+ [Previous JDBC Driver Versions](jdbc-previous-versions.md)

## Download the Amazon Redshift JDBC Driver<a name="download-jdbc-driver"></a>

Amazon Redshift offers drivers for tools that are compatible with either the JDBC 4\.2 API, JDBC 4\.1 API, or JDBC 4\.0 API\. For information about the functionality supported by these drivers, go to the [Amazon Redshift JDBC Driver Release Notes](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Release+Notes.pdf)\. 

JDBC drivers version 1\.2\.8\.1005 and later support database authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials\. For more information, see [Using IAM Authentication to Generate Database User Credentials](generating-user-credentials.md)\.

Download one of the following, depending on the version of the JDBC API that your SQL client tool or application uses\. If you're not sure, download the latest version of the JDBC 4\.2 API driver\.

**Note**  
For driver class name, use either com\.amazon\.redshift\.jdbc\.Driver or the version\-specific class name listed with the driver in the list following\.

+ JDBC 4\.2–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-1.2.10.1009.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

+  JDBC 4\.1–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-1.2.10.1009.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc41.Driver`\.

+  JDBC 4\.0–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-1.2.10.1009.jar)\. 

+ The class name for this driver is `com.amazon.redshift.jdbc4.Driver`\.

The standard Amazon Redshift JDBC drivers include the AWS SDK that is required to use IAM database authentication\. We recommend using the standard drivers unless the size of the driver files is an issue for your application\. If you need smaller driver files and you do not use IAM database authentication, or if you already have AWS SDK for Java 1\.11\. 118 or later in your Java class path, then download one of the following drivers\.

+ JDBC 4\.2–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC42\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC42-no-awssdk-1.2.10.1009.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc42.Driver`\.

+  JDBC 4\.1–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC41\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC41-no-awssdk-1.2.10.1009.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc41.Driver`\.

+  JDBC 4\.0–compatible driver: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/RedshiftJDBC4\-no\-awssdk\-1\.2\.10\.1009\.jar](https://s3.amazonaws.com/redshift-downloads/drivers/RedshiftJDBC4-no-awssdk-1.2.10.1009.jar)\. 

  The class name for this driver is `com.amazon.redshift.jdbc4.Driver`\.

Then download and review the [Amazon Redshift JDBC Driver License Agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+JDBC+Driver+License+Agreement.pdf)\. 

If your tool requires a specific previous version of a driver, see [Previous JDBC Driver Versions](jdbc-previous-versions.md)\.

If you need to distribute these drivers to your customers or other third parties, please send email to *redshift\-pm@amazon\.com* to arrange an appropriate license\. 

## Obtain the JDBC URL<a name="obtain-jdbc-url"></a>

Before you can connect to your Amazon Redshift cluster from a SQL client tool, you need to know the JDBC URL of your cluster\. The JDBC URL has the following format: 

jdbc:redshift://*endpoint*:*port*/*database*

**Note**  
A JDBC URL specified with the former format of jdbc:postgresql://*endpoint*:*port*/*database* will still work\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html)

The following is an example JDBC URL: `jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev` 

**To obtain your JDBC URL**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. At top right, select the region in which you created your cluster\. 

    If you followed the *Amazon Redshift Getting Started*, select **US West \(Oregon\)**\. 

1.  In the left navigation pane, click **Clusters**, and then click your cluster\. 

    If you followed the *Amazon Redshift Getting Started*, click `examplecluster`\. 

1.  On the **Configuration** tab, under **Cluster Database Properties**, copy the JDBC URL of the cluster\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-database-properties-jdbc.png)

 If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting Connection Issues in Amazon Redshift](troubleshooting-connections.md)\. 