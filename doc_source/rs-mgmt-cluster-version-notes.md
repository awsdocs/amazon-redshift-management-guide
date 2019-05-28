# Cluster Version History<a name="rs-mgmt-cluster-version-notes"></a>

Find notes about cluster versions for Amazon Redshift\.

## Version 1\.0\.7657, 1\.0\.7767<a name="rs-mgmt-cluster-version-notes-7657"></a><a name="rs-mgmt-cluster-version-notes-107657"></a>

Time period: May 22–June 12, 2019

**Features and Improvements**
+ **Amazon Redshift:** You can now use the ALTER TABLE command to increase the size of VARCHAR columns\. 
+ **Amazon Redshift:** Significant improvements to the query performance by caching compiled code segments at scale\. 
+ **Amazon Redshift:** Improved performance of the queries tab on the Amazon Redshift console when cluster is processing heavy workloads\. 
+ **Amazon Redshift:** Performance improvements for queries that involve DISTINCT over many columns\. 
+ **Redshift Spectrum:** UNLOAD and queries that use Spectrum to reference external tables are now eligible for Concurrency Scaling\. 

**Fixes**
+ Fix for DROP DATABASE when dropping external database using JDBC driver\. 

## Versions 1\.0\.7287, 1\.0\.7464, 1\.0\.7562<a name="rs-mgmt-cluster-version-notes-7287"></a><a name="rs-mgmt-cluster-version-notes-107287"></a>

Time period: April 24–May 22, 2019

**Features and Improvements**
+ **Amazon Redshift:** Workload management \(WLM\) now automatically manages the resources required to run queries in the default queue\. Automatic WLM will be deployed in all regions in the upcoming releases\. 
+ **Amazon Redshift:** Support for stored procedures in the PL/pgSQL language\. 
+ **Amazon Redshift:** Performance improvements for date and timestamp values when using EXTRACT\(\), DATEPART\(\) and DATEDIFF\(\)\. 

**Fixes**
+ Fix for an issue with aggregate pushdown below UNION ALL operator for certain data types\. 
+ Improved error handling for REGEXP\_REPLACE\(\)\.

## Versions 1\.0\.6882, 1\.0\.7075, 1\.0\.7078, 1\.0\.7152<a name="rs-mgmt-cluster-version-notes-106882"></a><a name="rs-mgmt-cluster-version-notes-106882"></a>

Time period: April 3–May 1, 2019

**Features and Improvements**
+ **Amazon Redshift:** Augmented the UNLOAD command to apply Zstandard compression to text and CSV files unloaded to Amazon S3\.
+ **Amazon Redshift:** Performance improvements in ORDER BY processing when the ORDER BY clause has constants\. 
+ **Redshift Spectrum:** Improved error reporting when accessing Amazon S3 data in a different region than the Amazon Redshift cluster\.
+ **Redshift Spectrum:** Performance enhancement for queries against external tables in ORC format in Amazon S3 by using file level statistics\.

**Fixes**
+ Improved handling of user interrupts during query planning\.
+ Tolerate 'ERROR: table <id> dropped by concurrent transaction' in late\-binding views
+ Fixed an issue with IS NOT NULL handling in certain queries\.
+  Fixed a cardinality issue with LEFT OUTER JOIN queries\.

## Versions 1\.0\.6527, 1\.0\.6630, 1\.06670, 1\.0\.6761, 1\.0\.6805<a name="rs-mgmt-cluster-version-notes-106527"></a><a name="rs-mgmt-cluster-version-notes-106527"></a>

Time period: March 13–April 5, 2019

**Features and Improvements**
+ **Amazon Redshift:** now automatically and elastically scales query processing power to provide consistently fast performance for hundreds of concurrent queries\. Concurrency Scaling resources are added to your Amazon Redshift cluster transparently in seconds, as concurrency increases, to process queries without wait time\. 
+ **Amazon Redshift:** You can now UNLOAD the result of a query to one or more text files in CSV format to Amazon S3\. 
+ **Amazon Redshift:** You can use the COPY command to load data from ZSTD compressed files\.
+ **Amazon Redshift:** The TO\_DATE\(\) and TO\_TIMESTAMP\(\) functions now accept new format fields\.

