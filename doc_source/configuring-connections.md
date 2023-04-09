# Configuring connections in Amazon Redshift<a name="configuring-connections"></a>

In the following section, learn how to configure JDBC, Python, and ODBC connections to connect to your cluster from SQL client tools\. This section describes how to set up JDBC, Python, and ODBC connections\. It also describes how to use Secure Sockets Layer \(SSL\) and server certificates to encrypt communication between the client and server\. 

## JDBC, Python, and ODBC drivers for Amazon Redshift<a name="connecting-drivers"></a>

To work with data in your cluster, you must have JDBC, Python, or ODBC drivers for connectivity from your client computer or instance\. Code your applications to use JDBC, Python, or ODBC data access API operations, and use SQL client tools that support either JDBC, Python, or ODBC\.

Amazon Redshift offers JDBC, Python, and ODBC drivers for download\. These drivers are supported by AWS Support\.  PostgreSQL drivers are not tested and not supported by the Amazon Redshift team\. Use the Amazon Redshift–specific drivers when connecting to an Amazon Redshift cluster\. The Amazon Redshift drivers have the following advantages:
+ Support for IAM, SSO, and federated authentication\.
+ Support for new Amazon Redshift data types\.
+ Support for authentication profiles\.
+ Improved performance in conjunction with Amazon Redshift enhancements\.

 For more information about how to download the JDBC and ODBC drivers and configure connections to your cluster, see [Configuring a connection for JDBC driver version 2\.1 for Amazon Redshift](jdbc20-install.md), [Configuring the Amazon Redshift Python connector](python-redshift-driver.md), and [Configuring an ODBC connection](configure-odbc-connection.md)\. 

For more information about managing IAM identities, including best practices for IAM roles, see [Identity and access management in Amazon Redshift](redshift-iam-authentication-access-control.md)\.

### Finding your cluster connection string<a name="connecting-connection-string"></a>

To connect to your cluster with your SQL client tool, you must have the cluster connection string\. You can find the cluster connection string in the Amazon Redshift console, on a cluster's details page\.

**To find the connection string for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose the cluster name from the list to open its details\.

1. The **JDBC URL** and **ODBC URL** connection strings are available, along with additional details, in the **General information** section\. Each string is based on the AWS Region where the cluster runs\. Click the icon next to the appropriate connection string to copy it\.