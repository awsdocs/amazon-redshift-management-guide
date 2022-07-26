# Cluster versions for Amazon Redshift<a name="cluster-versions"></a>

Amazon Redshift regularly releases cluster versions\. Your Amazon Redshift clusters are patched during your system maintenance window\. The timing of the patch depends on your AWS Region and maintenance window settings\. You can view or change your maintenance window settings from the Amazon Redshift console\. For more information about maintenance, see [Cluster maintenance](working-with-clusters.md#rs-cluster-maintenance)\. 

You can view the cluster version of your cluster on the Amazon Redshift console on the **Maintenance** tab of the cluster details\. Or you can see the cluster version in the output of the SQL command:

```
SELECT version();
```

**Topics**
+ [Amazon Redshift patch 169](#cluster-version-169)
+ [Amazon Redshift patch 168](#cluster-version-168)

## Amazon Redshift patch 169<a name="cluster-version-169"></a>

Cluster versions in this patch: 
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