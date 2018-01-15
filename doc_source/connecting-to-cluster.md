# Connecting to a Cluster<a name="connecting-to-cluster"></a>

You can connect to Amazon Redshift clusters from SQL client tools over Java Database Connectivity \(JDBC\) and Open Database Connectivity \(ODBC\) connections\. Amazon Redshift does not provide or install any SQL client tools or libraries, so you must install them on your client computer or Amazon EC2 instance to use them to work with data in your clusters\. You can use most SQL client tools that support JDBC or ODBC drivers\. 

You can use this section to walk through the process of configuring your client computer or Amazon EC2 instance to use a JDBC or ODBC connection, and related security options for the client connection to the server\. Additionally, in this section you can find information about setting up and connecting from two example third\-party SQL client tools, SQL Workbench/J and psql, if you don't have a business intelligence tool to use yet\. You can also use this section to learn about connecting to your cluster programmatically\. Finally, if you encounter issues when attempting to connect to your cluster, you can review the troubleshooting information in this section to identify possible solutions\. 


+ [Configuring Connections in Amazon Redshift](configuring-connections.md)
+ [Configure a JDBC Connection](configure-jdbc-connection.md)
+ [Configure an ODBC Connection](configure-odbc-connection.md)
+ [Configure Security Options for Connections](connecting-ssl-support.md)
+ [Connecting to Clusters from Client Tools and Code](connecting-via-client-tools.md)
+ [Troubleshooting Connection Issues in Amazon Redshift](troubleshooting-connections.md)