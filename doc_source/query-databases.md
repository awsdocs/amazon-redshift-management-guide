# Querying a database<a name="query-databases"></a>

To query databases hosted by your Amazon Redshift cluster, you have two options:
+ Connect to your cluster and run queries on the AWS Management Console with the query editor\. 

  If you use the query editor on the Amazon Redshift console, you don't have to download and set up a SQL client application\. 
+ Connect to your cluster through a SQL client tool, such as SQL Workbench/J\. 

  Amazon Redshift supports SQL client tools connecting through Java Database Connectivity \(JDBC\) and Open Database Connectivity \(ODBC\)\. Amazon Redshift doesn't provide or install any SQL client tools or libraries, so you must install them on your client computer or Amazon EC2 instance to use them\. You can use most SQL client tools that support JDBC or ODBC drivers\.

**Topics**
+ [Querying a database using the query editor](query-editor.md)
+ [Connecting to an Amazon Redshift cluster using SQL client tools](connecting-to-cluster.md)