**Fixes**
+ Fix for a query issue when arguments to user\-defined functions are NULL\.
+ Fix for a query issue when LIKE predicates are used\.
+ Fix for an issue with mismatched datatypes in queries using SET operations

## Versions 1\.0\.6145, 1\.0\.6230, 1\.0\.6246, 1\.0\.6431, 1\.0\.6476, 1\.0\.6754<a name="rs-mgmt-cluster-version-notes-106145"></a><a name="rs-mgmt-cluster-version-notes-106145"></a>

Time period: February 20–March 21, 2019

**Features and Improvements**
+ **Amazon Redshift:** Significant performance improvements by enhancing the Redshift optimizer for queries including NOT IN hash joins\. 
+ **Amazon Redshift:** Optimized processing of LEAST\(\) and GREATEST\(\) over constant input expressions\. 
+ **Amazon Redshift:** Faster Classic resize with optimized data transfer protocol\. 
+ **Amazon Redshift:** Improved join performance via early elimination of non\-matching rows with Bloom filters\. 
+ **Amazon Redshift Spectrum:** Spectrum Request Accelerator is automatically and transparently enabled, significantly improving the performance of queries against data in Amazon S3\. 

**Fixes**
+ Fix for an issue with predicate pushdown handling for outer joins\.

## Versions 1\.0\.5749, 1\.0\.5833<a name="rs-mgmt-cluster-version-notes-105749"></a><a name="rs-mgmt-cluster-version-notes-105749"></a>

Time period: Jan 23–Feb 20, 2019

**Features and Improvements**
+ **Amazon Redshift:** Automatic Analyze now prioritizes the collection of statistics for your tables based on the extent to which table data has changed\. Up\-to\-date statistics improve overall system performance\.
+ **Amazon Redshift:** Enhanced accuracy of the APPROXIMATE COUNT DISTINCT algorithm\. 
+ **Amazon Redshift:**Improved resiliency of ROLLBACK processing in case the queue concurrency is set to zero\.

**Fixes**
+ Fix for an issue when handling regex functions such as REGEXP\_SUBSTR\(\)\. 
+ Fix for an issue with mismatched datatypes in queries using SET operations\. 
+ Fix for an issue with the function pg\_get\_late\_binding\_view\_cols\(\)\.

## Versions 1\.0\.5494, 1\.0\.5671<a name="rs-mgmt-cluster-version-notes-105494"></a><a name="rs-mgmt-cluster-version-notes-105494"></a>

Time period: Jan 2–Jan 23, 2019

**Features and Improvements**
+ **Amazon Redshift:** Significant performance improvements to the Amazon Redshift communication layer\. 
+ **Amazon Redshift:** Significant performance improvements for complex queries that create internal temporary tables during query execution\. 
+ **Redshift Spectrum:** Spectrum now supports filtering row\-groups in Parquet and ORC files based on the statistics embedded in these files\. 
+ **Redshift Spectrum:** You can now COPY a SMALLINT column type from Parquet file format\.

**Fixes**
+ Fix for an issue when the UNLOAD command is used with the MANIFEST option\. 
+ Fix for an issue with predicate pushdown handling for outer joins\.

## Versions 1\.0\.5122, 1\.0\.5290, 1\.0\.5463<a name="rs-mgmt-cluster-version-notes-105122"></a><a name="rs-mgmt-cluster-version-notes-105122"></a>

Time period: Dec 3–Dec 24, 2018

**Features and Improvements**
+ **Amazon Redshift:** Improved performance of hash\-based aggregates via more effective memory utilization\. 
+ **Amazon Redshift:** Improved performance of queries by elimination of redundant IN subqueries\. ****
+ **Amazon Redshift:** Improved performance of queries that use outer joins\. 
+ **Amazon Redshift:** Improved performance of large cluster resizes\. 
+ **Amazon Redshift:** Improved performance for queries that require large hash tables to be created during a hash join operation\. 
+ **Redshift Spectrum:** Amazon Redshift now supports dropping an external database by including the DROP EXTERNAL DATABASE clause with a DROP SCHEMA command\. ****
+ **Redshift Spectrum:** RedshiftSpectrum now supports filtering row\-groups in Parquet and ORC files based on the statistics embedded in these files\. 

