# Resizing Clusters in Amazon Redshift<a name="rs-resize-tutorial"></a>

As your data warehousing capacity and performance needs change or grow, you can resize your cluster to make the best use of the computing and storage options that Amazon Redshift provides\. You can use elastic resize to scale your cluster by changing the number of nodes\. Or, you can use classic resize to scale the cluster by specifying a different node type\. You can resize your cluster by using one of the following approaches: 
+ **Elastic resize** – To quickly add or remove nodes from a cluster, use elastic resize\. The cluster is unavailable briefly, usually only a few minutes\. Amazon Redshift tries to hold connections open and queries are paused temporarily\.
+ **Classic resize** – To change the node type, the number of nodes, or both, use classic resize\. Your cluster is put into a read\-only state for the duration of the classic resize operation, which might take anywhere from a couple of hours to a couple of days\.
+ **Snapshot, restore, and resize** – To keep your cluster available during a classic resize, first make a copy of an existing cluster, then resize the new cluster\. 

**Topics**
+ [Elastic Resize](#elastic-resize)
+ [Classic Resize](#classic-resize)
+ [Snapshot, Restore, and Resize](#rs-tutorial-snapshot-restore-resize-overview)
+ [Resizing a Cluster Using the Console](#rs-tutorial-using-the-resize-operation)
+ [Resizing a Cluster Using the CLI and API](#rs-tutorial-resize-cluster-cli-api)

## Elastic Resize<a name="elastic-resize"></a>

Elastic resize is the fastest method to resize a cluster\. Elastic resize adds or removes nodes on an existing cluster, then automatically redistributes the data to the new nodes\. Because it doesn't create a new cluster, the elastic resize operation completes quickly, usually in a few minutes\. You might notice a slight increase in execution time for some queries while the data is redistributed in the background\.

An elastic resize operation occurs in the following stages:

1. Elastic resize takes a cluster snapshot\. 

   The snapshot that elastic resize creates includes [no\-backup tables](working-with-snapshots.md#snapshots-no-backup-tables)\. If your cluster doesn't have a recent snapshot because you disabled automated snapshots, the backup operation takes longer\. To minimize the time before the resize operation begins, we recommend that you enable automated snapshots or create a manual snapshot before starting an elastic resize\. For more information, see [Amazon Redshift Snapshots](working-with-snapshots.md)\. 

1. The cluster is temporarily unavailable while elastic resize migrates cluster metadata\. 

   This stage is very short, just a few minutes at most\. Amazon Redshift holds session connections and queries remain queued\. Some sessions and queries might time out\. 

1. Session connections are reinstated and queries resume\. 

1. Elastic resize redistributes data to the node slices in the background\. 

   The cluster is available for read and write operations, but some queries might take longer to execute\. 

To monitor the progress of an elastic resize operation using the Amazon Redshift console, choose the **Status** tab on the cluster details page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status-example.png)

You can't use elastic resize on single\-node clusters or change the node type\. In such cases, you can use classic resize\. 

Elastic resize doesn't sort tables or reclaim disk space, so it isn't a substitute for a vacuum operation\. A classic resize copies tables to a new cluster, so it can reduce the need to vacuum\. For more information, see [Vacuuming Tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html)\.

Elastic resize has the following constraints: 
+ Elastic resize is available only for clusters that use the EC2\-VPC platform\. For more information, see [Supported Platforms to Launch Your Cluster](working-with-clusters.md#cluster-platforms)
+ The new node configuration must have enough storage for existing data\. Even when you add nodes, your new configuration might not have enough storage because of the way that data is redistributed\. 
+ For dc2\.large or ds2\.xlarge node types, you can double the size or half the size of the number of nodes of the original cluster\. For example, a 4\-node cluster can increase to eight nodes or decrease to two nodes with elastic resize\. To resize to a different number of nodes, you must use [Classic Resize](#classic-resize)\.
+ For dc2\.8xlarge or ds2\.8xlarge node types, you can change the number of nodes to half the current number to double the current number of nodes\. For example, a 4\-node cluster can be resized to 2, 3, 5, 6, 7, or 8 nodes\.

**Note**  
Use [Classic Resize](#classic-resize) to reset the maximum node limit\. For example, to increase from 4 nodes to 10 nodes, first change to 5 nodes using classic resize\. 

## Classic Resize<a name="classic-resize"></a>

With the classic resize operation, your data is copied in parallel from the compute node or nodes in your source cluster to the compute node or nodes in the target cluster\. The time that it takes to resize depends on the amount of data and the number of nodes in the smaller cluster\. It can take anywhere from a couple of hours to a couple of days\. 

When you start the resize operation, Amazon Redshift puts the existing cluster into read\-only mode until the resize finishes\. During this time, you can only run queries that read from the database; you cannot run any queries that write to the database, including read\-write queries\. For more information, see [Write and read\-write operations](https://docs.aws.amazon.com/redshift/latest/dg/c_write_readwrite.html) in the *Amazon Redshift Database Developer Guide\. *

**Note**  
To resize with minimal production impact, you can use steps in the following section, [Snapshot, Restore, and Resize](#rs-tutorial-snapshot-restore-resize-overview)\. You can use these steps to create a copy of your cluster, resize the copy, and then switch the connection endpoint to the resized cluster when the resize is complete\. 

Both the classic resize approach and the snapshot and restore approach copy user tables and data to the new cluster; they don't retain system tables and data\. With either classic resize or snapshot and restore, if you have enabled audit logging in your source cluster, you can continue to access the logs in Amazon S3\. With these approaches, you can still access the logs after you delete the source cluster\. You can keep or delete these logs as your data policies specify\. Elastic resize retains the system log tables\. 

After Amazon Redshift puts the source cluster into read\-only mode, it provisions a new cluster, the target cluster\. It does so using the information that you specify for the node type, cluster type, and number of nodes\. Then Amazon Redshift copies the data from the source cluster to the target cluster\. When this is complete, all connections switch to use the target cluster\. If you have any queries in progress at the time this switch happens, your connection is lost and you must restart the query on the target cluster\. You can view the resize progress on the cluster's **Status** tab on the Amazon Redshift console\. 

Amazon Redshift doesn't sort tables during a resize operation, so the existing sort order is maintained\. When you resize a cluster, Amazon Redshift distributes the database tables to the new nodes based on their distribution styles and runs an ANALYZE command to update statistics\. Rows that are marked for deletion aren't transferred, so you need to run only a VACUUM command if your tables need to be resorted\. For more information, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the* Amazon Redshift Database Developer Guide\. *

You can cancel a resize operation before it completes by choosing **cancel resize** from the cluster list in the Amazon Redshift console\. The amount of time it takes to cancel a resize depends on the stage of the resize operation when you cancel\. The cluster isn't available until the cancel resize operation completes\. If the resize operation is in the final stage, you can't cancel the operation\.

For more information, see [Resizing a Cluster Using the Console](#rs-tutorial-using-the-resize-operation)\.

## Snapshot, Restore, and Resize<a name="rs-tutorial-snapshot-restore-resize-overview"></a>

As described in the preceding section, the time it takes to resize a cluster with the classic resize operation depends heavily on the amount of data in the cluster\. 

[Elastic resize](#elastic-resize) is the fastest method to resize an Amazon Redshift cluster\. If elastic resize isn't an option for you and you require near\-constant write access to your cluster, use the snapshot and restore operations described in the following section\. This approach requires that any data that is written to the source cluster after the snapshot is taken must be copied manually to the target cluster after the switch\. Depending on how long the copy takes, you might need to repeat this several times until you have the same data in both clusters\. Then you can make the switch to the target cluster\. This process might have a negative impact on existing queries until the full set of data is available in the target cluster\. However, it minimizes the amount of time that you can't write to the database\. 

The snapshot, restore, and resize approach uses the following process: 

1. Take a snapshot of your existing cluster\. The existing cluster is the source cluster\. 

1. Note the time that the snapshot was taken\. Doing this means that you can later identify the point when you need to rerun extract, transact, load \(ETL\) processes to load any post\-snapshot data into the target database\. 

1. Restore the snapshot into a new cluster\. This new cluster is the target cluster\. Verify that the sample data exists in the target cluster\. 

1. Resize the target cluster\. Choose the new node type, number of nodes, and other settings for the target cluster\. 

1. Review the loads from your ETL processes that occurred after you took a snapshot of the source cluster\. Be sure to reload the same data in the same order into the target cluster\. If you have ongoing data loads, repeat this process several times until the data is the same in both the source and target clusters\. 

1. Stop all queries running on the source cluster\. To do this, you can reboot the cluster, or you can log on as a super user and use the [PG\_CANCEL\_BACKEND](https://docs.aws.amazon.com/redshift/latest/dg/PG_CANCEL_BACKEND.html) and the [PG\_TERMINATE\_BACKEND](https://docs.aws.amazon.com/redshift/latest/dg/PG_TERMINATE_BACKEND.html) commands\. Rebooting the cluster is the easiest way to make sure that the cluster is unavailable\. 

1. Rename the source cluster\. For example, rename it from `examplecluster` to `examplecluster-source`\. 

1. Rename the target cluster to use the name of the source cluster before the rename\. For example, rename the target cluster from preceding to `examplecluster`\. From this point on, any applications that use the endpoint containing `examplecluster` connect to the target cluster\. 

1. Delete the source cluster after you switch to the target cluster, and verify that all processes work as expected\. 

Alternatively, you can rename the source and target clusters before reloading data into the target cluster\. This approach works if you don't have a requirement that any dependent systems and reports be immediately up\-to\-date with those for the target cluster\. In this case, step 6 moves to the end of the process described preceding\. 

The rename process is only required if you want applications to continue using the same endpoint to connect to the cluster\. If you don't require this, you can instead update any applications that connect to the cluster to use the endpoint of the target cluster without renaming the cluster\. 

There are a couple of benefits to reusing a cluster name\. First, you don't need to update application connection strings because the endpoint doesn't change, even though the underlying cluster changes\. Second, related items such as Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) notifications are tied to the cluster name\. This tie means that you can continue using the same alarms and notifications that you set up for the cluster\. This continued use is primarily a concern in production environments where you want to have the flexibility to resize the cluster without having to reconfigure related items, such as alarms and notifications\. 

## Resizing a Cluster Using the Console<a name="rs-tutorial-using-the-resize-operation"></a>

You can resize a cluster using either elastic resize or classic resize\. For more information about the types of resize, see [Resizing Clusters in Amazon Redshift](#rs-resize-tutorial)\.<a name="rs-tutorial-resize-cluster-console-proc"></a>

**To resize a cluster using the console:**

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, choose **Clusters**, and then choose the cluster to resize\. 

1. Choose **Cluster** and then choose **Resize**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1. For **Type of resize**, choose either of the following values:
   + **Elastic resize** 

     If your cluster configuration doesn't support elastic resize, **Elastic resize** isn't available\. 
   + **Classic resize** 

1. For **Number of Nodes**, choose the new node count\. For elastic resize, only valid node counts are available\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-resize.png)

1.  Choose **Resize**\. 

1. Monitor **Cluster Status ** to see the resize progress\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status-example.png)

## Resizing a Cluster Using the CLI and API<a name="rs-tutorial-resize-cluster-cli-api"></a>

To resize a cluster using the CLI, run the [resize\-cluster](https://docs.aws.amazon.com/cli/latest/reference/redshift/resize-cluster.html) command\. 

To resize a cluster using the API, use the [ResizeCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ResizeCluster.html) action\. 