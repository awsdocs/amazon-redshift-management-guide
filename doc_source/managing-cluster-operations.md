# Overview of managing clusters in Amazon Redshift<a name="managing-cluster-operations"></a>

After your cluster is created, there are several operations you can perform on it\. The operations include resizing, pausing, resuming, renaming, and deleting\.

## Resizing clusters in Amazon Redshift<a name="rs-resize-tutorial"></a>

As your data warehousing capacity and performance needs change, you can resize your cluster to make the best use of Amazon Redshift's computing and storage options\. 

A resize operation comes in two types:
+ **Elastic resize** – You can add nodes to or remove nodes from your cluster\. You can also change the node type, such as from DS2 nodes to RA3 nodes\. Elastic resize is a fast operation, typically completing in minutes\. For this reason, we recommend it as a first option\. When you perform an elastic resize, it redistributes data slices, which are partitions that are allocated memory and disk space in each node\. Elastic resize is appropriate when you:
  + *Add or reduce nodes in an existing cluster, but you don't change the node type* – This is commonly called an *in\-place* resize\. When you perform this type of resize, some running queries complete successfully, but others can be dropped as part of the operation\. An elastic resize completes within a few minutes\.
  + *Change the node type for a cluster* – When you change the node type, a snapshot is created and data is redistributed from the source cluster to a cluster comprised of the new node type\. On completion, running queries are dropped\. Like the *in\-place* resize, it completes quickly\.
+ **Classic resize** – You can change the node type, number of nodes, or both, in a similar manner to elastic resize\. Classic resize takes more time to complete, but it can be useful in cases where the change in node count or the node type to migrate to doesn't fall within the bounds for elastic resize\. This can apply, for instance, when the change in node count is really large\. You can also use classic resize to change the cluster encryption\. For example, you can use it to modify your unencrypted cluster to use AWS KMS encryption\.

