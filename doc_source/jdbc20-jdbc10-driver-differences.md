# Differences between the 2\.1 and 1\.x versions of the JDBC driver<a name="jdbc20-jdbc10-driver-differences"></a>

This section describes the differences in the information returned by the 2\.1 and 1\.x versions of the JDBC driver\. The JDBC driver version 1\.x is discontinued\.

The following table lists the DatabaseMetadata information returned by the getDatabaseProductName\(\) and getDatabaseProductVersion\(\) functions for each version of the JDBC driver\. JDBC driver version 2\.1 obtains the values while establishing the connection\. JDBC driver version 1\.x obtains the values as a result of a query\.


| JDBC driver version | getDatabaseProductName\(\) result | getDatabaseProductVersion\(\) result | 
| --- | --- | --- | 
|  2\.1  |  Redshift  |  8\.0\.2  | 
|  1\.x  |  PostgreSQL  |  08\.00\.0002  | 

The following table lists the DatabaseMetadata information returned by the getTypeInfo function for each version of the JDBC driver\. 


| JDBC driver version | getTypeInfo result | 
| --- | --- | 
|  2\.1  |  Consistent with Redshift datatypes  | 
|  1\.x  |  Consistent with PostgreSQL datatypes  | 