**Fixes**
+ Fix for an issue with UNLOAD to S3 of tables that include varchar columns of maximum length\. 
+ Fix for an issue where certain queries over a view may fail when the underlying table structure was altered via 'ALTER TABLE ADD COLUMN'\. 
+ Fix for an issue in PERCENTILE\_CONT\(\) window function when computing the 50th percentile\. 
+ Fix for a query issue where the LIMIT clause was being ignored in certain subqueries\. 
+ Fix for an issue with handling of NULL values in VARCHAR columns when copying Parquet files\. 

## Versions 1\.0\.4789, 1\.0\.4852, 1\.0\.4896, 1\.0\.4936<a name="rs-mgmt-cluster-version-notes-104745"></a><a name="rs-mgmt-cluster-version-notes-4789"></a>

Time period: October 31–Nov 21, 2018

**Features and Improvements**
+ **Amazon Redshift:**Amazon Redshift now automatically updates table statistics on your cluster via the new Auto Analyze feature\. Up\-to\-date statistics help the query planner to choose optimal plans and run queries faster\. Auto Analyze will be rolling out in all regions in the upcoming releases\. 
+ **Amazon Redshift:** Added a DISTSTYLE AUTO distribution style as a default for newly created tables\. With DISTSTYLE AUTO, Amazon Redshift now determines optimal distribution style based on table size\. 
+ **Amazon Redshift:** VACUUM DELETE now automatically runs in the background to reclaim the space freed by deleted rows\. Auto vacuum delete will roll out in all regions in the upcoming releases\. 
+ **Amazon Redshift:** Performance improvements for certain hash joins when the inner table join key column is the primary key\. 
+ **Amazon Redshift:** Significant performance improvements by using query rewrites that push down selective joins into a subquery 
+ **Amazon Redshift:** Optimize GROUP BY processing by leveraging informational constraints\. 
+ **Amazon Redshift:** Added support for cross region UNLOAD with REGION parameter\. 
+ **Amazon Redshift:** When the MANIFEST VERBOSE option is used with the UNLOAD command, we now add additional information in the manifest indicating that the author of the manifest is Redshift and the version of the manifest\. 
+ **Amazon Redshift:** Added support to cancel an ongoing resize\. 
+ **Amazon Redshift:** Update timezone information to Time Zone Database version 2018e\. 
+ **Redshift Spectrum:** Query support for nested data has been extended to support arrays of arrays and arrays of maps\. 
+ **Redshift Spectrum:** Added support for map data types in Redshift Spectrum to contain arrays\. 

**Fixes**
+ Improved memory utilization when many complex views are processed via queries over SVV\_COLUMNS\. 
+ Fix for an issue when using 'orc\.schema\.resolution' table property with nested external tables\. 
+ Fix for an issue with INSERT INTO queries that scan nested external tables\. 

## Versions 1\.0\.4349, 1\.0\.4515<a name="rs-mgmt-cluster-version-notes-104349"></a><a name="rs-mgmt-cluster-version-notes-104515"></a>

Time period: October 10–October 31, 2018

**Features and Improvements**
+ **Amazon Redshift:** You can now specify the VERBOSE option in the UNLOAD command to generate additional schema and metadata information inside the manifest\. 
+ **Amazon Redshift:** You can now manage \(both see and cancel\) the components of a multi\-part query in the AWS console\. 
+ **Amazon Redshift:** Significant performance improvements by optimizing the data redistribution strategy during query planning 
+ **Redshift Spectrum:** You can now use Redshift Spectrum with an Amazon Redshift cluster that is configured to use Enhanced VPC Routing\. 
+ **Redshift Spectrum:** Improved performance when retrieving metadata for late\-binding views\. 
+ **Redshift Spectrum:** Improved performance when using ILIKE and NOT ILIKE functions in a Redshift Spectrum query 
+ **Redshift Spectrum:** ALTER TABLE ADD PARTITION now supports creating multiple partitions at once\. This makes managing tables with many partitions faster and more convenient\.

