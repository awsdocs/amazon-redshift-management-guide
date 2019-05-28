# ODBC Driver Configuration Options<a name="configure-odbc-options"></a>

You can use the configuration options described in the following table to control the behavior of the Amazon Redshift ODBC driver\.

In Microsoft Windows, you typically set driver options when you configure a data source name \(DSN\)\. You can also set driver options in the connection string when you connect programmatically, or by adding or changing registry keys in HKEY\_LOCAL\_MACHINE\\SOFTWARE\\ODBC\\ODBC\.INI\\*your\_DSN*\. For more information about configuring a DSN, see [Install and Configure the Amazon Redshift ODBC Driver on Microsoft Windows Operating Systems](install-odbc-driver-windows.md)\. For an example of setting driver options in a connection string, see [Connect to Your Cluster Programmatically](connecting-in-code.md)\.

In Linux and Mac OS X, you set driver configuration options in your odbc\.ini and amazon\.redshiftodbc\.ini files, as described in [Configure the ODBC Driver on Linux and Mac OS X Operating Systems](odbc-driver-configure-linux-mac.md)\. Configuration options set in an amazon\.redshiftodbc\.ini file apply to all connections\. In contrast, configuration options set in an odbc\.ini file are specific to a connection\. Configuration options set in odbc\.ini take precedence over configuration options set in amazon\.redshiftodbc\.ini\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-options.html)