# Getting the JDBC URL<a name="jdbc20-obtain-url"></a>

Before you can connect to your Amazon Redshift cluster from a SQL client tool, you need to know the JDBC URL of your cluster\. The JDBC URL has the following format: `jdbc:redshift://endpoint:port/database`\.

The fields of the preceding format have the following values\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-obtain-url.html)

The following is an example JDBC URL: `jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev` 

Be sure to enter the URL values, for example SessionToken values, in URL encoded format\. 

For information about how to get your JDBC connection, see [Finding your cluster connection string](configuring-connections.md#connecting-connection-string)\. 

If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting connection issues in Amazon Redshift](troubleshooting-connections.md)\. 