**Fixes**
+ Fix for an issue where queries on svv\_columns could return an error if there were late\-binding views on the cluster\. 
+ Fix for an issue where certain conversion from TIMESTAMP to CHAR or VARCHAR could truncate milliseconds\. 
+ Fix for an issue where some very large Redshift Spectrum queries could encounter out of memory errors\. 

## Versions 1\.0\.3945, 1\.0\.4081, 1\.0\.4222<a name="rs-mgmt-cluster-version-notes-103945"></a><a name="rs-mgmt-cluster-version-notes-104222"></a>

Time period: September 19–October 10, 2018

**Features and Improvements**
+ **Amazon Redshift:** You can specify the HEADER option in the UNLOAD command to add a specific header row to each created output file\. The header row contains the column names created by the unload query\. 
+ **Amazon Redshift: **Significant performance improvements for queries with LIKE predicates\. 
+ **Amazon Redshift:** Improved query performance for mixed read and write workloads\.

**Fixes**
+ Enhanced the performance of catalog access for clusters with highly demanding workloads\. 
+ Fix for an issue when a lateral column alias reference is used with window functions\. 
+ Fix for an issue with evaluating NULL values in LEAST\(\) and GREATEST\(\) functions\. 
+ Fix for an issue for certain queries specifying LIMIT 0\. 
+ Fix for an issue when terminating a session with pg\_terminate\_backend while the session was returning data\. 

## Versions 1\.0\.3639, 1\.0\.3688<a name="rs-mgmt-cluster-version-notes-103639"></a><a name="rs-mgmt-cluster-version-notes-103688"></a>

Time period: August 29–September 19, 2018

**Features and Improvements**
+ **Amazon Redshift:** Significant performance improvements for complex queries that create internal temporary tables during query execution\.
+ **Amazon Redshift:** Significant performance improvements for some queries with rank functions: rank\(\), dense\_rank\(\) and row\_number\(\) in a subquery\.
+ **Redshift Spectrum:** Query support for nested data has been extended to support EXISTS and NOT EXISTS subqueries\.
+ **Redshift Spectrum:** Performance improvement of IN\-list predicate processing in Redshift Spectrum scans\.

**Fixes**
+ Fix for an issue in queries with a UDF that keeps running beyond the requirements of the LIMIT clause\.
+  Fix for a permission issue when a regular view references a late binding view\.

## Versions 1\.0\.3324, 1\.0\.3359<a name="rs-mgmt-cluster-version-notes-103324"></a><a name="rs-mgmt-cluster-version-notes-103359"></a>

Time period: August 9–August 28, 2018

**Features and Improvements**
+ **Amazon Redshift:** Improved performance for write and update workloads by automatically enhancing the locality of system metadata during the maintenance window of your cluster\.
+ **Amazon Redshift:** Significant performance improvements for complex `EXCEPT` subqueries\.
+ **Amazon Redshift:** Performance improvements for joins involving large numbers of `NULL` values in a join key column\. 
+ **Amazon Redshift:** Performance improvement for queries that refer to stable functions over constant expressions\.
+ **Amazon Redshift:** Performance improvements for queries with intermediate subquery results that can be distributed\.
+ **Redshift Spectrum:** Performance improvements for queries with expressions on the partition columns of external tables\.
+ **Redshift Spectrum:** You can now specify the root of an S3 bucket as the data source for an external table\.

**Fixes**
+ Fix for permission error when the definition of a regular view refers to a late\-binding view\.
+ Fix for error 'variable not found in subplan target lists' that affected some queries\.

## Version 1\.0\.3025<a name="rs-mgmt-cluster-version-notes-103025"></a>

Time period: July 19–August 8, 2018

**Features and Improvements**
+ **Amazon Redshift:** Queries can now refer to a column alias within the same query immediately after it is declared, improving the readability of complex SQL queries\.
+ **Amazon Redshift:** DC1 reserved nodes can now be migrated for free to DC2 with no change in term length\.
+ **Amazon Redshift:** A new Amazon CloudWatch metric tracks the current number of waiting queries per workload management \(WLM\) queue\.
+ **Amazon Redshift:** Made significant performance improvements for single\-row inserts into a table\.
+ **Amazon Redshift:** Made significant performance improvements for queries operating over CHAR and VARCHAR columns\.
+ **Amazon Redshift:** Improved performance for hash joins that have filtering dimensions in inner joins or right outer joins\.
+ **Amazon Redshift:** Improved performance for queries that refer to stable functions with constant expressions\.
+ **Redshift Spectrum:** Extended the SVV\_COLUMNS catalog view to provide information about the columns of late\-binding views\.

