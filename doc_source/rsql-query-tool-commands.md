# Amazon Redshift RSQL commands<a name="rsql-query-tool-commands"></a>

Amazon Redshift RSQL commands return informational records about databases or specific database objects\. Results can include various columns and metadata\. Other commands perform specific actions\.

## \\d\[S\+\]<a name="rsql-query-tool-describe-d"></a>

 Lists local user created tables, regular views, late\-binding views and materialized views\. `\dS ` also lists tables and views, like `\d`, but system objects are included in the returned records\. The `+` results in the additional metadata column `description` for all listed objects\. The following shows sample records returned as a result of the command\. 

```
List of relations
 schema |   name    | type  |  owner  
--------+-----------+-------+---------
 public | category  | table | awsuser
 public | date      | table | awsuser
 public | event     | table | awsuser
 public | listing   | table | awsuser
 public | sales     | table | awsuser
 public | users     | table | awsuser
 public | venue     | table | awsuser
(7 rows)
```

## \\d\[S\+\] NAME<a name="rsql-query-tool-describe-s-plus-named"></a>

Describes a table, view, or index\. Includes the column names and types\. It also provides the diststyle, backup configuration, create date \(tables created after October 2018\), and constraints\. For example, `\dS+ sample` returns object properties\. Appending `S+` results in additional columns included in the returned records\.

```
Table "public.sample"
 Column |            Type             |   Collation    | Nullable | Default Value | Encoding  | DistKey | SortKey
--------+-----------------------------+----------------+----------+---------------+-----------+---------+---------
 col1   | smallint                    |                | NO       |               | none      | t       | 1
 col2   | character(100)              | case_sensitive | YES      |               | none      | f       | 2
 col3   | character varying(100)      | case_sensitive | YES      |               | text32k   | f       | 3
 col4   | timestamp without time zone |                | YES      |               | runlength | f       | 0
 col5   | super                       |                | YES      |               | zstd      | f       | 0
 col6   | bigint                      |                | YES      |               | az64      | f       | 0

Diststyle: KEY
Backup: YES
Created: 2021-07-20 19:47:27.997045
Unique Constraints:
    "sample_pkey" PRIMARY KEY (col1)
    "sample_col2_key" UNIQUE (col2)
Foreign-key constraints:
    "sample_col2_fkey" FOREIGN KEY (col2) REFERENCES lineitem(l_orderkey)
```

The distribution style, or *Diststyle*, of the table can be KEY, AUTO, EVEN or ALL\.

*Backup* indicates if the table is backed up when a snapshot is taken\. Valid values are `YES` or `NO`\.

*Created* is the timestamp for when the table is created\. The creation date isn't available for Amazon Redshift tables created before November 2018\. Tables created before this date display n/a \(Not Available\)\. 

*Unique Constraints* lists unique and primary key constraints on the table\.

*Foreign\-key constraints* lists foreign\-key constraints on the table\.

## \\dC\[\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dc"></a>

Lists casts\. Includes the source type, target type, and whether the cast is implicit\.

The following shows a subset of results from `\dC+`\.

```
List of casts
         source type         |         target type         |      function       |   implicit?   | description 
-----------------------------+-----------------------------+---------------------+---------------+-------------
 "char"                      | character                   | bpchar              | in assignment | 
 "char"                      | character varying           | text                | in assignment | 
 "char"                      | integer                     | int4                | no            | 
 "char"                      | text                        | text                | yes           | 
 "path"                      | point                       | point               | no            | 
 "path"                      | polygon                     | polygon             | in assignment | 
 abstime                     | date                        | date                | in assignment | 
 abstime                     | integer                     | (binary coercible)  | no            | 
 abstime                     | time without time zone      | time                | in assignment | 
 abstime                     | timestamp with time zone    | timestamptz         | yes           | 
 abstime                     | timestamp without time zone | timestamp           | yes           | 
 bigint                      | bit                         | bit                 | no            | 
 bigint                      | boolean                     | bool                | yes           | 
 bigint                      | character                   | bpchar              | in assignment | 
 bigint                      | character varying           | text                | in assignment | 
 bigint                      | double precision            | float8              | yes           | 
 bigint                      | integer                     | int4                | in assignment | 
 bigint                      | numeric                     | numeric             | yes           | 
 bigint                      | oid                         | oid                 | yes           | 
 bigint                      | real                        | float4              | yes           | 
 bigint                      | regclass                    | oid                 | yes           | 
 bigint                      | regoper                     | oid                 | yes           | 
 bigint                      | regoperator                 | oid                 | yes           | 
 bigint                      | regproc                     | oid                 | yes           | 
 bigint                      | regprocedure                | oid                 | yes           | 
 bigint                      | regtype                     | oid                 | yes           | 
 bigint                      | smallint                    | int2                | in assignment | 
 bigint                      | super                       | int8_partiql        | in assignment |
```

