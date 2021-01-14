# Getting the JDBC URL<a name="jdbc20-obtain-url"></a>

Before you can connect to your Amazon Redshift cluster from a SQL client tool, you need to know the JDBC URL of your cluster\. The JDBC URL has the following format: `jdbc:redshift://endpoint:port/database`\.

**Note**  
A JDBC URL specified with the former format of `jdbc:postgresql://endpoint:port/database` still works\.

The fields of the preceding format have the following values\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-obtain-url.html)

The following is an example JDBC URL: `jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev` 

For information about how to get your JDBC connection, see [Finding your cluster connection string](configuring-connections.md#connecting-connection-string)\. 

If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting connection issues in Amazon Redshift](troubleshooting-connections.md)\. 

## Building the connection URL<a name="jdbc20-build-connection-url"></a>

Use the connection URL to supply connection information to the data store that you are accessing\. The following is the format of the connection URL for the Amazon Redshift JDBC driver version 2\.0\. Here, \[Host\] the endpoint of the Amazon Redshift server and \[Port\] is the number of the TCP port that the server uses to listen for client requests\.

```
jdbc:redshift://[Host]:[Port]
```

By default, Amazon Redshift uses port 5439\.

The following is the format of a connection URL that specifies some optional settings\.

```
jdbc:redshift://[Host]:[Port]/[Schema];[Property1]=[Value];
[Property2]=[Value];
```

For example, suppose that you want to connect to port 9000 on an Amazon Redshift cluster in the US West \(N\. California\) Region on AWS\. You also want to access the schema named Default and authenticate the connection using a user name and password\. In this case, you use the following connection URL\.

```
jdbc:redshift://redshift.company.us-west-
                1.redshift.amazonaws.com:9000/Default;UID=amazon;PWD=amazon
```

Do not duplicate properties in the connection URL\.