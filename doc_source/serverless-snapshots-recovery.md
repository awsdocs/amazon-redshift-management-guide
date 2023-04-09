# Working with snapshots and recovery points<a name="serverless-snapshots-recovery"></a>

## Working with snapshots and recovery points<a name="serverless-snapshots-recovery-points"></a>

A serverless snapshot includes all objects and data within a namespace\. There are three scenarios in which you can restore snapshots:
+ Restore a serverless snapshot to a serverless namespace\.
+ Restore a serverless snapshot to a provisioned cluster\.
+ Restore a provisioned cluster snapshot to a serverless namespace\.

You can restore a snapshot you created on the Amazon Redshift Serverless console to a provisioned Amazon Redshift cluster\. When you do so, you choose the node type to use, such as RA3, and the number of nodes, letting you control settings at the cluster or node level\. To initiate the restore, you choose a snapshot from the Amazon Redshift Serverless console and then specify the ID of the provisioned cluster to restore to\.

To restore a provisioned cluster snapshot to a serverless namespace, start from the Redshift provisioned console, choose the snapshot to restore, then choose **Restore from snapshot**, **Restore to serverless namespace**\. Amazon Redshift converts tables with interleaved keys into compound sort keys when you restore a provisioned cluster snapshot to a serverless namespace\. For more information about sort keys, see [Working with sort keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)\. 

Restoring a snapshot to a serverless namespace replaces the database with the database in the snapshot\. You can also restore recovery points, which are created approximately every 30 minutes and kept for 24 hours, to your serverless namespace\.

Restoring a snapshot to a serverless namespace is completed in two phases\. The first phase completes in a few minutes, restores the data to your namespace, and makes it available for queries\. The second phase of restoration is where your database is tuned, which can cause minor performance issues\. This second phase can last from a few hours to several days, and in some cases, a couple of weeks\. The amount of time depends on the size of the data, but performance progressively improves as the database gets tuned\. At the end of this phase, your serverless namespace is fully tuned, and you can submit queries without performance issues\.

If you want to add additional context, you can tag snapshots and recovery points with key\-value pairs that provide metadata and information to snapshots and recovery points\. For more information about tagging resources, see [Tagging resources overview](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-tagging-resources.html)\.

Finally, you can also share snapshots with other AWS accounts, which lets them access data within the snapshot and run queries\.

You can view a snapshot's details by viewing the details page in the Amazon Redshift Serverless console, or by calling the Amazon Redshift Serverless API operation `ListSnapshots` or the `list-snapshots` operation in the AWS CLI\. The Amazon Redshift Serverless console also contains details page for recovery points, or you can call `list-recovery-points` in the AWS CLI or the `ListRecoveryPoints` API operation\.

### Snapshots<a name="serverless-snapshots"></a>

You can restore a snapshot that you created on the Amazon Redshift Serverless console to an available namespace that's associated with a workgroup\. A namespace is available once it is ready for querying and/or modifying\. You can restore a snapshot encrypted with an AWS managed KMS key to a serverless namespace\.  

To see a list of all of your snapshots, on the Amazon Redshift Serverless console, choose **Data backup**\.

To create a snapshot

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose **Create snapshot**\.

1. Choose a namespace to create a snapshot of\.

1. Enter a snapshot identifier\.

1. \(Optional\) Choose a retention period\. If you choose **Custom value**, choose the number of days\. The amount you choose must be between 1\-3653 days, inclusive\. The default is retain indefinitely\.

1. Choose **Create**\.

To create a snapshot from namespace configuration

1. On the Amazon Redshift Serverless console, choose **Namespace configuration**\.

1. Choose the namespace to create a snapshot of\. You can only create snapshots of namespaces that are associated with a workgroup and whose statuses are Available\.

1. Choose the **Data backup** tab\.

1. Choose **Create snapshot**\.

1. Enter a snapshot identifier\.

1. \(Optional\) Choose a retention period\. If you choose **Custom value**, choose the number of days\. The amount you choose must be between 1\-3653 days, inclusive\.

1. Choose **Create**\.

To restore a snapshot to a serverless namespace

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose the snapshot to restore\. You can only restore one snapshot at a time\.

1. Choose **Actions**, **Restore to serverless namespace**\.

1. Choose an available namespace to restore to\. You can only restore to namespaces whose statuses are Available\.

1. Choose **Restore**\.

To restore a snapshot to a provisioned cluster

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose a snapshot to restore\.

1. Choose **Action**, **Restore to provisioned cluster**\.

1. Enter a cluster identifier\.

1. Choose a **Node type**\. The number of nodes depends on the node type\.

1. Follow the instructions on the page on the console page to enter the properties for **Cluster configuration**\. See [ Creating a cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster) for more information\.

To update a snapshot's retention period

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose a snapshot to update\.

1. Choose **Actions**, **Set manual snapshot settings\.**

1. Choose a retention period\. If you choose **Custom value**, choose the number of days\.

1. Choose **Save changes**\.

To delete a snapshot

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose a snapshot to delete\.

1. Choose **Actions**, **Delete**\.

1. Choose **Delete**\.

To create a final snapshot of all data within a namespace before deleting the namespace\.

1. On the Amazon Redshift Serverless console, choose **Namespace configuration**\.

1. Choose the namespace to delete\.

1. Choose **Actions**, **Delete**\.

