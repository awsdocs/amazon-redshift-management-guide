# JDBC Driver Configuration Options<a name="configure-jdbc-options"></a>

To control the behavior of the Amazon Redshift JDBC driver, you can append the configuration options described in the following table to the JDBC URL\. For example, the following JDBC URL connects to your cluster using Secure Socket Layer \(SSL\), user \(UID\), and password \(PWD\)\. 

```
jdbc:redshift://examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com:5439/dev?ssl=true&UID=your_username&PWD=your_password 
```

For more information about SSL options, see [Connect Using SSL](connecting-ssl-support.md#connect-using-ssl)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-options.html)