## \\dd\[S\] \[PATTERN\]<a name="rsql-query-tool-describe-dds"></a>

Shows object descriptions not displayed elsewhere\.

## \\de<a name="rsql-query-tool-describe-de"></a>

Lists external tables\. This includes tables in the AWS Glue data catalog, Hive Metastore and federated tables from Amazon RDS/Aurora MySQL, Amazon RDS/Aurora PostgreSQL and Amazon Redshift datashare tables\.

## \\de NAME<a name="rsql-query-tool-describe-de-name"></a>

Describes an external table\.

The following example shows an AWS Glue external table\.

```
# \de spectrum.lineitem
                            Glue External table "spectrum.lineitem"
     Column      | External Type | Redshift Type | Position | Partition Key | Nullable
-----------------+---------------+---------------+----------+---------------+----------
 l_orderkey      | bigint        | bigint        | 1        | 0             |
 l_partkey       | bigint        | bigint        | 2        | 0             |
 l_suppkey       | int           | int           | 3        | 0             |
 l_linenumber    | int           | int           | 4        | 0             |
 l_quantity      | decimal(12,2) | decimal(12,2) | 5        | 0             |
 l_extendedprice | decimal(12,2) | decimal(12,2) | 6        | 0             |
 l_discount      | decimal(12,2) | decimal(12,2) | 7        | 0             |
 l_tax           | decimal(12,2) | decimal(12,2) | 8        | 0             |
 l_returnflag    | char(1)       | char(1)       | 9        | 0             |
 l_linestatus    | char(1)       | char(1)       | 10       | 0             |
 l_shipdate      | date          | date          | 11       | 0             |
 l_commitdate    | date          | date          | 12       | 0             |
 l_receiptdate   | date          | date          | 13       | 0             |
 l_shipinstruct  | char(25)      | char(25)      | 14       | 0             |
 l_shipmode      | char(10)      | char(10)      | 15       | 0             |
 l_comment       | varchar(44)   | varchar(44)   | 16       | 0             |

Location: s3://redshiftbucket/kfhose2019/12/31
Input_format: org.apache.hadoop.mapred.TextInputFormat
Output_format: org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat
Serialization_lib: org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe
Serde_parameters: {"field.delim":"|","serialization.format":"|"}
Parameters: {"EXTERNAL":"TRUE","numRows":"178196721475","transient_lastDdlTime":"1577771873"}
```

A Hive Metastore table\.

```
# \de emr.lineitem
                     Hive Metastore External Table "emr.lineitem"
     Column      | External Type | Redshift Type | Position | Partition Key | Nullable
-----------------+---------------+---------------+----------+---------------+----------
 l_orderkey      | bigint        | bigint        | 1        | 0             |
 l_partkey       | bigint        | bigint        | 2        | 0             |
 l_suppkey       | int           | int           | 3        | 0             |
 l_linenumber    | int           | int           | 4        | 0             |
 l_quantity      | decimal(12,2) | decimal(12,2) | 5        | 0             |
 l_extendedprice | decimal(12,2) | decimal(12,2) | 6        | 0             |
 l_discount      | decimal(12,2) | decimal(12,2) | 7        | 0             |
 l_tax           | decimal(12,2) | decimal(12,2) | 8        | 0             |
 l_returnflag    | char(1)       | char(1)       | 9        | 0             |
 l_linestatus    | char(1)       | char(1)       | 10       | 0             |
 l_commitdate    | date          | date          | 11       | 0             |
 l_receiptdate   | date          | date          | 12       | 0             |
 l_shipinstruct  | char(25)      | char(25)      | 13       | 0             |
 l_shipmode      | char(10)      | char(10)      | 14       | 0             |
 l_comment       | varchar(44)   | varchar(44)   | 15       | 0             |
 l_shipdate      | date          | date          | 16       | 1             |

Location: s3://redshiftbucket/cetas
Input_format: org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat
Output_format: org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat
Serialization_lib: org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe
Serde_parameters: {"serialization.format":"1"}
Parameters: {"EXTERNAL":"TRUE", "numRows":"4307207", "transient_lastDdlTime":"1626990007"}
```

PostgreSQL external table\.

