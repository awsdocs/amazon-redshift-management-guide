# Amazon Redshift RSQL variables<a name="rsql-query-tool-variables"></a>

 Some keywords act as variables in RSQL\. You can set each to a specific value, or re\-set the value\. Most are set with `\rset`, which has an interactive mode and a batch mode\. Commands may be defined in lower or upper case\.

## ACTIVITYCOUNT<a name="rsql-query-tool-activitycount"></a>

 Indicates the number of rows affected by the last submitted request\. For a data\-returning request, this is the number of rows returned to RSQL from the database\. The value is 0 or a positive integer\. The maximum value is 18,446,744,073,709,551,615\. 

 The specially treated variable `ACTIVITYCOUNT` is similar to the variable `ROW_COUNT`\. However, `ROW_COUNT` doesn't report a count of affected rows to the client application at command completion for `SELECT`, `COPY` or `UNLOAD`\. But `ACTIVITYCOUNT` does\. 

activitycount\_01\.sql:

```
select viewname, schemaname
from pg_views
where schemaname = 'not_existing_schema';
\if :ACTIVITYCOUNT = 0
\remark 'views do not exist'
\endif
```

Console output:

```
viewname | schemaname
----------+------------
(0 rows)

views do not exist
```

## ERRORLEVEL<a name="rsql-query-tool-describe-rset-errorlevel"></a>

Assigns severity levels to errors\. Use the severity levels to determine a course of action\. If the `ERRORLEVEL` command has not been used, its value is `ON` by default\.

errorlevel\_01\.sql:

```
\rset errorlevel 42P01 severity 0

select * from tbl;

select 1 as col;

\echo exit
\quit
```

Console output:

```
Errorlevel is on.
rsql: ERROR: relation "tbl" does not exist
(1 row)

col
1

exit
```

## HEADING and RTITLE<a name="rsql-query-tool-describe-rset-heading-rtitle"></a>

Enables users to specify a header that appears at the top of a report\. Header specified by the `RSET RTITLE` command automatically includes the current system date of the client computer\.

rset\_heading\_rtitle\_02\.rsql content:

```
\remark Starting...
\rset rtitle "Marketing Department||Confidential//Third Quarter//Chicago"
\rset width 70
\rset rformat on
select * from rsql_test.tbl_currency order by id limit 2;
\exit
\remark Finishing...
```

Console output:

```
Starting...
Rtitle is set to: &DATE||Marketing Department||Confidential//Third Quarter//Chicago (Changes will take effect after RFORMAT is
switched ON)
Target width is 70.
Rformat is on.
09/11/20       Marketing       Department Confidential
                  Third Quarter
                     Chicago
id  | bankid  | name |      start_date
100 |       1 | USD | 2020-09-11 10:51:39.106905
110 |       1 | EUR | 2020-09-11 10:51:39.106905
(2 rows)

Press any key to continue . . .
```

## MAXERROR<a name="rsql-query-tool-describe-rset-maxerror"></a>

Designates a maximum error\-severity level beyond which RSQL terminates job processing\. Return codes are integer values that RSQL returns to the client operating system after completing each job or task\. The value of the return code indicates the completion status of the job or task\. If a script contains a statement that produces an error\-severity level greater than the designated `maxerror` value, RSQL immediately exits\. Therefore, to have RSQL exit on an error\-severity level of 8, use `RSET MAXERROR 7`\.

maxerror\_01\.sql content:

```
\rset maxerror 0
                        
select 1 as col;

\quit
```

Console output:

```
Maxerror is default.
(1 row)

col
1
```

## RFORMAT<a name="rsql-query-tool-describe-rset-heading-rformat"></a>

Enables users to specify whether to apply settings for the formatting commands\.

rset\_rformat\.rsql content:

```
\remark Starting...
\pset border 2
\pset format wrapped
\pset expanded on
\pset title 'Great Title'
select * from rsql_test.tbl_long where id = 500;
\rset rformat
select * from rsql_test.tbl_long where id = 500;
\rset rformat off
select * from rsql_test.tbl_long where id = 500;
\rset rformat on
select * from rsql_test.tbl_long where id = 500;
\exit
\remark Finishing...
```

Console output:

```
Starting...
Border style is 2. (Changes will take effect after RFORMAT is switched ON)
Output format is wrapped. (Changes will take effect after RFORMAT is switched ON)
Expanded display is on. (Changes will take effect after RFORMAT is switched ON)
Title is "Great Title". (Changes will take effect after RFORMAT is switched ON)
id  |                                                             long_string
500 | In general, the higher the number the more borders and lines the tables will have, but details depend on the particular
format.
(1 row)

Rformat is on.
Great Title
+-[ RECORD 1 ]+----------------------------------------------------------------------------------------------------------------------
-----------+
| id           | 500
|
| long_string | In general, the higher the number the more borders and lines the tables will have, but details depend on the
particular format. |
+-------------+----------------------------------------------------------------------------------------------------------------------
-----------+

Rformat is off.
id  |                                                             long_string
500 | In general, the higher the number the more borders and lines the tables will have, but details depend on the particular format.
(1 row)

Rformat is on.
Great Title
+-[ RECORD 1 ]+----------------------------------------------------------------------------------------------------------------------
-----------+
| id           | 500
|
| long_string | In general, the higher the number the more borders and lines the tables will have, but details depend on the
particular format. |
+-------------+----------------------------------------------------------------------------------------------------------------------
-----------+
Press any key to continue . . .
```

## ROW\_COUNT<a name="rsql-query-tool-describe-rset-row_count"></a>

Gets the number of records affected by the previous query\. It's typically used to check a result, like in the following code fragment:

```
SET result = ROW_COUNT;

IF result = 0
...
```

## TITLEDASHES<a name="rsql-query-tool-describe-rset-heading-titledashes"></a>

This control enables users to specify whether a line of dash characters is to be printed above the column data returned for SQL statements\.

Example:

```
\rset titledashes on
select dept_no, emp_no, salary from rsql_test.EMPLOYEE
where dept_no = 100;
\rset titledashes off
select dept_no, emp_no, salary from rsql_test.EMPLOYEE
where dept_no = 100;
```

Console output:

```
dept_no      emp_no          salary
----------- ----------- --------------------
100         1000346        1300.00
100         1000245        5000.00
100         1000262        2450.00

dept_no     emp_no         salary
100         1000346        1300.00
100         1000245        5000.00
100         1000262        2450.00
```

## WIDTH<a name="rsql-query-tool-describe-rset-heading-width"></a>

Sets the output format to wrapped and specifies the target width for each line in a report\. Without a parameter, it returns the current settings for both the format and target width\.

rset\_width\_01\.rsql content:

```
\echo Starting...
\rset width
\rset width 50
\rset width
\quit
\echo Finishing...
```

Console output:

```
Starting...
Target width is 75.
Target width is 50.
Target width is 50.
Press any key to continue . . .
```

Example with parameter:

```
\echo Starting...
\rset rformat on
\pset format wrapped
select * from rsql_test.tbl_long where id = 500;
\rset width 50
select * from rsql_test.tbl_long where id = 500;
\quit
\echo Finishing...
```

Console output:

```
Starting...
Rformat is on.
Output format is wrapped.
id  |                                          long_string
500 | In general, the higher the number the more borders and lines the ta.
    |.bles will have, but details depend on the particular format.
(1 row)

Target width is 50.
id  |                                          long_string
500 | In general, the higher the number the more.
    |. borders and lines the tables will have, b.
    |.ut details depend on the particular format.
    |..
(1 row)
Press any key to continue . . .
```