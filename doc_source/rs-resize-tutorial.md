# Tutorial: Resizing Clusters in Amazon Redshift<a name="rs-resize-tutorial"></a>


+ [Overview](#rs-tutorial-resizing-clusters-overview)
+ [Resize Operation Overview](#rs-tutorial-resize-operation-overview)
+ [Snapshot, Restore, and Resize Operation Overview](#rs-tutorial-snapshot-restore-resize-overview)
+ [Tutorial: Using the Resize Operation to Resize a Cluster](rs-tutorial-using-the-resize-operation.md)
+ [Tutorial: Using the Snapshot, Restore, and Resize Operations to Resize a Cluster](rs-tutorial-using-snapshot-restore-resize-operations.md)

## Overview<a name="rs-tutorial-resizing-clusters-overview"></a>

 As your data warehousing capacity and performance needs change or grow, you can resize your cluster to make the best use of the computing and storage options that Amazon Redshift provides\. You can scale the cluster in or out by changing the number of nodes\. Or, you can scale the cluster up or down by specifying a different node type\. You can resize your cluster by using one of the following approaches: 

+ Use the resize operation with an existing cluster\.

+ Use the snapshot and restore operations to make a copy of an existing cluster\. Then, resize the new cluster\.

 Both the resize approach and the snapshot and restore approach copy user tables and data to the new cluster; they do not do anything with system tables and data\. If you have enabled audit logging in your source cluster, you’ll be able to continue to access the logs in Amazon Simple Storage Service \(Amazon S3\) even after you delete the source cluster\. You can keep or delete these logs as your data policies specify\.

## Resize Operation Overview<a name="rs-tutorial-resize-operation-overview"></a>

The resize operation is the preferred method to resize your cluster because it is the simplest method\. With the resize operation, your data is copied in parallel from the compute node or nodes in your source cluster to the compute node or nodes in the target cluster\. The time that it takes to resize depends on the amount of data and the number of nodes in the smaller cluster\. It can take anywhere from a couple of hours to a couple of days\. 

When you start the resize operation, Amazon Redshift puts the existing cluster into read\-only mode until the resize finishes\. During this time, you can only run queries that read from the database; you cannot run any queries that write to the database, including read\-write queries\. For more information, see [Write and read\-write operations](http://docs.aws.amazon.com/redshift/latest/dg/c_write_readwrite.html) in the *Amazon Redshift Database Developer Guide\. *

**Note**  
 If you would like to resize with minimal production impact, you can use the following section, [Snapshot, Restore, and Resize Operation Overview](#rs-tutorial-snapshot-restore-resize-overview), to create a copy of your cluster, resize the copy, and then switch the connection endpoint to the resized cluster when the resize is complete\. 

After Amazon Redshift puts the source cluster into read\-only mode, it provisions a new cluster, the target cluster, using the information that you specify for the node type, cluster type, and number of nodes\. Then, Amazon Redshift copies the data from the source cluster to the target cluster\. When this is complete, all connections switch to use the target cluster\. If you have any queries in progress at the time this switch happens, your connection will be lost and you must restart the query on the target cluster\. You can view the resize progress on the cluster's **Status** tab on the Amazon Redshift console\. 

Amazon Redshift does not sort tables during a resize operation, so the existing sort order is maintained\. When you resize a cluster, Amazon Redshift distributes the database tables to the new nodes based on their distribution styles and runs an ANALYZE command to update statistics\. Rows that are marked for deletion are not transferred, so you will only need to run a VACUUM command if your tables need to be resorted\. For more information, see [Vacuuming tables](http://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the* Amazon Redshift Database Developer Guide\. *

To walk through the process of resizing an Amazon Redshift cluster using the resize operation, see [Tutorial: Using the Resize Operation to Resize a Cluster](rs-tutorial-using-the-resize-operation.md)\.

## Snapshot, Restore, and Resize Operation Overview<a name="rs-tutorial-snapshot-restore-resize-overview"></a>

As described in the preceding section, the time it takes to resize a cluster with the resize operation depends heavily on the amount of data in the cluster\. Because you cannot perform write or read\-write operations in the database during the resize, you should determine whether you want to use the resize operation or an alternate method that reduces the amount of time that the cluster is in read\-only mode\. 

If you require near\-constant write access to your Amazon Redshift cluster, you can use the snapshot and restore operations described in the following section\. This approach requires that any data that is written to the source cluster after the snapshot is taken must be copied manually to the target cluster after the switch\. Depending on how long the copy takes, you might need to repeat this several times until you have the same data in both clusters and can make the switch to the target cluster\. This process might have a negative impact on existing queries until the full set of data is available in the target cluster, but it does minimize the amount of time that you cannot write to the database\. 

The snapshot, restore, and resize approach uses the following process: 

1. Take a snapshot of your existing cluster\. The existing cluster is the source cluster\. 

1. Make note of the time the snapshot was taken so that you can later identify the point at which you’ll need to rerun extract, transact, load \(ETL\) processes to load any post\-snapshot data into the target database\. 

1. Restore the snapshot into a new cluster\. This new cluster is the target cluster\. Verify that the sample data exists in the target cluster\. 

1. Resize the target cluster\. Select the new node type, number of nodes, and other settings for the target cluster\. 

1. Review the loads from your ETL processes that occurred after you took a snapshot of the source cluster\. You’ll need to reload the same data in the same order into the target cluster\. If you have ongoing data loads, you’ll need to repeat this process several times until the data is the same in both the source and target clusters\. 

1. Stop all queries running on the source cluster\. To do this, you can reboot the cluster, or you can log on as a super user and use the [PG\_CANCEL\_BACKEND](http://docs.aws.amazon.com/redshift/latest/dg/PG_CANCEL_BACKEND.html) and the [PG\_TERMINATE\_BACKEND](http://docs.aws.amazon.com/redshift/latest/dg/PG_TERMINATE_BACKEND.html) commands\. Rebooting the cluster is the easiest way to make sure that the cluster is unavailable\. 

1. Rename the source cluster\. For example, rename it from `examplecluster` to `examplecluster-source`\. 

1. Rename the target cluster to use the name of the source cluster prior to the rename\. For example, rename the target cluster from preceding to `examplecluster`\. From this point on, any applications that use the endpoint containing `examplecluster` will be connecting to the target cluster\. 

1. Delete the source cluster after you switch to the target cluster, and verify that all processes work as expected\. 

Alternatively, you can rename the source and target clusters before reloading data into the target cluster if you do not have a requirement that any dependent systems and reports be immediately up\-to\-date with those for the target cluster\. In this case, step 6 would be moved to the end of the process described preceding\. 

The rename process is only required if you want applications to continue using the same endpoint to connect to the cluster\. If you do not require this, you can instead update any applications that connect to the cluster to use the endpoint of the target cluster without renaming the cluster\. 

There are a couple of benefits to reusing a cluster name\. First, you do not need to update application connection strings because the endpoint does not change, even though the underlying cluster changes\. Second, related items such as Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) notifications are tied to the cluster name, so you can continue using the same alarms and notifications that you’ve set up for the cluster\. This continued use is primarily a concern in production environments where you want to have the flexibility to resize the cluster without having to reconfigure related items, such as alarms and notifications\. 

To walk through the process of resizing an Amazon Redshift cluster using the snapshot, restore, and resize operations, see [Tutorial: Using the Snapshot, Restore, and Resize Operations to Resize a Cluster](rs-tutorial-using-snapshot-restore-resize-operations.md)\.