```
# \de pgrsql.alltypes
                                Postgres Federated Table "pgrsql.alltypes"
 Column |        External Type        |        Redshift Type        | Position | Partition Key | Nullable
--------+-----------------------------+-----------------------------+----------+---------------+----------
 col1   | bigint                      | bigint                      | 1        | 0             |
 col2   | bigint                      | bigint                      | 2        | 0             |
 col5   | boolean                     | boolean                     | 3        | 0             |
 col6   | box                         | varchar(65535)              | 4        | 0             |
 col7   | bytea                       | varchar(65535)              | 5        | 0             |
 col8   | character(10)               | character(10)               | 6        | 0             |
 col9   | character varying(10)       | character varying(10)       | 7        | 0             |
 col10  | cidr                        | varchar(65535)              | 8        | 0             |
 col11  | circle                      | varchar(65535)              | 9        | 0             |
 col12  | date                        | date                        | 10       | 0             |
 col13  | double precision            | double precision            | 11       | 0             |
 col14  | inet                        | varchar(65535)              | 12       | 0             |
 col15  | integer                     | integer                     | 13       | 0             |
 col16  | interval                    | varchar(65535)              | 14       | 0             |
 col17  | json                        | varchar(65535)              | 15       | 0             |
 col18  | jsonb                       | varchar(65535)              | 16       | 0             |
 col19  | line                        | varchar(65535)              | 17       | 0             |
 col20  | lseg                        | varchar(65535)              | 18       | 0             |
 col21  | macaddr                     | varchar(65535)              | 19       | 0             |
 col22  | macaddr8                    | varchar(65535)              | 20       | 0             |
 col23  | money                       | varchar(65535)              | 21       | 0             |
 col24  | numeric                     | numeric(38,20)              | 22       | 0             |
 col25  | path                        | varchar(65535)              | 23       | 0             |
 col26  | pg_lsn                      | varchar(65535)              | 24       | 0             |
 col28  | point                       | varchar(65535)              | 25       | 0             |
 col29  | polygon                     | varchar(65535)              | 26       | 0             |
 col30  | real                        | real                        | 27       | 0             |
 col31  | smallint                    | smallint                    | 28       | 0             |
 col32  | smallint                    | smallint                    | 29       | 0             |
 col33  | integer                     | integer                     | 30       | 0             |
 col34  | text                        | varchar(65535)              | 31       | 0             |
 col35  | time without time zone      | varchar(65535)              | 32       | 0             |
 col36  | time with time zone         | varchar(65535)              | 33       | 0             |
 col37  | timestamp without time zone | timestamp without time zone | 34       | 0             |
 col38  | timestamp with time zone    | timestamp with time zone    | 35       | 0             |
 col39  | tsquery                     | varchar(65535)              | 36       | 0             |
 col40  | tsvector                    | varchar(65535)              | 37       | 0             |
 col41  | txid_snapshot               | varchar(65535)              | 38       | 0             |
 col42  | uuid                        | varchar(65535)              | 39       | 0             |
 col43  | xml                         | varchar(65535)              | 40       | 0             |
```

## \\df\[anptw\]\[S\+\] \[PATTERN\]<a name="rsql-query-tool-df"></a>

 Lists functions of various types\. The command `\df`, for example, returns a list of functions\. Results include properties like name, data\-type returned, access privileges, and additional metadata\. Function types can include triggers, stored procedures, window functions and other types\. When you append `S+` to the command, for example `\dfantS+`, additional metadata columns are included, such as `owner`, `security`, and `access privileges`\. 

## \\dL\[S\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dl"></a>

 Lists data about procedural languages associated with the database\. Information includes the name, such as plpgsql, and additional metadata, which includes whether it is trusted, access privileges, and description\. Sample call is, for example, `\dLS+`, which lists languages and their properties\. When you append `S+` to the command, additional metadata columns are included, such as `call handler` and `access privileges`\. 

Sample results:

```
List of languages
   name    | trusted | internal language |      call handler       |                         validator                          | access privileges |          description           
-----------+---------+-------------------+-------------------------+------------------------------------------------------------+-------------------+--------------------------------
 c         | f       | t                 | -                       | fmgr_c_validator(oid)                                      |                   | Dynamically-loaded C functions
 exfunc    | f       | f                 | exfunc_call_handler()   | -                                                          | rdsdb=U/rdsdb     | 
 internal  | f       | t                 | -                       | fmgr_internal_validator(oid)                               |                   | Built-in functions
 mlfunc    | f       | f                 | mlfunc_call_handler()   | -                                                          | rdsdb=U/rdsdb     | 
 plpgsql   | t       | f                 | plpgsql_call_handler()  | plpgsql_validator(oid)                                     |                   | 
 plpythonu | f       | f                 | plpython_call_handler() | plpython_compiler(cstring,cstring,cstring,cstring,cstring) | rdsdb=U/rdsdb     | 
 sql       | t       | t                 | -                       | fmgr_sql_validator(oid)                                    | =U/rdsdb          | SQL-language functions
```