**Topics**
+ [Elastic resize](#elastic-resize)
+ [Classic resize](#classic-resize-faster)

### Elastic resize<a name="elastic-resize"></a>

An elastic resize operation, when you add or remove nodes of the same type, has the following stages:

1. Elastic resize takes a cluster snapshot\. This snapshot always includes [no\-backup tables](working-with-snapshots.md#snapshots-no-backup-tables) for nodes where it's applicable\. \(Some node types, like RA3, don't have no\-backup tables\.\) If your cluster doesn't have a recent snapshot, because you disabled automated snapshots, the backup operation can take longer\. \(To minimize the time before the resize operation begins, we recommend that you enable automated snapshots or create a manual snapshot before starting the resize\.\)  When you start an elastic resize and a snapshot operation is in progress, the resize can fail if the snapshot operation doesn't complete within a few minutes\. For more information, see [Amazon Redshift snapshots and backups](working-with-snapshots.md)\.

1. The operation migrates cluster metadata\. The cluster is unavailable for a few minutes\. The majority of queries are temporarily paused and connections are held open\. It is possible, however, for some queries to be dropped\. This stage is short\.

1. Session connections are reinstated and queries resume\. 

1. Elastic resize redistributes data to node slices, in the background\. The cluster is available for read and write operations, but some queries can take longer to run\.

1. After the operation completes, Amazon Redshift sends an event notification\.

When you use elastic resize to change the node type, it works similarly to when you add or subtract nodes of the same type\. First, a snapshot is created\. A new target cluster is provisioned with the latest data from the snapshot, and data is transferred to the new cluster in the background\. During this period, data is read only\. When the resize nears completion, Amazon Redshift updates the endpoint to point to the new cluster and all connections to the source cluster are dropped\.

If you have reserved nodes, for example DS2 reserved nodes, you can upgrade to RA3 reserved nodes when you perform a resize\. You can do this when you perform an elastic resize or use the console to restore from a snapshot\. The console guides you through this process\. For more information about upgrading to RA3 nodes, see [Upgrading to RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-upgrading-to-ra3)\. 

Elastic resize doesn't sort tables or reclaims disk space, so it isn't a substitute for a vacuum operation\.  For more information, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html)\.

 Elastic resize has the following constraints:
+ *Elastic resize and data sharing clusters* \- When you add or subtract nodes on a cluster that's a producer for data sharing, you can’t connect to it from consumers while Amazon Redshift migrates cluster metadata\. Similarly, if you perform an elastic resize and choose a new node type, data sharing is unavailable while connections are dropped and transferred to the new target cluster\. In both types of elastic resize, the producer is unavailable for several minutes\.
+ *Single\-node clusters* \- You can't use elastic resize to resize from or to a single\-node cluster\.
+ *Data transfer from a shared snapshot* \- To run an elastic resize on a cluster that is transferring data from a shared snapshot, at least one backup must be available for the cluster\. You can view your backups on the Amazon Redshift console snapshots list, the `describe-cluster-snapshots` CLI command, or the `DescribeClusterSnapshots` API operation\.
+ *Platform restriction* \- Elastic resize is available only for clusters that use the EC2\-VPC platform\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\. 
+ *Storage considerations* \- Make sure that your new node configuration has enough storage for existing data\. You may have to add additional nodes or change configuration\. 
+ *Source vs target cluster size* \- The number of nodes and node type that it's possible to resize to with elastic resize is determined by the number of nodes in the source cluster and the node type chosen for the resized cluster\. To determine the possible configurations available, you can use the console\. Or you can use the `describe-node-configuration-options` AWS CLI command with the `action-type resize-cluster` option\. For more information about the resizing using the Amazon Redshift console, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\. 

  The following example CLI command describes the configuration options available\. In this example, the cluster named `mycluster` is a `dc2.large` 8\-node cluster\.

  ```
  aws redshift describe-node-configuration-options --cluster-identifier mycluster --region eu-west-1 --action-type resize-cluster
  ```

  This command returns an option list with recommended node types, number of nodes, and disk utilization for each option\. The configurations returned can vary based on the specific input cluster\. You can choose one of the returned configurations when you specify the options of the `resize-cluster` CLI command\. 
+ *Ceiling on additional nodes* \- Elastic resize has limits on the nodes that you can add to a cluster\. For example, a dc2 cluster supports elastic resize up to double the number of nodes\. To illustrate, you can add a node to a 4\-node dc2\.8xlarge cluster to make it a five\-node cluster, or add more nodes until you reach eight\.

  With some ra3 node types, you can increase the number of nodes up to four times the existing count\. Specifically, suppose that your cluster consists of ra3\.4xlarge or ra3\.16xlarge nodes\. You can then use elastic resize to increase the number of nodes in an 8\-node cluster to 32\. Or you can pick a value below the limit\. \(Keep in mind that the ability to grow the cluster by 4x depends on the source cluster size\.\) If your cluster has ra3\.xlplus nodes, the limit is double\.

  All ra3 node types support a decrease in the number of nodes to a quarter of the existing count\. For example, you can decrease the size of a cluster with ra3\.4xlarge nodes from 12 nodes to 3, or to a number above the minimum\.

  The following table lists growth and reduction limits for each node type that supports elastic resize\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-operations.html)

### Classic resize<a name="classic-resize-faster"></a>

Classic resize handles use cases where the change in cluster size or node type isn't supported by elastic resize\. When you perform a classic resize, Amazon Redshift creates a target cluster and migrates your data and metadata to it from the source cluster\. 

#### Classic resize to RA3 can provide better availability<a name="classic-resize-improved"></a>

Classic resize has been enhanced when the target node type is RA3\. It does this by using a backup and restore operation between the source and target cluster\. When the resize begins, the source cluster restarts and is unavailable for a few minutes\. After that, the cluster is available for read and write operations while the resize continues in the background\.

##### Checking your cluster<a name="classic-resize-improved-considerations"></a>

To ensure you have the best performance and results when you perform a classic resize to an RA3 cluster, complete this checklist\.

1. The size of the data must be below 400TB\. To validate the size of your data, create a snapshot and check its size\. You can also run the following query to check the size: 

   ```
   SELECT
   sum(case when lower(diststyle) like ('%key%') then size else 0 end) distkey_blocks,
   sum(size) as total_blocks,
   ((distkey_blocks/(total_blocks*1.00)))*100 as Blocks_need_redist
   FROM svv_table_info;
   ```

   The `svv_table_info` table is visible only to superusers\.

1. The node type of the target cluster must be RA3, and the target cluster must have at least two nodes\. Classic resize to a single\-node cluster takes much longer, even if the target node type is RA3\.

1. Before you initiate a classic resize, make sure you have a manual snapshot that is no more than 10 hours old\. If not, take a snapshot\.

1. The snapshot used to perform the classic resize can't be used for a table restore or other purpose\.

1. The cluster must be in a VPC\.

##### Sorting and distribution operations that result from classic resize to RA3<a name="classic-resize-effects"></a>

During classic resize to RA3, Distribution Key tables migrated as Distribution Even are converted back to their original distribution style, using operations in the background\. The duration of this is dependent on the size of the data and how busy your cluster is\. Query workloads are given higher priority to run over data migration\. For more information, see [Distribution styles](https://docs.aws.amazon.com/redshift/latest/dg/c_choosing_dist_sort.html)\. Both reads and writes to the database work during this migration process, but there can be degradation in query performance\. However, concurrency scaling can boost performance by adding resources for query workloads\. You can see the progress of data migration by using results from the **svl\_restore\_alter\_table\_progress** view\. More information about monitoring follows\.

After the cluster is fully resized, the following sort behavior occurs:
+ If the resize results in the cluster having more slices, KEY distribution tables become partially unsorted, but EVEN tables remain sorted\. Additionally, the information about how much data is sorted may not be up to date, directly following the resize\. After key recovery, automatic vacuum sorts the table over time\.
+ If the resize results in the cluster having fewer slices, both KEY distribution and EVEN distribution tables become partially unsorted\. Automatic vacuum sorts the table over time\.

For more information about automatic table vacuum, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html)\. For more information about slices in compute nodes, see [Data warehouse system architecture](https://docs.aws.amazon.com/redshift/latest/dg/c_high_level_system_architecture.html)\.

##### Classic resize steps when the target cluster is RA3<a name="classic-resize-stages-ra3"></a>

Classic resize consists of the following steps, when the target cluster type is RA3 and you've met the prerequisites detailed in the previous section\.

1. Migration initiates from the source cluster to the target cluster\. When the new, target cluster is provisioned, Amazon Redshift sends an event notification that the resize has started\. It restarts your existing cluster, which closes all connections\. If your existing cluster is a datasharing producer cluster, connections with consumer clusters are also closed\. The restart takes a few minutes\. 

   Note that any database relation, such as a table or materialized view, created with `BACKUP NO` is not retained during the classic resize\. For more information, see [CREATE MATERIALIZED VIEW](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html)\.

1. After the restart, the database is available for reads and writes\. Additionally, data sharing resumes, which takes an additional few minutes\.

1. Data is migrated to the target cluster\. When the target node type is RA3, reads and writes are available during data migration\.

1. When the resize process nears completion, Amazon Redshift updates the endpoint to the target cluster, and all connections to the source cluster are dropped\. The target cluster becomes the producer for data sharing\.

1. The resize completes\. Amazon Redshift sends an event notification\.

You can view the resize progress on the Amazon Redshift console\. The time it takes to resize a cluster depends on the amount of data\. 

##### Encrypting a cluster using classic resize to RA3<a name="resize-classic-encryption"></a>

 When you encrypt a cluster with RA3 nodes, so the cluster uses AWS KMS encryption, for instance, it completes the process with better availability\. All the steps are the same as a classic resize\. The cluster is encrypted in the first phase\.  In the same manner as a traditional classic resize, you can experience degradation in query performance during the process\. This is because queries share resources with migration tasks\. The time the resize takes depends on the amount of data\. 

When you perform cluster encryption, the initial cluster backup isn't created until the cluster is fully encrypted\. The amount of time this takes can vary\. It can take hours for a large amount of data\. Additionally, because a backup/restore operation occurs during the encryption process, any tables or materialized views created with `BACKUP NO` aren't retained\. For more information, see [CREATE TABLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_TABLE_NEW.html) or [CREATE MATERIALIZED VIEW](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html)\.

##### Monitoring a classic resize when the target cluster is RA3<a name="resize-monitoring"></a>

To monitor a classic resize of a provisioned cluster in progress, including KEY distribution, use **svl\_restore\_alter\_table\_progress**\. It shows the percentage completed for the table being converted\. You must be a super user to access the data\. The following shows a sample query:

```
select * from svl_restore_alter_table_progress;
```

The result is the following:

```
  tbl   | progress |                          message                          
--------+----------+-----------------------------------------------------------
 105614 | ABORTED  | Abort:Table no longer contains the prior dist key column.
 105610 | ABORTED  | Abort:Table no longer contains the prior dist key column.
 105594 | 0.00%    | Table waiting for alter diststyle conversion.
 105602 | ABORTED  | Abort:Table no longer contains the prior dist key column.
 105606 | ABORTED  | Abort:Table no longer contains the prior dist key column.
 105598 | 100.00%  | Restored to distkey successfully.
```

Drop tables that you don't need when you perform a classic resize\. When you do this, existing tables can be distributed more quickly\.

#### Classic resize steps when the target cluster isn't RA3<a name="classic-resize-stages"></a>

Classic resize consists of the following, when the target node type is anything other than RA3, like DS2, for instance\.

1. Migration initiates from the source cluster to the target cluster\. When the new, target cluster is provisioned, Amazon Redshift sends an event notification that the resize has started\. It restarts your existing cluster, which closes all connections\. If your existing cluster is a datasharing producer cluster, connections with consumer clusters are also closed\. The restart takes a few minutes\.

   Note that any database relation, such as a table or materialized view, created with `BACKUP NO` is not retained during the classic resize\. For more information, see [CREATE MATERIALIZED VIEW](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html)\.

1. Following the restart, the database is available as read only\. Data sharing resumes, which takes an additional few minutes\.

1. Data is migrated to the target cluster\. The database remains read only\.

1. When the resize process nears completion, Amazon Redshift updates the endpoint to the target cluster, and all connections to the source cluster are dropped\. The target cluster becomes the producer for data sharing\.

1. The resize completes\. Amazon Redshift sends an event notification\.

You can view the resize progress on the Amazon Redshift console\. The time it takes to resize a cluster depends on the amount of data\. 

**Note**  
It can take days or possibly weeks to resize a cluster with a large amount of data when the target cluster isn't RA3, or it doesn't meet the prerequisites for an RA3 target cluster detailed in the previous section\.

#### Elastic resize vs classic resize<a name="classic-resize-vs-classic-resize"></a>

The following table compares behavior between the two resize types\.


**Elastic resize vs classic resize**  

| Behavior | Elastic resize | Classic resize | Comments | 
| --- | --- | --- | --- | 
| System data retention | Elastic resize retains system log data\. | Classic resize doesn't retain system tables and data\. | If you have audit logging enabled in your source cluster, you can continue to access the logs in Amazon S3 or in CloudWatch, following a resize\. You can keep or delete these logs as your data policies specify\. | 
| Changing node types | Elastic resize, when the node type doesn't change: In\-place resize, and most queries are held\. Elastic resize, with a new node type selected: A new cluster is created\. Queries are dropped as the resize process completes\. | Classic Resize: A new cluster is created\. Queries are dropped during the resize process\. |  | 
| Session and query retention | Elastic resize retains sessions and queries when the node type is the same in the source cluster and target\. If you choose a new node type, queries are dropped\. | Classic resize doesn't retain sessions and queries\. Queries are dropped\. | When queries are dropped, you can expect some performance degradation\. It's best to perform a resize operation during a period of light use\. | 
| Cancelling a resize operation | You can't cancel an elastic resize\. | You can cancel a classic resize operation before it completes by choosing **Cancel resize** from the cluster details in the Amazon Redshift console\.  | The amount of time it takes to cancel a resize depends on the stage of the resize operation when you cancel\. When you do this, the cluster isn't available until the cancel operation completes\. If the resize operation is in the final stage, you can't cancel\. For classic resize to an RA3 cluster, you can't cancel\. | 

#### Scheduling a resize<a name="rs-restore-resize-overview-schedule"></a>

You can schedule resize operations for your cluster to scale up to anticipate high use or to scale down for cost savings\. Scheduling works for both elastic resize and classic resize\. You can set up a schedule on the Amazon Redshift console\. For more information, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster), under **Managing clusters using the console**\. You can also use AWS CLI or Amazon Redshift API operations to schedule a resize\. For more information, see [create\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-scheduled-action.html) in the *AWS CLI Command Reference* or [CreateScheduledAction](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateScheduledAction.html) in the *Amazon Redshift API Reference*\.

#### Snapshot, restore, and resize<a name="rs-tutorial-snapshot-restore-resize-overview"></a>

[Elastic resize](#elastic-resize) is the fastest method to resize an Amazon Redshift cluster\. If elastic resize isn't an option for you and you require near\-constant write access to your cluster, use the snapshot and restore operations with classic resize as described in the following section\. This approach requires that any data that is written to the source cluster after the snapshot is taken must be copied manually to the target cluster after the switch\. Depending on how long the copy takes, you might need to repeat this several times until you have the same data in both clusters\. Then you can make the switch to the target cluster\. This process might have a negative impact on existing queries until the full set of data is available in the target cluster\. However, it minimizes the amount of time that you can't write to the database\. 

The snapshot, restore, and classic resize approach uses the following process: 

1. Take a snapshot of your existing cluster\. The existing cluster is the source cluster\. 

1. Note the time that the snapshot was taken\. Doing this means that you can later identify the point when you need to rerun extract, transact, load \(ETL\) processes to load any post\-snapshot data into the target database\. 

1. Restore the snapshot into a new cluster\. This new cluster is the target cluster\. Verify that the sample data exists in the target cluster\. 

1. Resize the target cluster\. Choose the new node type, number of nodes, and other settings for the target cluster\. 

1. Review the loads from your ETL processes that occurred after you took a snapshot of the source cluster\. Be sure to reload the same data in the same order into the target cluster\. If you have ongoing data loads, repeat this process several times until the data is the same in both the source and target clusters\. 

1. Stop all queries running on the source cluster\. To do this, you can reboot the cluster, or you can log on as a superuser and use the [PG\_CANCEL\_BACKEND](https://docs.aws.amazon.com/redshift/latest/dg/PG_CANCEL_BACKEND.html) and the [PG\_TERMINATE\_BACKEND](https://docs.aws.amazon.com/redshift/latest/dg/PG_TERMINATE_BACKEND.html) commands\. Rebooting the cluster is the easiest way to make sure that the cluster is unavailable\. 

1. Rename the source cluster\. For example, rename it from `examplecluster` to `examplecluster-source`\. 

1. Rename the target cluster to use the name of the source cluster before the rename\. For example, rename the target cluster from preceding to `examplecluster`\. From this point on, any applications that use the endpoint containing `examplecluster` connect to the target cluster\. 

1. Delete the source cluster after you switch to the target cluster, and verify that all processes work as expected\. 

Alternatively, you can rename the source and target clusters before reloading data into the target cluster\. This approach works if you don't require that any dependent systems and reports be immediately up to date with those for the target cluster\. In this case, step 6 moves to the end of the process described preceding\. 

The rename process is only required if you want applications to continue using the same endpoint to connect to the cluster\. If you don't require this, you can instead update any applications that connect to the cluster to use the endpoint of the target cluster without renaming the cluster\. 

There are a couple of benefits to reusing a cluster name\. First, you don't need to update application connection strings because the endpoint doesn't change, even though the underlying cluster changes\. Second, related items such as Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) notifications are tied to the cluster name\. This tie means that you can continue using the same alarms and notifications that you set up for the cluster\. This continued use is primarily a concern in production environments where you want the flexibility to resize the cluster without reconfiguring related items, such as alarms and notifications\. 

#### Getting the leader node IP address<a name="cluster-resize-intro"></a>

If your cluster is public and is in a VPC, it keeps the same Elastic IP address \(EIP\) for the leader node after resizing\. If your cluster is private and is in a VPC, it keeps the same private IP address for the leader node after resizing\. If your cluster isn't in a VPC, a new public IP address is assigned for the leader node as part of the resize operation\.

To get the leader node IP address for a cluster, use the dig utility, as shown following\.

```
dig mycluster.abcd1234.us-west-2.redshift.amazonaws.com
```

The leader node IP address is at the end of the ANSWER SECTION in the results, as shown following\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/dig_IP_address.png)

## Pausing and resuming clusters<a name="rs-mgmt-pause-resume-cluster"></a>

If you have a cluster that only needs to be available at specific times, you can pause the cluster and later resume it\. While the cluster is paused, on\-demand billing is suspended\. Only the cluster's storage incurs charges\. For more information about pricing, see the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 

When you pause a cluster, Amazon Redshift creates a snapshot, begins terminating queries, and puts the cluster in a pausing state\. If you delete a paused cluster without requesting a final snapshot, then you can't restore the cluster\. You can't cancel or roll back a pause or resume operation after it's initiated\. 

You can pause and resume a cluster on the Amazon Redshift console, with the AWS CLI, or with Amazon Redshift API operations\. 

You can schedule actions to pause and resume a cluster\. When you use the new Amazon Redshift console to create a recurring schedule to pause and resume, then two scheduled actions are created for the date range that you choose\. The scheduled action names are suffixed with `-pause` and `-resume`\. The total length of the name must fit within the maximum size of a scheduled action name\. 

You can't pause the following types of clusters: 
+ EC2\-Classic clusters\. 
+ Clusters that are not active, for example, a cluster that is currently modifying\. 
+ Hardware security module \(HSM\) clusters\. 
+ Clusters that have automated snapshots turned off\. 

When deciding to pause a cluster, consider the following: 
+ Connections or queries to the cluster aren't available\. 
+ You can't see query monitoring information of a paused cluster on the Amazon Redshift console\. 
+ You can't modify a paused cluster\. Any scheduled actions on the cluster aren't done\. These include creating snapshots, resizing clusters, and cluster maintenance operations\. 
+ Hardware metrics aren't created\. Update your CloudWatch alarms if you have alarms set on missing metrics\. 
+ You can't copy the latest automated snapshots of a paused cluster to manual snapshots\. 
+ While a cluster is pausing, it can't be resumed until the pause operation is complete\. 
+ When you pause a cluster, billing is suspended\. However, the pause operation typically completes within 15 minutes, depending upon the size of the cluster\. 
+ Audit logs are archived and not restored on resume\. 
+ After a cluster is paused, traces and logs might not be available for troubleshooting problems that occurred before the pause\. 
+ No\-backup tables on the cluster are not restored on resume\. For more information about no\-backup tables, see [Excluding tables from snapshots](working-with-snapshots.md#snapshots-no-backup-tables)\.

When you resume a cluster, consider the following: 
+ The cluster version of the resumed cluster is updated to the maintenance version based on the maintenance window of the cluster\. 
+ If you delete the subnet associated with a paused cluster, you might have an incompatible network\. In this case, restore your cluster from the latest snapshot\. 
+ If you delete an Elastic IP address while the cluster is paused, then a new Elastic IP address is requested\. 
+ If Amazon Redshift can't resume the cluster with its previous elastic network interface, then Amazon Redshift tries to allocate a new one\. 
+ When you resume a cluster, your node IP addresses might change\. You might need to update your VPC settings to support these new IP addresses for features like COPY from Secure Shell \(SSH\) or COPY from Amazon EMR\.
+ If you try to resume a cluster that isn't paused, the resume operation returns an error\. If the resume operation is part of a scheduled action, modify or delete the scheduled action to prevent future errors\. 
+ Depending upon the size of the cluster, it can take several minutes to resume a cluster before queries can be processed\. In addition, query performance can be impacted for some period of time while the cluster is being re\-hydrated after resume completes\. 

## Renaming clusters<a name="rs-mgmt-rename-cluster"></a>

You can rename a cluster if you want the cluster to use a different name\. Because the endpoint to your cluster includes the cluster name \(also referred to as the *cluster identifier*\), the endpoint changes to use the new name after the rename finishes\. For example, if you have a cluster named `examplecluster` and rename it to `newcluster`, the endpoint changes to use the `newcluster` identifier\. Any applications that connect to the cluster must be updated with the new endpoint\. 

You may rename a cluster if you want to change the cluster your applications connect to without having to change the endpoint in those applications\. In this case, you must first rename the original cluster and then change the second cluster to reuse the name of the original cluster before the rename\. Doing this is necessary because the cluster identifier must be unique within your account and region, so the original cluster and second cluster cannot have the same name\. You might do this if you restore a cluster from a snapshot and don't want to change the connection properties of any dependent applications\. 

**Note**  
 If you delete the original cluster, you are responsible for deleting any unwanted cluster snapshots\. 

When you rename a cluster, the cluster status changes to `renaming` until the process finishes\. The old DNS name that was used by the cluster is immediately deleted, although it could remain cached for a few minutes\. The new DNS name for the renamed cluster becomes effective within about 10 minutes\. The renamed cluster is not available until the new name becomes effective\. The cluster will be rebooted and any existing connections to the cluster will be dropped\. After this completes, the endpoint will change to use the new name\. For this reason, you should stop queries from running before you start the rename and restart them after the rename finishes\. 

 Cluster snapshots are retained, and all snapshots associated with a cluster remain associated with that cluster after it is renamed\. For example, suppose that you have a cluster that serves your production database and the cluster has several snapshots\. If you rename the cluster and then replace it in the production environment with a snapshot, the cluster that you renamed still has those existing snapshots associated with it\. 

 Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) event notifications are associated with the name of the cluster\. If you rename the cluster, you must update these accordingly\. You can update the CloudWatch alarms in the CloudWatch console, and you can update the Amazon SNS event notifications in the Amazon Redshift console on the **Events** pane\. The load and query data for the cluster continues to display data from before the rename and after the rename\. However, performance data is reset after the rename process finishes\. 

For more information, see [Modifying a cluster](managing-clusters-console.md#modify-cluster)\.

## Shutting down and deleting clusters<a name="rs-mgmt-shutdown-delete-cluster"></a>

You can shut down your cluster if you want to stop it from running and incurring charges\. When you shut it down, you can optionally create a final snapshot\. If you create a final snapshot, Amazon Redshift will create a manual snapshot of your cluster before shutting it down\. You can later restore that snapshot if you want to resume running the cluster and querying data\. 

If you no longer need your cluster and its data, you can shut it down without creating a final snapshot\. In this case, the cluster and data are deleted permanently\. For more information about shutting down and deleting clusters, see [Deleting a cluster](managing-clusters-console.md#delete-cluster)\. 

Regardless of whether you shut down your cluster with a final manual snapshot, all automated snapshots associated with the cluster will be deleted after the cluster is shut down\. Any manual snapshots associated with the cluster are retained\. Any manual snapshots that are retained, including the optional final snapshot, are charged at the Amazon Simple Storage Service storage rate if you have no other clusters running when you shut down the cluster, or if you exceed the available free storage that is provided for your running Amazon Redshift clusters\. For more information about snapshot storage charges, see the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 