**Fixes**
+ Improved memory management for prefetching for wide table scans or aggregations\.
+ Improved behavior for long\-running queries against Redshift Spectrum external tables with a large number of small files\.

## Versions 1\.0\.2762, 1\.0\.2819, 10\.2842<a name="rs-mgmt-cluster-version-notes-102762"></a><a name="rs-mgmt-cluster-version-notes-102819"></a><a name="rs-mgmt-cluster-version-notes-102842"></a>

Time period: June 27–July 18, 2018

**Features and Improvements**
+ **Amazon Redshift:** Improved handling of short read queries leading to reduced contention and better system utilization\.
+ **Amazon Redshift:** Significant enhancement to the Redshift cluster resize feature, enabling you to create temporary tables during the resize operation\.
+ **Amazon Redshift:** Made further improvements to the compiled code cache, leading to an improvement in overall query performance by reducing the number of segments that have to be recompiled\.
+ **Amazon Redshift:** Performance improvements for the COPY operation when ingesting data from Parquet and ORC file formats\.
+ **Redshift Spectrum:** You can now query external columns during a resize operation\.
+ **Redshift Spectrum:** Made performance improvements for Redshift Spectrum queries with aggregations on partition columns\.

**Fixes**
+ Fix for an issue when reassigning a query from the short query queue to a user queue\. 
+ Fix for an issue when pushing down predicates during a query rewrite\.
+ Fix for an issue with the `DATE_TRUNC` function in DST timezones\.
+ Fix for an issue when the LOWER function is evaluated on specific multibyte character sequences\.
+ Fix for a data type derivation issue in a CREATE TABLE AS \(CTAS\) statement\.
+ Enhanced error messaging when the Redshift table total column count limit is exceeded by the number of external tables\.

## Versions 1\.0\.2524, 1\.0\.2557, 1\.02610, 1\.0\.2679, 1\.02719<a name="rs-mgmt-cluster-version-notes-102524"></a><a name="rs-mgmt-cluster-version-notes-102719"></a><a name="rs-mgmt-cluster-version-notes-102557"></a><a name="rs-mgmt-cluster-version-notes-102610"></a><a name="rs-mgmt-cluster-version-notes-102679"></a>

Time period: June 7–July 5, 2018

**Features and Improvements**
+ **Amazon Redshift:** Query Monitoring Rules \(QMR\) now support three times as many rules \(up to 25\)\. You can use QMRs to manage the resource allocation of your Amazon Redshift cluster based on query execution boundaries for WLM queues and take action automatically when a query goes beyond those boundaries\.
+ **Amazon Redshift:** A new Amazon CloudWatch metric, QueryRuntimeBreakdown, is now available\. You can use this metric to get details on the various query execution stages\. For more information, see [Amazon Redshift Performance Data](metrics-listing.md)\. 
+ **Redshift Spectrum:** You can now rename external columns\.
+ **Redshift Spectrum:** You can now specify the file compression type for external tables\.
+ **Amazon Redshift:** Enhanced performance to hash joins when queries involve large joins\. Some complex queries now run three times as fast\.
+ **Amazon Redshift:** Substantial enhancements to the VACUUM DELETE command so that it frees up additional disk space and has improved performance\.
+ **Amazon Redshift:** Late materialization now supports DELETE and UPDATE operations, enhancing query performance\.
+ **Amazon Redshift:** Improved performance for cluster resize operations\.
+ **Redshift Spectrum:** Support for an increased number of add and drop operations on a single external Redshift Spectrum table\. 
+ **Redshift Spectrum:** Enhanced predicate filtering efficiency when using the `DATE_TRUNC` function for timestamp columns\. 