## \\dm\[S\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dm"></a>

 Lists materialized views\. For example, `\dmS+` lists materialized views and their properties\. When you append `S+` to the command, additional metadata columns are included\. 

## \\dn\[S\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dn"></a>

 Lists schemas\. When you append `S+` to the command, for example `\dnS+`, additional metadata columns are included, such as `description` and `access privileges`\. 

## \\dp \[PATTERN\]<a name="rsql-query-tool-describe-dp"></a>

 Lists table, view, and sequence access privileges\. 

## \\dt\[S\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dt"></a>

 Lists tables\. When you append `S+` to the command, for example `\dtS+`, additional metadata columns are included, such as `description` in this case\. 

## \\du<a name="rsql-query-tool-describe-du"></a>

 Lists the users for the database\. Includes their name and their roles, such as Superuser, and attributes\. 

## \\dv\[S\+\] \[PATTERN\]<a name="rsql-query-tool-describe-dv"></a>

 Lists views\. Includes schema, type, and owner data\. When you append `S+` to the command, for example `\dvS+`, additional metadata columns are included\. 

## \\H<a name="rsql-query-tool-describe-h"></a>

 Turns on HTML output\. This is useful to quickly return formatted results\. For example, `select * from sales; \H` returns results from the sales table, in HTML\. To switch back to tablular results, use `\q`, or quiet\. 

## \\i<a name="rsql-query-tool-describe-i"></a>

 Runs commands from a file\. For example, assuming you have rsql\_steps\.sql in your working directory, the following runs the commands in the file: `\i rsql_steps.sql`\. 

## \\l\[\+\] \[PATTERN\]<a name="rsql-query-tool-describe-l"></a>

 Lists databases\. Includes owner, encoding, and additional information\. 

## \\q<a name="rsql-query-tool-describe-q"></a>

 The quit, or `\q` command, logs off database sessions and exits RSQL\. 

## \\sv\[\+\] VIEWNAME<a name="rsql-query-tool-describe-sv-name"></a>

 Shows a view's definition\. 

## \\timing<a name="rsql-query-tool-describe-timing"></a>

 Shows the run time, for a query, for instance\. 

## \\z \[PATTERN\]<a name="rsql-query-tool-describe-z"></a>

 The same output as \\dp\. 

## \\?<a name="rsql-query-tool-help"></a>

 Shows help information\. The optional parameter specifies the item to explain\. 

## \\EXIT<a name="rsql-query-tool-flow-control-exit"></a>

 Logs off all database sessions and exits Amazon Redshift RSQL\. In addition, you can specify an optional exit code\. For example, `\EXIT 15` will exit the Amazon Redshift RSQL terminal and return exit code 15\.

The following example shows output from a connection and exit from RSQL\.

```
% rsql -D testuser
DSN Connected
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.34.1000
Rsql Version: 1.0.1
Redshift Version: 1.0.29306 
Type "help" for help.

(testcluster) user1@dev=# \exit 15

% echo $?
15
```

## \\LOGON<a name="rsql-query-tool-flow-control-logon"></a>

 Connects to a database\. You can specify connection parameters using positional syntax or as a connection string\.

Command syntax is the following: `\logon {[DBNAME|- USERNAME|- HOST|- PORT|- [PASSWORD]] | conninfo}`

The `DBNAME` is the name of the database to connect to\. The `USERNAME` is the user name to connect as\. The default `HOST` is `localhost`\. The default `PORT` is `5439`\.

When a host name is specified in a `\LOGON` command, it becomes the default host name for additional `\LOGON` commands\. To change the default host name, specify a new `HOST` in an additional `\LOGON` command\.

Sample output from the `\LOGON` command for `user1` follows\.

```
(testcluster) user1@redshiftdb=# \logon dev
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
You are now connected to database "dev" as user "user1".
(testcluster) user1@dev=#
```

Sample output for *user2*\.

```
(testcluster) user1@dev=# \logon dev user2 testcluster2.example.com
Password for user user2: 
DBMS Name: Amazon Redshift
Driver Name: Amazon Redshift ODBC Driver
Driver Version: 1.4.27.1000
Rsql Version: 1.0.1
You are now connected to database "dev" as user "user2" on host "testcluster2.example.com" at port "5439".
(testcluster2) user2@dev=#
```

## \\LOGOFF<a name="rsql-query-tool-flow-control-logoff"></a>

