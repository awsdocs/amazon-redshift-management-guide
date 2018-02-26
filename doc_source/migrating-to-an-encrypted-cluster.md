# Migrating an Unencrypted Cluster to an Encrypted Cluster<a name="migrating-to-an-encrypted-cluster"></a>

You enable encryption when you launch a cluster\. To migrate from an unencrypted cluster to an encrypted cluster, you first unload your data from the existing, source cluster\. Then you reload the data in a new, target cluster with the chosen encryption setting\. For more information about launching an encrypted cluster, see [Amazon Redshift Database Encryption](working-with-db-encryption.md)\. During the migration process, your source cluster is available for read\-only queries until the last step\. The last step is to rename the target and source clusters, which switches endpoints so all traffic is routed to the new, target cluster\. The target cluster is unavailable until you reboot following the rename\. Suspend all data loads and other write operations on the source cluster while data is being transferred\. <a name="prepare-for-migration"></a>

**To prepare for migration**

1. Identify all the dependent systems that interact with Amazon Redshift, for example business intelligence \(BI\) tools and extract, transform, and load \(ETL\) systems\.

1. Identify validation queries to test the migration\. 

   For example, you can use the following query to find the number of user\-defined tables\.

   ```
   select count(*)
   from pg_table_def
   where schemaname != 'pg_catalog';
   ```

   The following query returns a list of all user\-defined tables and the number of rows in each table\.

   ```
   select "table", tbl_rows
   from svv_table_info;
   ```

1. Choose a good time for your migration\. To find a time when cluster usage is lowest, monitor cluster metrics such as CPU utilization and number of database connections\. For more information, see [Viewing Cluster Performance Data](performance-metrics-perf.md)\.

1. Drop unused tables\. 

   To create a list of tables and the number of the times each table has been queried, run the following query\. 

   ```
   select database,
   schema,
   table_id,
   "table",
   round(size::float/(1024*1024)::float,2) as size,
   sortkey1,
   nvl(s.num_qs,0) num_qs
   from svv_table_info t
   left join (select tbl,
   perm_table_name,
   count(distinct query) num_qs
   from stl_scan s
   where s.userid > 1
   and   s.perm_table_name not in ('internal worktable','s3')
   group by tbl,
   perm_table_name) s on s.tbl = t.table_id
   where t."schema" not in ('pg_internal');
   ```

1. Launch a new, encrypted cluster\. 

   Use the same port number for the target cluster as for the source cluster\. For more information about launching an encrypted cluster, see [Amazon Redshift Database Encryption](working-with-db-encryption.md)\. 

1. Set up the unload and load process\. 

   You can use the [Amazon Redshift Unload/Copy Utility](https://github.com/awslabs/amazon-redshift-utils/tree/master/src/UnloadCopyUtility) to help you to migrate data between clusters\. The utility exports data from the source cluster to a location on Amazon S3\. The data is encrypted with AWS KMS\. The utility then automatically imports the data into the target\. Optionally, you can use the utility to clean up Amazon S3 after migration is complete\. 

1. Run a test to verify your process and estimate how long write operations must be suspended\. 

   During the unload and load operations, maintain data consistency by suspending data loads and other write operations\. Using one of your largest tables, run through the unload and load process to help you estimate timing\. 

1. Create database objects, such as schemas, views, and tables\. To help you generate the necessary data definition language \(DDL\) statements, you can use the scripts in [AdminViews](https://github.com/awslabs/amazon-redshift-utils/tree/master/src/AdminViews) in the AWS GitHub repository\.<a name="migration-your-cluster"></a>

**To migrate your cluster:**

1. Stop all ETL processes on the source cluster\. 

   To confirm that there are no write operations in process, use the Amazon Redshift Management Console to monitor write IOPS\. For more information, see [Viewing Cluster Performance Data](performance-metrics-perf.md)\. 

1. Run the validation queries you identified earlier to collect information about the unencrypted source cluster before migration\.

1. \(Optional\) Create one workload management \(WLM\) queue to use the maximum available resources in both the source and target cluster\. For example, create a queue named `data_migrate` and configure the queue with memory of 95 percent and concurrency of 4\. For more information, see [Routing Queries to Queues Based on User Groups and Query Groups](http://docs.aws.amazon.com/redshift/latest/dg/tutorial-wlm-routing-queries-to-queues.html) in the *Amazon Redshift Database Developer Guide*\.

1. Using the `data_migrate` queue, run the UnloadCopyUtility\. 

   Monitor the UNLOAD and COPY process using the Amazon Redshift Console\. 

1. Run the validation queries again and verify that the results match the results from the source cluster\. 

1. Rename your source and target clusters to swap the endpoints\. For more information, see [Step 6: Rename the Source and Target Clusters](rs-tutorial-using-snapshot-restore-resize-operations.md#rs-tutorial-rename-clusters)\. To avoid disruption, perform this operation outside of business hours\.

1. Verify that you can connect to the target cluster using all of your SQL clients, such as ETL and reporting tools\.

1. Shut down the unencrypted source cluster\.