**Fixes**
+ Fix for an issue for queries based on certain views with constants\.
+ Fix for an issue when accessing certain late\-binding views after a schema is renamed\.
+ Fix for an issue related to unsupported join type\.
+ Fix for an issue during query hopping as part of Amazon Redshift Workload Management\.
+ Fix for an issue when compiling certain very large queries\.
+ Fix for whitespace handling in the format string of the TO\_DATE function\.
+ Fix for an issue when canceling some queries\.
+ Fix an issue when joining on a partition column of CHAR or DECIMAL type\.

## Version 1\.0\.2294, 1\.0\.2369<a name="rs-mgmt-cluster-version-notes-102294"></a>

Time period: May 17–June 14, 2018<a name="rs-mgmt-cluster-version-notes-102369"></a>

**Features and Improvements**
+ **Amazon Redshift:** The COPY command now supports loading data in Parquet and ORC columnar formats to Amazon Redshift\. 
+ **Amazon Redshift:** Short Query Acceleration now automatically and dynamically determines the maximum run time for short queries based on your workload to further enhance query performance\. 
+ **Redshift Spectrum:** ALTER TABLE ADD/DROP COLUMN for external tables is now supported by using standard JDBC calls\.
+ **Redshift Spectrum:** Amazon Redshift can now push the LENGTH\(\) string function to Redshift Spectrum, enhancing performance\. 
+ **Amazon Redshift:** Significant improvements to the effectiveness of the compiled code cache leading to an improvement in overall query performance by reducing the number of segments that have to be recompiled\.
+ **Amazon Redshift:** Improved performance of certain UNION ALL queries\.
+ **Amazon Redshift:** Improved performance of the communication layer when dealing with out\-of\-order network packets\. 

**Fixes**
+ Fix for an issue when NULL values are generated for certain queries\.

## Version 1\.0\.2058<a name="rs-mgmt-cluster-version-notes-102058"></a>

Time period: April 19–May 8, 2018

**Features and Improvements**
+ **Monitoring functionality and performance enhancements –** Amazon CloudWatch metrics for Amazon Redshift provide detailed information about your cluster's health and performance\. You can now use the added Query Throughput and Query Latency metrics with Amazon Redshift\.
+ **Console performance for query plan execution details has been significantly improved –** For more information, see [Amazon Redshift Performance Data](metrics-listing.md)\. 
+ **Amazon Redshift:** Enhanced performance for aggregation queries\.
+ **Amazon Redshift:** Added support for closing client connections in case keepalive hasn't been acknowledged\.
+ **Redshift Spectrum:** Support for an increased number of add and drop operations on a single external Redshift Spectrum table\. 
+ **Redshift Spectrum:** Enhanced predicate filtering efficiency when using the `DATE_TRUNC` function for timestamp columns\. 

**Fixes**
+ Fix for an issue during the optimization of certain complex queries having an EXISTS clause\.
+ Fix for a permission error when some late\-binding views are nested within regular views\.
+ Fix for an issue during the creation of certain late\-binding views that specify 'WITH NO SCHEMA BINDING', where the underlying view definition has a WITH clause\.
+ Fix for an issue when deriving data types for columns of certain CREATE TABLE AS SELECT statements involving UNION ALL queries\.
+ Fix for Redshift Spectrum issue for DROP TABLE IF EXISTS when the table doesn't exist\.

## Version 1\.0\.2044<a name="rs-mgmt-cluster-version-notes-102044"></a>

Time period covered: March 16–April 18, 2018

**Features and Improvements**
+ **Increase to the number of tables you can create to 20,000 for 8xlarge cluster types –** You now have greater control and granularity for your data with twice as many tables\.
+ **New search patterns for [REGEXP\_INSTR](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_INSTR.html) and [REGEXP\_SUBSTR](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_SUBSTR.html) –** You can now search for a specific occurrence of a regular expression pattern and specify case sensitivity\.
+ **Amazon Redshift:** Performance of window functions has been significantly improved for 8xlarge clusters\.
+ **Amazon Redshift:** Performance improvement for resize operations\.
+ **Redshift Spectrum:** Now available in Asia Pacific \(Mumbai\) and South America \(São Paulo\) regions\.
+ **Redshift Spectrum:** Support for ALTER TABLE ADD/DROP COLUMN for external tables\. 
+ **Redshift Spectrum:** New column added to `stl_s3query`, `svl_s3query`, and `svl_s3query_summary` showing file formats for external tables\. 

