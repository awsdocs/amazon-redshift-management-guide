# Amazon Redshift RSQL<a name="rsql-query-tool"></a>

 Amazon Redshift RSQL is a command\-line client for interacting with Amazon Redshift clusters and databases\. You can connect to an Amazon Redshift cluster, describe database objects, query data, and view query results in various output formats\. 

 Amazon Redshift RSQL supports the capabilities of the PostgreSQL psql command\-line tool with an additional set of capabilities specific to Amazon Redshift\. These include the following: 
+ You can use single sign\-on authentication using ADFS, PingIdentity, Okta, Azure ADm or other SAML/JWT based identity providers\. You can also use browser\-based SAML identity providers for multi\-factor authentication \(MFA\)\.
+ You can describe properties or attributes of Amazon Redshift objects such as table distribution keys, table sort keys, late\-binding views \(LBVs\), and materialized views\. You can also describe properties or attributes of external tables in an AWS Glue catalog or Apache Hive Metastore, external databases in Amazon RDS for PostgreSQL, Amazon Aurora PostgreSQL\-Compatible Edition, RDS for MySQL \(preview\) and Amazon Aurora MySQL\-Compatible Edition \(preview\), and tables shared by using Amazon Redshift data sharing\.
+ You can also use enhanced control flow commands such as `IF` \(`\ELSEIF`, `\ELSE,` `\ENDIF`\), `\GOTO` and `\LABEL`\.

 With Amazon Redshift RSQL batch mode, which runs a script passed as an input parameter, you can run scripts that include both SQL and complex business logic\. If you have existing self\-managed, on\-premises data warehouses, you can use Amazon Redshift RSQL to replace existing extract, transform, load \(ETL\) and automation scripts, such as Teradata BTEQ scripts\. Using RSQL helps you to avoid manually reimplementing scripts in a procedural language\. 

 Amazon Redshift RSQL is available for Linux, Windows, and macOS X operating systems\. 

To report issues for Amazon Redshift RSQL, write to redshift\-rsql\-support@amazon\.com\.