1. Choose **Create final snapshot**\.

1. Enter a name for the snapshot\.

1. Enter delete\.

1. Choose **Delete**\.

To share a snapshot with another AWS account

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose a snapshot to share\.

1. Choose **Actions**, **Manage access**\.

1. Enter an **AWS account ID**\.

1. Choose **Save changes**\.

For more information about snapshots on provisioned clusters, see [Amazon Redshift snapshots](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-snapshots.html) in the *Amazon Redshift Management Guide*\.

### Recovery points<a name="serverless-recovery-point"></a>

Recovery points in Amazon Redshift Serverless are created approximately every 30 minutes and saved for 24 hours\. 

On the Amazon Redshift Serverless console, choose **Data backup** to manage recovery points\. You can do the following operations:
+ Restore a recovery point to a serverless namespace\.
+ Convert a recovery point to a snapshot\.

To restore a recovery point to a serverless namespace

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Under **Recovery points**, choose the **Creation time** of the recovery point that you want to restore\.

1. Choose **Restore**\. You can only restore to namespaces whose statuses are Available\.

1. Enter **restore** in the text input field and choose **Restore**\.

To convert a recovery point to a snapshot

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Under **Recovery points**, choose the **Creation time** of the recovery point that you want to convert to a snapshot\.

1. Choose **Create snapshot from recovery point**\.

1. Enter a **Snapshot identifier**\.

1. Choose **Create**\.

### Restoring a table from a snapshot<a name="serverless-table-restore"></a>

 Rather than restore an entire snapshot to your serverless namespace, you can also restore a specific table from a snapshot\. When you do so, you specify the source snapshot, database, schema, table, and the target database, schema, and new table name\. This new table cannot have the same name as an existing table\. If you want to replace an existing table by restoring a table from a snapshot, you must first rename or drop the table before you restore the table\.

 The target table is created using the source table's column definitions, table attributes, and column attributes except for foreign keys\. To prevent conflicts due to dependencies, the target table doesn't inherit foreign keys from the source table\. Any dependencies, such as views or permissions granted on the source table, aren't applied to the target table\. 

If the owner of the source table exists, then that user is the owner of the restored table, provided that the user has sufficient permissions to become the owner of a relation in the specified database and schema\. Otherwise, the restored table is owned by the admin user that was created when the cluster was launched\.

The restored table returns to the state it was in at the time the backup was taken\. This includes transaction visibility rules defined by the Amazon Redshift adherence to [serializable isolation](https://docs.aws.amazon.com/redshift/latest/dg/c_serial_isolation.html), meaning that data will be immediately visible to in flight transactions started after the backup\.

 You can use the Amazon Redshift Serverless console to restore tables from a snapshot\. 

Restoring a table from a snapshot has the following limitations:
+ You can only restore one table at a time\.
+ Any dependencies, such as views or permissions granted on the source table, aren't applied to the target table\.
+ If row\-level security is turned on for a table being restored, Amazon Redshift Serverless restores the table with row\-level security turned on\.

To restore a table from a snapshot using the Amazon Redshift Serverless console

1. On the Amazon Redshift Serverless console, choose **Data backup**\.

1. Choose a snapshot to restore\.

1. Choose **Actions**, **Restore table from snapshot**\.

1. Enter information about the source snapshot and target table, then choose **Restore table**\.

### Managing snapshots and recovery points using the AWS Command Line Interface and Amazon Redshift Serverless API<a name="serverless-snapshots-and-recovery-points-cli"></a>

 Aside from using the AWS console, you can also use the AWS CLI or the Amazon Redshift Serverless API to interact with snapshots and recovery points\. 

 To create a snapshot, use the AWS Command Line Interface operation create\-snapshot\. You can also use the Amazon Redshift Serverless API operation `CreateSnapshot`\. Snapshots must be associated with a namespace, so you must include the name of a namespace in your request\. By default, Amazon Redshift Serverless keeps snapshots for an indefinite period, but you can specify a retention period in your request\. 

 To restore a snapshot, use `restore-from-snapshot` or `RestoreFromSnapshot`\. If you're restoring a snapshot from Amazon Redshift Serverless to a provisioned cluster, you must specify the `snapshotArn` of the snapshot you're restoring\. Otherwise, if you're restoring from serverless to serverless, you can specify `snapshotArn` or `snapshotName`, but not both\. 

 To retrieve information about a snapshot, such as which namespace it is associated with or how large the data inside a snapshot is, use the AWS CLI operation `get-snapshot`\. Alternatively, you can also use the API operation `GetSnapshot`\. To get information about multiple snapshots at once, use `list-snapshots` or `ListSnapshots`\. 

 To delete a snapshot, use the CLI operation `delete-snapshot` or the API operation `DeleteSnapshot`\. 

 Recovery points are automatically created by Amazon Redshift Serverless, so you cannot use a AWS CLI or API operation to create recovery points\. You can use `restore-from-recovery-point` or `RestoreFromRecoveryPoint` to restore the data within a recovery point to a namespace, just like restoring data from a snapshot\. You can also convert a recovery point to a snapshot by using `convert-recovery-point-to-snapshot` or `ConvertRecoveryPointToSnapshot`\. 

To see information about a specific recovery point, use `get-recovery-point` or `GetRecoveryPoint`\. To retrieve information about multiple recovery points, use `list-recovery-points` or `ListRecoveryPoints`\.