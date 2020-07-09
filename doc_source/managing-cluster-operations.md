# Overview of managing clusters in Amazon Redshift<a name="managing-cluster-operations"></a>

After your cluster is created, there are several operations you can perform on it\. The operations include resizing, pausing, resuming, renaming, and deleting\. 

## Resizing clusters in Amazon Redshift<a name="rs-resize-tutorial"></a>

As your data warehousing capacity and performance needs change or grow, you can resize your cluster to make the best use of the computing and storage options that Amazon Redshift provides\. You can use elastic resize to scale your cluster by changing the node type and number of nodes\. Or, if your new node configuration is not available through elastic resize, you can use classic resize\. 

To resize your cluster, use one of the following approaches: 
+ **Elastic resize** – Use elastic resize to change the node type, number of nodes, or both\. If you only change the number of nodes, then queries are temporarily paused and connections are held open if possible\. During the resize operation, the cluster is read\-only\. Typically, elastic resize takes 10–15 minutes\. We recommend using elastic resize when possible\. 
+ **Classic resize** – Use classic resize to change the node type, number of nodes, or both\. Choose this option when you are resizing to a configuration that isn't available through elastic resize\. An example is to or from a single\-node cluster\. During the resize operation, the cluster is read\-only\. Typically, classic resize takes 2 hours–2 days or longer, depending on your data's size\. 
+ **Snapshot and restore with classic resize** – To keep your cluster available during a classic resize, you can first make a copy of an existing cluster, then resize the new cluster\. 

You can resize \(both elastic resize and classic resize\) your cluster on a schedule\. When you use the new Amazon Redshift console, you can set up a schedule to resize your cluster\. For more information, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\. You can also use the AWS CLI or Amazon Redshift API operations to schedule a resize\. For more information, see [create\-scheduled\-action](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-scheduled-action.html) in the *AWS CLI Command Reference* or [CreateScheduledAction](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateScheduledAction.html) in the *Amazon Redshift API Reference*\. 

