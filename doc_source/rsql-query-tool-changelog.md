# Amazon Redshift RSQL change log<a name="rsql-query-tool-changelog"></a>

*1\.0\.7 \(2023\-03\-22\)*

Bug Fixes
+ Fixed an issue where RSQL could not describe materialized views\.
+ Fixed permission\-denied error on stl\_connection\_log when using Amazon Redshift Serverless\.
+ Fixed issue where RSQL may process \\GOTO labels incorrectly\.
+ Fixed issue where SSL messages are printed in quiet mode\.
+ Fixed issue with random characters displayed when describing stored procedures\.
+ Fixed issue with printing duplicate ERROR/INFO messages\.

New
+ RSQL now gets SSL information directly from the ODBC driver\.



*1\.0\.6 \(2023\-02\-21\)*

Bug Fixes
+ Fixed an issue where \\d throws an error \- invalid input syntax for integer: "xid" \- on Redshift patch 1\.0\.46086 \(P173\)\.

New
+ Renamed installation files to reflect supported architecture\.



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



## Amazon Redshift RSQL previous versions<a name="rsql-query-tool-changelog-legacy-versions"></a>

Choose one of the links to download the version of Amazon Redshift RSQL you need, based on your operating system\.

**Linux 64\-bit RPM**
+ [RSQL Version 1\.0\.6](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.6/AmazonRedshiftRsql-1.0.6.x86_64.rpm)
+ [RSQL Version 1\.0\.5](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.5/AmazonRedshiftRsql-1.0.5-1.x86_64.rpm)
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4-1.x86_64.rpm)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3-1.x86_64.rpm)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1-1.x86_64.rpm)

**Mac OS 64\-bit DMG**
+ [RSQL Version 1\.0\.6](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.6/AmazonRedshiftRsql-1.0.6.x86_64.dmg)
+ [RSQL Version 1\.0\.5](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.5/AmazonRedshiftRsql-1.0.5.dmg)
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4.dmg)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3.dmg)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.dmg)

**Windows 64\-bit MSI**
+ [RSQL Version 1\.0\.6](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.6/AmazonRedshiftRsql-1.0.6.x86_64.msi)
+ [RSQL Version 1\.0\.5](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.5/AmazonRedshiftRsql-1.0.5.msi)
+ [RSQL Version 1\.0\.4](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.4/AmazonRedshiftRsql-1.0.4.msi)
+ [RSQL Version 1\.0\.3](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.3/AmazonRedshiftRsql-1.0.3.msi)
+ [RSQL Version 1\.0\.1](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.msi)