# Cluster versions for Amazon Redshift<a name="cluster-versions"></a>

Amazon Redshift regularly releases cluster versions\. Your Amazon Redshift clusters are patched during your system maintenance window\. The timing of the patch depends on your AWS Region and maintenance window settings\. You can view or change your maintenance window settings from the Amazon Redshift console\. For more information about maintenance, see [Cluster maintenance](working-with-clusters.md#rs-cluster-maintenance)\. 

You can view the cluster version of your cluster on the Amazon Redshift console on the **Maintenance** tab of the cluster details\. Or you can see the cluster version in the output of the SQL command:

```
SELECT version();
```

**Topics**
+ [Amazon Redshift patch 172](#cluster-version-172)
+ [Amazon Redshift patch 171](#cluster-version-171)
+ [Amazon Redshift patch 170](#cluster-version-170)
+ [Amazon Redshift patch 169](#cluster-version-169)
+ [Amazon Redshift patch 168](#cluster-version-168)

## Amazon Redshift patch 172<a name="cluster-version-172"></a>

Cluster versions in this patch: 
+ 1\.0\.44126 – Released on November 23, 2022
+ 1\.0\.43980 – Released on November 17, 2022

### New features and improvements in this patch<a name="cluster-version-2022-11-17-features"></a>
+ Adds support for row\-level security \(RLS\) on materialized views\.
+ Increases the S3 timeout to improve cross\-Region data sharing\.
+ Adds new spatial function `ST_GeomFromGeohash`\.
+ Improves automatic selection of distribution key from composite primary keys to improve out\-of\-the\-box performance\.
+ Adds automatic primary key to distribution key for tables with composite primary keys, improving out\-of\-the\-box performance\.
+ Improves concurrency scaling to allow more queries to scale even as data changes\.
+ Improves data sharing query performance\.
+ Adds Machine Learning probability metrics for classification models\.
+ Adds new system tables for monitoring: `SVV_USER_INFO`, `SVV_MV_INFO`, `SYS_CONNECTION_LOG`, `SYS_DATASHARE_USAGE_PRODUCER`, `SYS_DATASHARE_USAGE_CONSUMER`, and `SYS_DATASHARE_CHANGE_LOG`\.

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