Ends database sessions without exiting RSQL\.

## \\REMARK<a name="rsql-query-tool-flow-control-remark"></a>

 An extension of the `\echo` command\. `\REMARK` prints the specified string to the output stream\. `\REMARK `extends `\echo` by adding the ability to break the output over separate lines\.

The following sample shows output from the command\.

```
(testcluster) user1@dev=# \remark 'hello//world'
hello
world
```

## \\RUN<a name="rsql-query-tool-flow-control-run"></a>

 Runs the Amazon Redshift RSQL script contained in the specified file\. `\RUN` extends the `\i` command by adding an option to skip header lines in a file\.

If the file name includes a comma, semicolon, or space, enclose it in single quotation marks\. Additionally, if text follows the file name, enclose it in quotation marks\. In UNIX, file names are case sensitive\. In Windows, file names are case insensitive\.

The following sample shows output from the command\.

```
(testcluster) user1@dev=# \! cat test.sql
select count(*) as lineitem_cnt from lineitem;
select count(*) as customer_cnt from customer;
select count(*) as orders_cnt from orders;



(testcluster) user1@dev=# \run file=test.sql
 lineitem_cnt
--------------
      4307207
(1 row)

 customer_cnt
--------------
     37796166
(1 row)

 orders_cnt
------------
          0
(1 row)


(testcluster) user1@dev=# \run file=test.sql skip=2
2 records skipped in RUN file.
 orders_cnt
------------
          0
(1 row)
```

## \\OS<a name="rsql-query-tool-flow-control-os"></a>

 An alias for the `\!` command\. `\OS` runs the operating system command that is passed as a parameter\. Control returns to Amazon Redshift RSQL after the command is run\. For example, you can run the following command to print the current system date time and return to the RSQL terminal: `\os date`\.

```
(testcluster) user1@dev=# \os date
Tue Sep 7 20:47:54 UTC 2021
```

## \\GOTO<a name="rsql-query-tool-flow-control-goto"></a>

 A new command for Amazon Redshift RSQL\. `\GOTO` skips all intervening commands and resumes processing at the specified `\LABEL`\. The `\LABEL` must be a forward reference\. You cannot jump to a `\LABEL` that lexically precedes the `\GOTO`\.

The following shows sample output\.

```
(testcluster) user1@dev=# \! cat test.sql
select count(*) as cnt from lineitem \gset
select :cnt as cnt;
\if :cnt > 100
    \goto LABELB
\endif

\label LABELA
\remark 'this is label LABELA'
\label LABELB
\remark 'this is label LABELB'


(testcluster) user1@dev=# \i test.sql
   cnt
---------
 4307207
(1 row)

\label LABELA ignored
\label LABELB processed
this is label LABELB
```

## \\LABEL<a name="rsql-query-tool-flow-control-label"></a>

 A new command for Amazon Redshift RSQL\. `\LABEL` establishes an entry point for running the program, as the target for a `\GOTO` command\.

The following shows sample output from the command\.

```
(testcluster) user1@dev=# \! cat test.sql
select count(*) from lineitem limit 5;
\goto LABELB
\remark "this step was skipped by goto label";
\label LABELA
\remark 'this is label LABELA'
\label LABELB
\remark 'this is label LABELB'



(testcluster) user1@dev=# \i testgoto.sql
  count
 4307193
(1 row)

\label LABELA ignored
\label LABELB processed
this is label LABELB
```

## \\IF \(\\ELSEIF, \\ELSE, \\ENDIF\)<a name="rsql-query-tool-flow-control-if"></a>

 `\IF` and related commands conditionally run portions of the input script\. An extension of the PSQL `\if` \(`\elif`, `\else`, `\endif`\) command\. `\IF` and `\ELSEIF` support boolean expressions including `AND`, `OR` and `NOT` conditions\. 

The following shows sample output from the commands\.

```
(testcluster) user1@dev=# \! cat test.sql
SELECT query FROM stv_inflight LIMIT 1 \gset
select :query as query;
\if :query > 1000000
    \remark 'Query id is greater than 1000000'
\elseif :query = 1000000
    \remark 'Query id is equal than 1000000'
\else
    \remark 'Query id is less than 1000000'
\endif


(testcluster) user1@dev=# \i test.sql 
 query
--------
 994803
(1 row)
 
Query id is less than 1000000
```

Use `ERRORCODE` in your branching logic\.

```
\if :'ERRORCODE' = '00000'
    \remark 'The statement was executed without error'
\else
    \remark :LAST_ERROR_MESSAGE
\endif
```

Use `\GOTO` within an `\IF` block to control how code is run\.