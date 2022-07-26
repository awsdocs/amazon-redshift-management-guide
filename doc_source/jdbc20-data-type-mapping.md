# Converting data types<a name="jdbc20-data-type-mapping"></a>

The Amazon Redshift JDBC driver version 2\.1 supports many common data formats, converting between Amazon Redshift, SQL, and Java data types\.

The following table lists the supported data type mappings\.


| Amazon Redshift type | SQL type | Java type | 
| --- | --- | --- | 
|  BIGINT  |  SQL\_BIGINT  |  Long  | 
|  BOOLEAN  |  SQL\_BIT  |  Boolean  | 
|  CHAR  |  SQL\_CHAR  |  String  | 
|  DATE  |  SQL\_TYPE\_DATE  |  java\.sql\.Date  | 
|  DECIMAL  |  SQL\_NUMERIC  |  BigDecimal  | 
|  DOUBLE PRECISION  |  SQL\_DOUBLE  |  Double  | 
|  GEOMETRY  |  SQL\_ LONGVARBINARY  |  byte\[\]  | 
|  INTEGER  |  SQL\_INTEGER  |  Integer  | 
|  OID  |  SQL\_BIGINT  |  Long  | 
|  SUPER  |  SQL\_LONGVARCHAR  |  String  | 
|  REAL  |  SQL\_REAL  |  Float  | 
|  SMALLINT  |  SQL\_SMALLINT  |  Short  | 
|  TEXT  |  SQL\_VARCHAR  |  String  | 
|  TIME  |  SQL\_TYPE\_TIME  |  java\.sql\.Time  | 
|  TIMETZ  |  SQL\_TYPE\_TIME  |  java\.sql\.Time  | 
|  TIMESTAMP  |  SQL\_TYPE\_ TIMESTAMP  |  java\.sql\.Timestamp  | 
|  TIMESTAMPTZ  |  SQL\_TYPE\_ TIMESTAMP  |  java\.sql\.Timestamp  | 
|  VARCHAR  |  SQL\_VARCHAR  |  String  | 