# Registering the driver class<a name="jdbc20-register-driver-class"></a>

Make sure that you register the appropriate class for your application\. You use following classes to connect the Amazon Redshift JDBC driver to Amazon Redshift data stores:
+ `Driver` classes extend `java.sql.Driver`\.
+ `DataSource` classes extend `javax.sql.DataSource` and `javax.sql.ConnectionPoolDataSource`\.

The driver supports the following fully qualified class names that are independent of the JDBC version:
+ `com.amazon.redshift.jdbc.Driver`
+ `com.amazon.redshift.jdbc.DataSource`

The following example shows how to use the DriverManager class to establish a connection for JDBC 4\.2\.

```
            private static Connection connectViaDM() throws Exception
{
Connection connection = null;
connection = DriverManager.getConnection(CONNECTION_URL);
return connection;
}
```

The following example shows how to use the `DataSource` class to establish a connection\.

```
 private static Connection connectViaDS() throws Exception
{
Connection connection = null;
11
Amazon Redshift JDBC Driver Installation and Configuration Guide
DataSource ds = new com.amazon.redshift.jdbc.DataSource
();
ds.setURL(CONNECTION_URL);
connection = ds.getConnection();
return connection;
}
```