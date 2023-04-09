# Configuring a connection for ODBC driver version 2\.x for Amazon Redshift<a name="odbc20-install"></a>

You can use an ODBC connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools and applications\. If your client tool supports JDBC, you can choose to use that type of connection rather than ODBC due to the ease of configuration that JDBC provides\. However, if your client tool doesn't support JDBC, you can follow the steps in this section to set up an ODBC connection on your client computer or Amazon EC2 instance\.

Amazon Redshift provides 64\-bit ODBC drivers for Linux and Windows operating systems; the 32\-bit ODBC drivers are discontinued\. Currently, macOS X is not supported\. Further updates to the 32\-bit ODBC drivers will not be released, except for urgent security patches\. To download and install ODBC drivers for macOS X and 32\-bit operating systems, see [ Configuring an ODBC connection](https://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-connection.html)\.

For the latest information about ODBC driver changes, see the [change log](https://github.com/aws/amazon-redshift-odbc-driver/blob/master/CHANGELOG.md)\.

**Topics**
+ [Getting the ODBC URL](odbc20-getting-url.md)
+ [Installing and configuring the Amazon Redshift ODBC driver on Microsoft Windows](odbc20-install-config-win.md)
+ [Installing and configuring the Amazon Redshift ODBC driver on Linux](odbc20-install-config-linux.md)
+ [Configuring authentication](odbc20-authentication-ssl.md)
+ [Converting data types](odbc20-converting-data-types.md)
+ [Configuring ODBC driver options](odbc20-configuration-options.md)
+ [Previous ODBC driver versions](odbc20-previous-versions.md)