**Fixes**
+ Fix for an issue during query cancellation\.
+ Fix for an issue when dropping temp tables\.
+ Fix for an issue when altering a table\.

## Versions 1\.0\.1792, 1\.0\.1793, 1\.0\.1808, 1\.0\.1865<a name="rs-mgmt-cluster-version-notes-101865"></a><a name="rs-mgmt-cluster-version-notes-101808"></a><a name="rs-mgmt-cluster-version-notes-101793"></a><a name="rs-mgmt-cluster-version-notes-101792"></a>

Time period covered: February 22–March 15, 2018

**Features and Improvements**
+ **Amazon Redshift:** Start\-time scheduling added to improve the response time of complex queries that are decomposed into multiple subqueries\. 
+ **Redshift Spectrum** You can now process scalar JSON and ION file formats for external tables in Amazon S3\.
+ **Amazon Redshift:** Short Query Acceleration: Enhanced accuracy of the ML classifier for highly selective queries\.
+ **Amazon Redshift:** Improved hash join execution time\. 
+ **Amazon Redshift:** Improved runtime of vacuum delete operations on DISTALL tables\.
+ **Redshift Spectrum:** Improved runtime for queries of external tables consisting of multiple small files\. 
+ **Redshift Spectrum:** Improved performance for querying `svv_external_tables` and `svv_external_columns`\. 

**Fixes**
+ Improved handling of Boolean predicates during query rewrites\.
+ Fix for an issue with inconsistent temp table states\.
+ Fix for a race condition between drop and select statements\.
+ Enhanced resiliency during cancellation of some queries with prepared statements\.
+ Enhanced resiliency for some query rewrites\.

## Versions 1\.0\.1691 and 1\.0\.1703<a name="rs-mgmt-cluster-version-notes-101703"></a><a name="rs-mgmt-cluster-version-notes-101691"></a>

Time period: February 1–February 23, 2018

**Features and Improvements**
+ **Amazon Redshift:** Enhanced support for regular expressions\. `REGEXP_INSTR()` and `REGEXP_SUBSTR()` can now msearch for a specific occurrence of a regular expression pattern and specify case sensitivity when matching a pattern\.
+ **Amazon Redshift:** Enhanced result caching to support cursor queries and prepared statements\.
+ **Amazon Redshift:** Enhanced Short Query Acceleration to handle a burst of short queries\.
+ **Amazon Redshift:** Added support for IAM role chaining to assume roles that span accounts\.
+ **Redshift Spectrum:** Pushing to Redshift Spectrum the F\_TIMESTAMP\_PL\_INTERVAL, T\_CoalesceExpr, and DATE\-relates functions\.

**Fixes**
+ Fixes for the calculation of timeouts for very small files on S3\.
+ PSQL\-c was modified to respect the auto\-commit behavior for TRUNCATE
+ Fix for a query compilation failed error\.
+ Added support for a three\-part column names in late\-binding views\.
+ Fix for queries with Boolean predicates inside complex expressions\.
+ Redshift Spectrum: Increased parallelism using smaller split sizes\.

## Version 1\.0\.1636<a name="rs-mgmt-cluster-version-notes-101636"></a>

Time period: January 11–January 25, 2018

**Features and Improvements**
+ **Amazon Redshift:** Released Elastic Short Query Acceleration which improves system responsiveness when there is a burst of short queries in a workload\.
+ **Amazon Redshift:** Enabled result caching for read queries within a transaction block\. 
+ **Redshift Spectrum: Added support for DATE columns in Parquet files and for running CREATE EXTERNAL TABLE command with DATE columns\. ** 
+ **Redshift Spectrum:** Added support for `IF NOT EXISTS` with the `ADD PARTITION` command\.
+ **Amazon Redshift:** Some short queries over tables with the `DISTSTYLE` attributes of either `KET` or `ALL` now run three times as fast\. 

**Fixes**
+ Fix for an intermittent assert error during query processing\. The assert message is of the form 'p \- id=380 not in rowsets list' 
+ Fix for a memory leak during exception handling of certain queries\.
+ Addressed inconsistent results between `CTAS` and `INSERT`\.