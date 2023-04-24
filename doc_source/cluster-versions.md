# Cluster versions for Amazon Redshift<a name="cluster-versions"></a>

Amazon Redshift regularly releases cluster versions\. Your Amazon Redshift clusters are patched during your system maintenance window\. The timing of the patch depends on your AWS Region and maintenance window settings\. You can view or change your maintenance window settings from the Amazon Redshift console\. For more information about maintenance, see [Cluster maintenance](working-with-clusters.md#rs-cluster-maintenance)\. 

You can view the cluster version of your cluster on the Amazon Redshift console on the **Maintenance** tab of the cluster details\. Or you can see the cluster version in the output of the SQL command:

```
SELECT version();
```

**Topics**
+ [Amazon Redshift patch 174](#cluster-version-174)
+ [Amazon Redshift patch 173](#cluster-version-173)
+ [Amazon Redshift patch 172](#cluster-version-172)
+ [Amazon Redshift patch 171](#cluster-version-171)
+ [Amazon Redshift patch 170](#cluster-version-170)
+ [Amazon Redshift patch 169](#cluster-version-169)
+ [Amazon Redshift patch 168](#cluster-version-168)

## Amazon Redshift patch 174<a name="cluster-version-174"></a>

### 1\.0\.49087 – Released on April 12, 2023<a name="cluster-version-2023-03-11-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.48805 – Released on April 5, 2023<a name="cluster-version-2023-03-11-features"></a>

Release notes for this version:
+ Amazon Redshift introduced additional performance enhancements to string\-heavy queries using BYTEDICT, a new compression encoding in Amazon Redshift that speeds up string\-based data processing between 5x to 63x compared to alternative compression encodings such as LZO or ZSTD\. For more information on this feature, see [ Byte\-dictionary encoding](https://docs.aws.amazon.com/redshift/latest/dg/c_Byte_dictionary_encoding.html) in the *Amazon Redshift Database Developer Guide*\. 

### 1\.0\.48004 – Released on March 17, 2023<a name="cluster-version-2023-03-11-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.47470 – Released on March 11, 2023<a name="cluster-version-2023-03-11-features"></a>

Release notes for this version:
+ Improves query performance on `pg_catalog.svv_table_info`\. Also adds new column `create_time`\. When creating a table, this column stores the date/time stamp in UTC\.
+ Adds support for specifying session level timeout on federated query\.

## Amazon Redshift patch 173<a name="cluster-version-173"></a>

### 1\.0\.49074 – Released on April 12, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.48766 – Released on April 5, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.48714 – Released on April 5, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.48022 – Released on March 17, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.47357 – Released on March 7, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.46987 – Released on February 24, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.46806 – Released on February 18, 2023<a name="cluster-version-2023-01-20-features"></a>

Maintenance release\. No release notes\.

### 1\.0\.46607 – Released on February 13, 2023<a name="cluster-version-2023-01-20-features"></a>

Release notes for this version:
+ We now automatically convert tables with manually set interleaved sort keys to compound sort keys if their distribution style has been set to **DISTSTYLE KEY**, to improve the performance of these tables\. This is done at the time of restoring a snapshot into Amazon Redshift Serverless\.

### 1\.0\.45698 – Released on January 20, 2023<a name="cluster-version-2023-01-20-features"></a>

Release notes for this version:
+ Adds a file extension parameter to the UNLOAD command, so file extensions are automatically added to filenames\.
+ Supports protecting RLS\-protected objects by default when adding them to a datashare or if they're already part of a datashare\. Administrators can now turn off RLS for datashares to allow consumers access to the protected object\.
+ Adds new system tables for monitoring: `SVV_ML_MODEL_INFO`, `SVV_MV_DEPENDENCY`, and `SYS_LOAD_DETAIL`\. Also adds the columns `data_skewness` and `time_skewness` to the system table `SYS_QUERY_DETAIL`\.

## Amazon Redshift patch 172<a name="cluster-version-172"></a>

Cluster versions in this patch: 
+ 1\.0\.46534 – Released on February 18, 2023
+ 1\.0\.46523 – Released on February 13, 2023
+ 1\.0\.46206 – Released on February 1, 2023
+ 1\.0\.45603 – Released on January 20, 2023
+ 1\.0\.44924 – Released on December 19, 2022
+ 1\.0\.44903 – Released on December 18, 2022
+ 1\.0\.44540 – Released on December 13, 2022
+ 1\.0\.44126 – Released on November 23, 2022
+ 1\.0\.43980 – Released on November 17, 2022

### New features and improvements in this patch<a name="cluster-version-2022-11-17-features"></a>
+ Tables created by CTAS are AUTO by default\.
+ Adds support for row\-level security \(RLS\) on materialized views\.
+ Increases the S3 timeout to improve cross\-Region data sharing\.
+ Adds new spatial function `ST_GeomFromGeohash`\.
+ Improves automatic selection of distribution key from composite primary keys to improve out\-of\-the\-box performance\.
+ Adds automatic primary key to distribution key for tables with composite primary keys, improving out\-of\-the\-box performance\.
+ Improves concurrency scaling to allow more queries to scale even as data changes\.
+ Improves data sharing query performance\.
+ Adds Machine Learning probability metrics for classification models\.
+ Adds new system tables for monitoring: `SVV_USER_INFO`, `SVV_MV_INFO`, `SYS_CONNECTION_LOG`, `SYS_DATASHARE_USAGE_PRODUCER`, `SYS_DATASHARE_USAGE_CONSUMER`, and `SYS_DATASHARE_CHANGE_LOG`\.
+ Adds support for querying VARBYTE columns in external tables for Parquet and ORC file types\.

## Amazon Redshift patch 171<a name="cluster-version-171"></a>

Cluster versions in this patch: 
+ 1\.0\.43931 – Released on November 16, 2022
+ 1\.0\.43551 – Released on November 5, 2022
+ 1\.0\.43331 – Released on September 29, 2022
+ 1\.0\.43029 – Released on September 26, 2022

### New features and improvements in this patch<a name="cluster-version-2022-11-09-features"></a>
+ CONNECT BY support: Adds support for the CONNECT BY SQL construct, letting you recursively query the hierarchical data in your data warehouse based on parent\-child relationship within that data set\. 

## Amazon Redshift patch 170<a name="cluster-version-170"></a>

Cluster versions in this patch: 
+ 1\.0\.43922 – Released on November 21, 2022 
+ 1\.0\.43573 – Released on November 7, 2022 
+ 1\.0\.41881 – Released on September 20, 2022
+ 1\.0\.41465 – Released on September 7, 2022 
+ 1\.0\.40325 – Released on July 27, 2022 

### New features and improvements in this patch<a name="cluster-version-2022-07-20-features"></a>
+  ST\_GeomfromGeoJSON: Constructs an Amazon Redshift spatial geometry object from VARCHAR in GeoJSON representation\.

## Amazon Redshift patch 169<a name="cluster-version-169"></a>

Cluster versions in this patch: 
+ 1\.0\.41050 – Released on September 7, 2022
+ 1\.0\.40083 – Released on July 16, 2022 
+ 1\.0\.39734 – Released on July 7, 2022 
+ 1\.0\.39380 – Released on June 23, 2022 
+ 1\.0\.39251 – Released on June 15, 2022 
+ 1\.0\.39009 – Released on June 8, 2022 

### New features and improvements in this patch<a name="cluster-version-2022-06-08-features"></a>
+  Adds role as a parameter for the Alter Default Privileges command to support Role\-based Access Control\.
+  Adds ACCEPTINVCHARS parameter to support replacing invalid UTF\-8 characters when copying from Parquet and ORC files\.
+  Adds OBJECT\(k,v\) function to construct SUPER objects from key and value pairs\.

## Amazon Redshift patch 168<a name="cluster-version-168"></a>

Cluster versions in this patch: 
+ 1\.0\.38698 – Released on May 25, 2022 
+ 1\.0\.38551 – Released on May 20, 2022 
+ 1\.0\.38463 – Released on May 18, 2022 
+ 1\.0\.38361 – Released on May 13, 2022 
+ 1\.0\.38199 – Released on May 9, 2022 
+ 1\.0\.38112 – Released on May 6, 2022
+ 1\.0\.37684 – Released on April 20, 2022

### New features and improvements in this patch<a name="cluster-version-2022-04-19-features"></a>
+ Adds support for the Linear Learner model type in Amazon Redshift ML\.
+ Adds SNAPSHOT option for SQL transaction isolation level\.
+ Adds `farmhashFingerprint64` as new hashing algorithm for `VARBYTE` and `VARCHAR` data\.
+ Supports the AVG function in incremental refresh of materialized views\.
+ Supports correlated sub\-queries on external tables in Redshift Spectrum\.
+ To improve the out\-of\-the\-box query performance, Amazon Redshift automatically chooses a single column primary key for specific tables as a distribution key\.