**Topics**
+ [Elastic resize](#elastic-resize)
+ [Classic resize](#classic-resize)
+ [Snapshot, restore, and resize](#rs-tutorial-snapshot-restore-resize-overview)
+ [Details of resizing a cluster](#cluster-resize-intro)

### Elastic resize<a name="elastic-resize"></a>

Elastic resize is the fastest method to resize a cluster\. You can use elastic resize to add or remove nodes and change node types for an existing cluster\. 

When a cluster is resized using elastic resize with the same node type, it automatically redistributes the data to the new nodes\. Because it doesn't create a new cluster in this scenario, the elastic resize operation completes quickly, usually in a few minutes\. You might notice a slight increase in execution time for some queries while the data is redistributed in the background\. An elastic resize operation occurs in the following stages: 

1. Elastic resize takes a cluster snapshot\. 

   The snapshot that elastic resize creates includes [no\-backup tables](working-with-snapshots.md#snapshots-no-backup-tables)\. If your cluster doesn't have a recent snapshot because you disabled automated snapshots, the backup operation takes longer\. To minimize the time before the resize operation begins, we recommend that you enable automated snapshots or create a manual snapshot before starting an elastic resize\.  When you start an elastic resize and a snapshot operation is currently in progress, then elastic resize might fail if the snapshot operation doesn't complete within a few minutes\. For more information, see [Amazon Redshift snapshots](working-with-snapshots.md)\. 

1. The cluster is temporarily unavailable while elastic resize migrates cluster metadata\. 

   This stage is very short, just a few minutes at most\. Amazon Redshift holds session connections and queries remain queued\. Some sessions and queries might time out\. 

1. Session connections are reinstated and queries resume\. 

1. Elastic resize redistributes data to the node slices in the background\. 

   The cluster is available for read and write operations, but some queries might take longer to execute\. 

When a cluster is resized using elastic resize to change the node type, a snapshot is created\. A new cluster is provisioned for you with the latest data from the snapshot\. The cluster is temporarily unavailable for writes when the data is transferred to the new cluster\. It is available for reads\. The new cluster is populated in the background\. After the new cluster is fully populated, queries should reach optimal performance\. When the resize process nears completion, Amazon Redshift updates the endpoint of the new cluster, and all connections to the original cluster are terminated\.

After the resize completes, Amazon Redshift sends an event notification\. You can connect to the new cluster and resume running read and write queries\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

#### New console<a name="resize-monitor"></a>

To monitor the progress of a resize operation using the Amazon Redshift console, choose **CLUSTERS**, then choose the cluster being resized to see the details\. 

#### Original console<a name="resize-monitor-originalconsole"></a>

To monitor the progress of an elastic resize operation using the Amazon Redshift console, choose the **Status** tab on the cluster details page\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status-example.png)

You can't use elastic resize on single\-node clusters\. 

To run an elastic resize on a cluster that is transferring data from a shared snapshot, at least one backup must be available for the cluster\. You can view your backups on the Amazon Redshift console snapshots list, the `describe-cluster-snapshots` CLI command, or the `DescribeClusterSnapshots` API operation\.

Elastic resize doesn't sort tables or reclaim disk space, so it isn't a substitute for a vacuum operation\. A classic resize copies tables to a new cluster, so it can reduce the need to vacuum\. For more information, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html)\.

Elastic resize has the following constraints: 
+ Elastic resize is available only for clusters that use the EC2\-VPC platform\. For more information, see [Use EC2\-VPC when you create your cluster](working-with-clusters.md#cluster-platforms)\. 
+ The new node configuration must have enough storage for existing data\. Even when you add nodes, your new configuration might not have enough storage because of the way that data is redistributed\. 
+ The possible configurations \(number of nodes and node type\) you can resize to is determined by the number of nodes in the original cluster and the target node type of the resized cluster\. To determine the possible configurations available, you can use the Amazon Redshift console or the `describe-node-configuration-options` AWS CLI command with `action-type resize-cluster`\. For more information about the resizing using the Amazon Redshift console, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\. 

  With the AWS CLI, run the following command to describe the configuration options available\. In this example, the cluster named `mycluster` is a `dc2.large` 8\-node cluster\.

  ```
  aws redshift describe-node-configuration-options --cluster-identifier mycluster --region eu-west-1 -—action-type resize-cluster
  ```

  This command returns an option list with recommended node types, number of nodes, and disk utilization for each option\. You can choose one of these configurations when you specify the options of the `resize-cluster` AWS CLI command\. 

  ```
  {
      "NodeConfigurationOptionList": [
          {
              "NodeType": "dc2.8xlarge",
              "NumberOfNodes": 2,
              "EstimatedDiskUtilizationPercent": 21.57
          },
          {
              "NodeType": "dc2.8xlarge",
              "NumberOfNodes": 3,
              "EstimatedDiskUtilizationPercent": 14.71
          },
          {
              "NodeType": "dc2.8xlarge",
              "NumberOfNodes": 4,
              "EstimatedDiskUtilizationPercent": 11.29
          },
          {
              "NodeType": "dc2.large",
              "NumberOfNodes": 16,
              "EstimatedDiskUtilizationPercent": 58.53
          },
          {
              "NodeType": "dc2.large",
              "NumberOfNodes": 32,
              "EstimatedDiskUtilizationPercent": 37.46
          },
          {
              "NodeType": "ds2.8xlarge",
              "NumberOfNodes": 2,
              "EstimatedDiskUtilizationPercent": 3.45
          },
          {
              "NodeType": "ds2.8xlarge",
              "NumberOfNodes": 3,
              "EstimatedDiskUtilizationPercent": 2.35
          },
          {
              "NodeType": "ds2.8xlarge",
              "NumberOfNodes": 4,
              "EstimatedDiskUtilizationPercent": 1.81
          },
          {
              "NodeType": "ds2.xlarge",
              "NumberOfNodes": 8,
              "EstimatedDiskUtilizationPercent": 7.86
          },
          {
              "NodeType": "ds2.xlarge",
              "NumberOfNodes": 16,
              "EstimatedDiskUtilizationPercent": 4.57
          },
          {
              "NodeType": "ds2.xlarge",
              "NumberOfNodes": 32,
              "EstimatedDiskUtilizationPercent": 2.93
          },
          {
              "NodeType": "ra3.16xlarge",
              "NumberOfNodes": 2,
              "EstimatedDiskUtilizationPercent": 0.9
          },
          {
              "NodeType": "ra3.16xlarge",
              "NumberOfNodes": 3,
              "EstimatedDiskUtilizationPercent": 0.62
          },
          {
              "NodeType": "ra3.16xlarge",
              "NumberOfNodes": 4,
              "EstimatedDiskUtilizationPercent": 0.47
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 3,
              "EstimatedDiskUtilizationPercent": 0.62
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 4,
              "EstimatedDiskUtilizationPercent": 0.47
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 5,
              "EstimatedDiskUtilizationPercent": 0.39
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 6,
              "EstimatedDiskUtilizationPercent": 0.33
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 7,
              "EstimatedDiskUtilizationPercent": 0.29
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 8,
              "EstimatedDiskUtilizationPercent": 0.26
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 9,
              "EstimatedDiskUtilizationPercent": 0.23
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 10,
              "EstimatedDiskUtilizationPercent": 0.21
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 11,
              "EstimatedDiskUtilizationPercent": 0.2
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 12,
              "EstimatedDiskUtilizationPercent": 0.19
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 13,
              "EstimatedDiskUtilizationPercent": 0.17
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 14,
              "EstimatedDiskUtilizationPercent": 0.17
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 15,
              "EstimatedDiskUtilizationPercent": 0.16
          },
          {
              "NodeType": "ra3.4xlarge",
              "NumberOfNodes": 16,
              "EstimatedDiskUtilizationPercent": 0.15
          }
      ]
  }
  ```

**Note**  
There are scenarios when you can't use elastic resize to change the number of nodes to a specific value\. For example, if you use elastic resize to change a 4\-node `dc2.8xlarge` cluster to a 6\-node cluster, and then to an 8\-node cluster, the maximum number of nodes for this `dc2.8xlarge` cluster has been reached at 8 nodes\. To go beyond the 8 node limit, for example to 10 nodes, you can use [Classic resize](#classic-resize) to increase the number of nodes to a 10\-node `dc2.8xlarge` cluster\. Using classic resize in this scenario also raises the maximum number of nodes to 20 for future elastic resize operations on this cluster\. 

### Classic resize<a name="classic-resize"></a>

With the classic resize operation, your data is copied in parallel from the compute node or nodes in your source cluster to the compute node or nodes in the target cluster\. The time that it takes to resize depends on the amount of data and the number of nodes in the smaller cluster\. It can take anywhere from a couple of hours to a couple of days or longer\. 

The duration of a classic resize varies based several factors, including: 
+ The workload on the source cluster\.
+ The number and size of the tables being transferred\.
+ How evenly data is distributed across the compute nodes and slices\.
+ The node configuration in the source and target clusters\.

When you start the resize operation, Amazon Redshift puts the existing cluster into read\-only mode until the resize finishes\. During this time, you can only run queries that read from the database\. You can't run any queries that write to the database, including read\-write queries\. For more information, see [Write and read\-write operations](https://docs.aws.amazon.com/redshift/latest/dg/c_write_readwrite.html) in the *Amazon Redshift Database Developer Guide\. *

**Note**  
To resize with minimal production impact, you can use the steps in the following section, [Snapshot, restore, and resize](#rs-tutorial-snapshot-restore-resize-overview)\. You can use these steps to create a copy of your cluster, resize the copy, and then switch the connection endpoint to the resized cluster when the resize is complete\. 

Both the classic resize approach and the snapshot and restore approach copy user tables and data to the new cluster; they don't retain system tables and data\. With either classic resize or snapshot and restore, if you have enabled audit logging in your source cluster, you can continue to access the logs in Amazon S3\. With these approaches, you can still access the logs after you delete the source cluster\. You can keep or delete these logs as your data policies specify\. Elastic resize retains the system log tables\. 

After Amazon Redshift puts the source cluster into read\-only mode, it provisions a new cluster, the target cluster\. It does so using the information that you specify for the node type, cluster type, and number of nodes\. Then Amazon Redshift copies the data from the source cluster to the target cluster\. When this is complete, all connections switch to use the target cluster\. If you have any queries in progress at the time this switch happens, your connection is lost and you must restart the query on the target cluster\. You can view the resize progress on the Amazon Redshift console\. 

Amazon Redshift doesn't sort tables during a resize operation, so the existing sort order is maintained\. When you resize a cluster, Amazon Redshift distributes the database tables to the new nodes based on their distribution styles and runs an ANALYZE command to update statistics\. Rows that are marked for deletion aren't transferred, so you need to run only a VACUUM command if your tables need to be resorted\. For more information, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the* Amazon Redshift Database Developer Guide\. *

You can cancel a classic resize operation before it completes by choosing **Cancel resize** from the cluster details in the Amazon Redshift console\. The amount of time it takes to cancel a resize depends on the stage of the resize operation when you cancel\. The cluster isn't available until the cancel resize operation completes\. If the resize operation is in the final stage, you can't cancel the operation\.

### Snapshot, restore, and resize<a name="rs-tutorial-snapshot-restore-resize-overview"></a>

As described in the preceding section, the time it takes to resize a cluster with the classic resize operation depends heavily on the amount of data in the cluster\. 

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

Alternatively, you can rename the source and target clusters before reloading data into the target cluster\. This approach works if you don't have a requirement that any dependent systems and reports be immediately up to date with those for the target cluster\. In this case, step 6 moves to the end of the process described preceding\. 

The rename process is only required if you want applications to continue using the same endpoint to connect to the cluster\. If you don't require this, you can instead update any applications that connect to the cluster to use the endpoint of the target cluster without renaming the cluster\. 

There are a couple of benefits to reusing a cluster name\. First, you don't need to update application connection strings because the endpoint doesn't change, even though the underlying cluster changes\. Second, related items such as Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) notifications are tied to the cluster name\. This tie means that you can continue using the same alarms and notifications that you set up for the cluster\. This continued use is primarily a concern in production environments where you want the flexibility to resize the cluster without reconfiguring related items, such as alarms and notifications\. 

### Details of resizing a cluster<a name="cluster-resize-intro"></a>

 If your storage and performance needs change after you initially provision your cluster, you can resize your cluster\. You can scale the cluster in or out by adding or removing nodes\. Additionally, you can scale the cluster up or down by specifying a different node type\. 

 For example, you can add more nodes, change node types, change a single\-node cluster to a multi\-node cluster, or change a multi\-node cluster to a single\-node cluster\. However, you must ensure that the resulting cluster is large enough to hold the data that you currently have or else the resize will fail\. When using the API, you have to specify the node type, node size, and the number of nodes even if you only change one of the properties\. 

 The following describes the resize process: 

1. When you initiate the resize process, Amazon Redshift sends an event notification that acknowledges the resize request and starts to provision the new \(target\) cluster\.

1. When the new \(target\) cluster is provisioned, Amazon Redshift sends an event notification that the resize has started, then restarts your existing \(source\) cluster in read\-only mode\. The restart terminates all existing connections to the cluster\. All uncommitted transactions \(including COPY\) are rolled back\. While the cluster is in read\-only mode, you can run read queries but not write queries\.

1. Amazon Redshift starts to copy data from the source cluster to the target cluster\.

1. When the resize process nears completion, Amazon Redshift updates the endpoint of the target cluster, and all connections to the source cluster are terminated\.

1. After the resize completes, Amazon Redshift sends an event notification that the resize has completed\. You can connect to the target cluster and resume running read and write queries\.

When you resize your cluster, it will remain in read\-only mode until the resize completes\. You can view the resize progress on the Amazon Redshift console\. The time it takes to resize a cluster depends on the amount of data in each node\. Typically, the resize process varies from a couple of hours to a day, although clusters with larger amounts of data might take even longer\. This is because the data is copied in parallel from each node on the source cluster to the nodes in the target cluster\. For more information about resizing clusters, see [Resizing a cluster](managing-clusters-console.md#resizing-cluster)\. 

Amazon Redshift doesn't sort tables during a resize operation\. When you resize a cluster, Amazon Redshift distributes the database tables to the new compute nodes based on their distribution styles and runs an ANALYZE to update statistics\. Rows that are marked for deletion aren't transferred, so you need to run a VACUUM only if your tables need to be resorted\. For more information, see [Vacuuming tables](https://docs.aws.amazon.com/redshift/latest/dg/t_Reclaiming_storage_space202.html) in the *Amazon Redshift Database Developer Guide*\. 

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

You can pause and resume a cluster on the new Amazon Redshift console \(not the original console\), with the AWS CLI, or with Amazon Redshift API operations\. 

You can schedule actions to pause and resume a cluster\. When you use the new Amazon Redshift console to create a recurring schedule to pause and resume, then two scheduled actions are created for the date range that you choose\. The scheduled action names are suffixed with `-pause` and `-resume`\. The total length of the name must fit within the maximum size of a scheduled action name\. 

You can't pause the following types of clusters: 
+ EC2\-Classic clusters\. 
+ Clusters that are not active, for example a cluster that is currently modifying\. 
+ Hardware security module \(HSM\) clusters\. 
+ Clusters that have automated snapshots disabled\. 

When deciding to pause a cluster, consider the following: 
+ Connections or queries to the cluster aren't available\. 
+ You can't see query monitoring information of a paused cluster on the Amazon Redshift console\. 
+ You can't modify a paused cluster\. Any scheduled actions on the cluster aren't done\. These include creating snapshots, resizing clusters, and cluster maintenance operations\. 
+ Hardware metrics aren't created\. Update your CloudWatch alarms if you have alarms set on missing metrics\. 
+ You can't copy the latest automated snapshots of a paused cluster to manual snapshots\. 
+ While a cluster is pausing it can't be resumed until the pause operation is complete\. 
+ When you pause a cluster, billing is suspended\. However, the pause operation typically completes within 15 minutes, depending upon the size of the cluster\. 
+  Audit logs are archived and not restored on resume\. 

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

You might rename a cluster if you want to change the cluster to which your applications connect without having to change the endpoint in those applications\. In this case, you must first rename the original cluster and then change the second cluster to reuse the name of the original cluster before the rename\. Doing this is necessary because the cluster identifier must be unique within your account and region, so the original cluster and second cluster cannot have the same name\. You might do this if you restore a cluster from a snapshot and don't want to change the connection properties of any dependent applications\. 

**Note**  
 If you delete the original cluster, you are responsible for deleting any unwanted cluster snapshots\. 

When you rename a cluster, the cluster status changes to `renaming` until the process finishes\. The old DNS name that was used by the cluster is immediately deleted, although it could remain cached for a few minutes\. The new DNS name for the renamed cluster becomes effective within about 10 minutes\. The renamed cluster is not available until the new name becomes effective\. The cluster will be rebooted and any existing connections to the cluster will be dropped\. After this completes, the endpoint will change to use the new name\. For this reason, you should stop queries from running before you start the rename and restart them after the rename finishes\. 

 Cluster snapshots are retained, and all snapshots associated with a cluster remain associated with that cluster after it is renamed\. For example, suppose that you have a cluster that serves your production database and the cluster has several snapshots\. If you rename the cluster and then replace it in the production environment with a snapshot, the cluster that you renamed still has those existing snapshots associated with it\. 

 Amazon CloudWatch alarms and Amazon Simple Notification Service \(Amazon SNS\) event notifications are associated with the name of the cluster\. If you rename the cluster, you need to update these accordingly\. You can update the CloudWatch alarms in the CloudWatch console, and you can update the Amazon SNS event notifications in the Amazon Redshift console on the **Events** pane\. The load and query data for the cluster continues to display data from before the rename and after the rename\. However, performance data is reset after the rename process finishes\. 

For more information, see [Modifying a cluster](managing-clusters-console.md#modify-cluster)\.

## Shutting down and deleting clusters<a name="rs-mgmt-shutdown-delete-cluster"></a>

You can shut down your cluster if you want to stop it from running and incurring charges\. When you shut it down, you can optionally create a final snapshot\. If you create a final snapshot, Amazon Redshift will create a manual snapshot of your cluster before shutting it down\. You can later restore that snapshot if you want to resume running the cluster and querying data\. 

If you no longer need your cluster and its data, you can shut it down without creating a final snapshot\. In this case, the cluster and data are deleted permanently\. For more information about shutting down and deleting clusters, see [Deleting a cluster](managing-clusters-console.md#delete-cluster)\. 

Regardless of whether you shut down your cluster with a final manual snapshot, all automated snapshots associated with the cluster will be deleted after the cluster is shut down\. Any manual snapshots associated with the cluster are retained\. Any manual snapshots that are retained, including the optional final snapshot, are charged at the Amazon Simple Storage Service storage rate if you have no other clusters running when you shut down the cluster, or if you exceed the available free storage that is provided for your running Amazon Redshift clusters\. For more information about snapshot storage charges, see the [Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\. 