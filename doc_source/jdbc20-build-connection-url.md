# Building the connection URL<a name="jdbc20-build-connection-url"></a>

Use the connection URL to supply connection information to the data store that you are accessing\. The following is the format of the connection URL for the Amazon Redshift JDBC driver version 2\.1\. Here, \[Host\] the endpoint of the Amazon Redshift server and \[Port\] is the number of the Transmission Control Protocol \(TCP\) port that the server uses to listen for client requests\.

```
jdbc:redshift://[Host]:[Port]
```

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

You can use the following characters to separate configuration options in the URL string:
+ ;
+ ?
+ &

For example, the following URL strings are equivalent:

```
jdbc:redshift://my_host:5439/dev;ssl=false;defaultRowFetchSize=100
```

```
jdbc:redshift://my_host:5439/dev?ssl=false?defaultRowFetchSize=100
```

The following URL example specifies a log level of 6 and the path for the logs\.

```
jdbc:redshift://redshift.amazonaws.com:5439/dev;DSILogLevel=6;LogPath=/home/user/logs;
```

Don't duplicate properties in the connection URL\.

For a complete list of the configuration options that you can specify, see [Options for JDBC driver version 2\.1 configuration](jdbc20-configuration-options.md)\. 