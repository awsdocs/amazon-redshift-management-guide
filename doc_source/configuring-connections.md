# Configuring connections in Amazon Redshift<a name="configuring-connections"></a>

In the following section, you can find how to configure JDBC and ODBC connections to connect to your cluster from SQL client tools\. This section describes how to set up JDBC and ODBC connections\. It also describes how to use Secure Sockets Layer \(SSL\) and server certificates to encrypt communication between the client and server\. 

**Topics**
+ [JDBC and ODBC drivers for Amazon Redshift](#connecting-drivers)
+ [Configuring a JDBC connection](configure-jdbc-connection.md)
+ [Configuring an ODBC connection](configure-odbc-connection.md)

## JDBC and ODBC drivers for Amazon Redshift<a name="connecting-drivers"></a>

To work with data in your cluster, you need JDBC or ODBC drivers for connectivity from your client computer or instance\. Code your applications to use JDBC or ODBC data access API operations, and use SQL client tools that support either JDBC or ODBC\.

Amazon Redshift offers JDBC and ODBC drivers for download\. Previously, Amazon Redshift recommended PostgreSQL drivers for JDBC and ODBC\. If you currently use those drivers, we recommend moving to the new Amazon Redshift–specific drivers\. For more information about how to download the JDBC and ODBC drivers and configure connections to your cluster, see [Configuring a JDBC connection](configure-jdbc-connection.md) and [Configuring an ODBC connection](configure-odbc-connection.md)\. 

### Finding your cluster connection string<a name="connecting-connection-string"></a>

To connect to your cluster with your SQL client tool, you need the cluster connection string\. You can find the cluster connection string in the Amazon Redshift console, on a cluster's details page\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

#### New console<a name="connect-drivers-url"></a>

**To find the connection string for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster name from the list to open its details\. 

1. Choose the **Properties** tab for the cluster\. Then view **Connection details** and choose **View all connection details** to see the **JDDBC URL** and **ODBC URL** values\. The connection string is based on the AWS Region where the cluster runs\. 

1. Choose **Copy** to copy the connection string needed for your driver\. 

#### Original console<a name="connect-drivers-url-originalconsole"></a>

**To get your cluster connection string**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the **Clusters** page, choose the name of the cluster for which you want to get the connection string\.

1. On the cluster's **Configuration** tab, under **JDBC URL** or **ODBC URL**, copy the connection string\.

   The following example shows the connection strings of a cluster launched in the US West \(Oregon\) Region\. If you launch your cluster in a different AWS Region, the connection strings are based that Region's endpoint\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-database-properties.png)