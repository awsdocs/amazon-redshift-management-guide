# Connecting to an Amazon Redshift cluster using SQL client tools<a name="connecting-to-cluster"></a>

You can connect to Amazon Redshift clusters from SQL client tools over Java Database Connectivity \(JDBC\), Python, and Open Database Connectivity \(ODBC\) connections\. Amazon Redshift doesn't provide or install any SQL client tools or libraries\. To use these tools or libraries to work with data in your clusters, install them on your client computer or Amazon EC2 instance\. You can use most SQL client tools that support JDBC, Python, or ODBC drivers\. 

Use the following sections to walk through the process of configuring your client computer or Amazon EC2 instance to use a JDBC, Python, or ODBC connection\. They also discuss related security options for the client connection to the server\. Additionally, find information about setting up and connecting from SQL client tools, such as SQL Workbench/J, a third\-party tool, and [Amazon Redshift RSQL](https://docs.aws.amazon.com/redshift/latest/mgmt/rsql-query-tool.html)\. You can try these tools if you don't have a business intelligence tool to use yet\. You can also use this section to learn about connecting to your cluster programmatically\. Finally, if you encounter issues when attempting to connect to your cluster, you can review the troubleshooting information in this section to identify possible solutions\. 

**Topics**
+ [Configuring connections in Amazon Redshift](configuring-connections.md)
+ [Configuring security options for connections](connecting-ssl-support.md)
+ [Connecting to clusters from client tools and code](connecting-via-client-tools.md)
+ [Troubleshooting connection issues in Amazon Redshift](troubleshooting-connections.md)