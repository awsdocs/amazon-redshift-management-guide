# Amazon Redshift RSQL change log<a name="rsql-query-tool-changelog"></a>

*1\.0\.5 \(2022\-06\-27\)*

Bug Fixes
+ Send SQL error messages to standard error \(stderr\)\.
+ Fixed issue with exit codes when using ON\_ERROR\_STOP\. Scripts now end after encountering an error and return the correct exit codes\.
+ Maxerror is now case insensitive\.

New
+ Added support for ODBC 2\.x driver\.



*1\.0\.4 \(2022\-03\-19\)*
+ Add support for RSPASSWORD environment variable\. Set a password to connect to Amazon Redshift\. For example, `export RSPASSWORD=TestPassw0rd`\.



*1\.0\.3 \(2021\-12\-08\)*

Bug Fixes
+ Fixed dialogue pop up when using `\c` or `\logon` to switch between databases in Windows OS\.
+ Fixed crash when checking ssl information\.



*1\.0\.2 \(2021\-11\-26\)*

Bug Fixes
+ Fixed issue with prompt for superuser when using IAM authentication\.
+ Fixed incorrect 'Failed' status message when running queries that return no rows\.
+ Fixed crash when using SingleRowMode in DSN file\.

New
+ Added DSN connection option to the help content\.

## Amazon Redshift RSQL previous versions<a name="rsql-query-tool-changelog-legacy-versions"></a>

Choose one of the links to download the version of Amazon Redshift RSQL you need, based on your operating system\.

**Linux 64\-bit RPM**
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4-1.x86_64.rpm)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3-1.x86_64.rpm)
+ [RSQL Version 1\.0\.2](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.2/AmazonRedshiftRsql-1.0.2-1.x86_64.rpm)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1-1.x86_64.rpm)

**Mac OS 64\-bit DMG**
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4.dmg)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3.dmg)
+ [RSQL Version 1\.0\.2](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.2/AmazonRedshiftRsql-1.0.2.dmg)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.dmg)

**Windows 64\-bit MSI**
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4.msi)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3.msi)
+ [RSQL Version 1\.0\.2](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.2/AmazonRedshiftRsql-1.0.2.msi)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.msi)