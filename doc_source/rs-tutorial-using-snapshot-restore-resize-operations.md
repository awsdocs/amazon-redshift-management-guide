# Tutorial: Using the Snapshot, Restore, and Resize Operations to Resize a Cluster<a name="rs-tutorial-using-snapshot-restore-resize-operations"></a>

This section walks you through the process of using the snapshot and restore operations as part of a resize process for an Amazon Redshift cluster\. This process is an advanced one that is useful primarily in environments where you are unable or do not want to stop write and read\-write operations in the database for the period of time it takes to resize your cluster\. If you are unsure how long your cluster takes to resize, you can use this procedure to take a snapshot, restore it into a new cluster, and then resize it to get an estimate\. This section takes that process further by switching from the source to the target cluster after the resize of the target cluster completes\. 

**Important**  
 You are charged for any clusters until they are deleted\. 

Complete this tutorial by performing the steps in the following: 

+ [Prerequisites](#rs-tutorial-snapshot-restore-resize-prereq)

+ [Step 1: Take a Snapshot](#rs-tutorial-take-snapshot)

+ [Step 2: Restore the Snapshot into the Target Cluster](#rs-tutorial-restore-snapshot)

+ [Step 3: Verify Data in the Target Cluster](#rs-tutorial-verify-data)

+ [Step 4: Resize the Target Cluster](#rs-tutorial-resize-target-cluster)

+ [Step 5: Copy Post\-Snapshot Data from the Source to the Target Cluster](#rs-tutorial-copy-data-to-target-cluster)

+ [Step 6: Rename the Source and Target Clusters](#rs-tutorial-rename-clusters)

+ [Step 7: Delete the Source Cluster](#rs-tutorial-delete-source-cluster)

+ [Step 8: Clean Up Your Environment](#rs-tutorial-resize-clean-up)

## Prerequisites<a name="rs-tutorial-snapshot-restore-resize-prereq"></a>

Before you start this tutorial, make sure that you have the following prerequisites: 

+ A sample cluster\. In this example, you’ll start with the sample cluster that you created in the [Amazon Redshift Getting Started](http://docs.aws.amazon.com/redshift/latest/gsg/) exercise\. If you don't have a sample cluster to use for this tutorial, complete the Getting Started exercise to create one and then return to this tutorial\. 

+ A SQL client tool or application to connect to the cluster\. This tutorial uses SQL Workbench/J, which you installed if you performed the steps in the [Amazon Redshift Getting Started](http://docs.aws.amazon.com/redshift/latest/gsg/) exercise\. If you do not have SQL Workbench/J or another SQL client tool, see [Connect to Your Cluster by Using SQL Workbench/J](connecting-using-workbench.md)\. 

+ Sample data\. In this tutorial, you’ll take a snapshot of your cluster, and then perform some write queries in the database that cause a difference between the data in the source cluster and the new cluster where you will restore the snapshot\. Before you begin this tutorial, load your cluster with the sample data from Amazon S3 as described in the [Amazon Redshift Getting Started](http://docs.aws.amazon.com/redshift/latest/gsg/) exercise\. 

## Step 1: Take a Snapshot<a name="rs-tutorial-take-snapshot"></a>

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster from the Amazon Redshift Getting Started exercise, click **examplecluster**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-landing-page.png)

1.  On the **Configuration** tab of the **Cluster** details page, click **Take Snapshot** in the **Backup** list\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-backup-snapshot.png)

1.  In the **Create Snapshot** window, type *examplecluster\-source* in the **Snapshot Identifier** box, and then click **Create**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-backup-snapshot-create.png)

1.  In the navigation pane, click **Snapshots** and verify that a new manual snapshot is being created\. The snapshot status will be **creating**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-snapshots-creating.png)

## Step 2: Restore the Snapshot into the Target Cluster<a name="rs-tutorial-restore-snapshot"></a>

1. In the navigation pane, click **Snapshots**, and then select the **examplecluster\-source** snapshot\. 

1. Click **Restore From Snapshot**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-snapshots-restore-from-snapshot-button.png)

1. In the **Choose Platform** window, select the platform you want to restore the cluster into\. If your account and region continue to support the EC2\-Classic platform, choose **EC2\-Classic**\. Otherwise, choose **EC2\-VPC**\. Then, click **Continue**\. 
**Note**  
 If you choose EC2\-VPC, you must have a cluster subnet group\. For more information, see [Creating a Cluster Subnet Group](managing-cluster-subnet-group-console.md#create-cluster-subnet-group)\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-snapshots-restore-choose-platform.png)

1. In the **Restore Cluster From Snapshot** window, do the following: 

   +  **Snapshot Identifier**: check the snapshot name, **examplecluster\-source**\. 

   +  **Cluster Identifier**: type **examplecluster\-target**\. 

   +  **Port**: leave the port number as is\. 

   +  **Allow Version Upgrade**: leave this option as **Yes**\. 

   +  **Availability Zone**: select an Availability Zone\. 

   +  **Cluster Parameter Group**: select a parameter group to use\. 

   +  **Cluster Security Group**: select a security group or groups to use\. 

1.  In the navigation pane, click **Clusters**\. A new cluster, **examplecluster\-target**, will be created from the source cluster’s snapshot\. 

    First, the target cluster is created\. The **Cluster Status** value is **creating, restoring** at this point\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-restoring.png)

    After the target cluster is created, the **Cluster Status** value changes to **available, restoring**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-restoring-available.png)

1.  Click **examplecluster\-target** to open it\. The **Cluster Status** value should display **available**, and the **Restore Status** should display **completed**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-restore-completed.png)

## Step 3: Verify Data in the Target Cluster<a name="rs-tutorial-verify-data"></a>

After the restore operation completes, you can verify that the data in the target cluster meets your expectation of the data that you had in the snapshot from the source\. You can use a SQL client tool to connect to the target cluster and run a query to validate the data in the new cluster\. For example, you can run the same queries that you ran in the Amazon Redshift Getting Started exercise: 

```
-- Get definition for the sales table.
SELECT *    
FROM pg_table_def    
WHERE tablename = 'sales';    

-- Find total sales on a given calendar date.
SELECT sum(qtysold) 
FROM   sales, date 
WHERE  sales.dateid = date.dateid 
AND    caldate = '2008-01-05';

-- Find top 10 buyers by quantity.
SELECT firstname, lastname, total_quantity 
FROM   (SELECT buyerid, sum(qtysold) total_quantity
        FROM  sales
        GROUP BY buyerid
        ORDER BY total_quantity desc limit 10) Q, users
WHERE Q.buyerid = userid
ORDER BY Q.total_quantity desc;

-- Find events in the 99.9 percentile in terms of all-time gross sales.
SELECT eventname, total_price 
FROM  (SELECT eventid, total_price, ntile(1000) over(order by total_price desc) as percentile 
       FROM (SELECT eventid, sum(pricepaid) total_price
             FROM   sales
             GROUP BY eventid)) Q, event E
       WHERE Q.eventid = E.eventid
       AND percentile = 1
ORDER BY total_price desc;
```

## Step 4: Resize the Target Cluster<a name="rs-tutorial-resize-target-cluster"></a>

Once you verify that your target cluster works as expected, you can resize the target cluster\. You can continue to allow write and read\-write operations in the source cluster, because later in this tutorial you will copy any data that was loaded after your snapshot to the target\. 

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster from this tutorial, click **examplecluster\-target**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Resize** in the **Cluster** list\. 

1.  In the **Resize Cluster** window, select the following values: 

   +  **Node Type**: **dc1\.large**\. 

   +  **Cluster Type**: **Multi Node**\. 

   +  **Number of Nodes**: **2**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-resize.png)

1.  Click **Resize**\. 

1.  Click **Status**, and review the resize status information to see the resize progress\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-resize-inprogress-03.png)

## Step 5: Copy Post\-Snapshot Data from the Source to the Target Cluster<a name="rs-tutorial-copy-data-to-target-cluster"></a>

For the purposes of this tutorial, this step provides a simple set of COPY statements to load data from Amazon S3 into Amazon Redshift\. This step is included to simulate bringing the target cluster up\-to\-date with the same data as the source cluster\. It is not meant to demonstrate an effort to bring an actual production environment into line between the source and target cluster\. In production environments, your own ETL process will determine how load your target cluster with all the same data as the source cluster after the snapshot was taken\. 

If there have been multiple loads after the snapshot was taken, you’ll need to make sure that you rerun the loads in the target database in the same order as they were run in the source database\. Additionally, if there continue to be loads into the source database while you are working on bringing the target cluster up\-to\-date, you will need to repeat this process until the target and source match, and find a suitable time to rename the clusters and switch applications to connect to the target database\. 

In this example, let’s suppose that your ETL process loaded data into the source cluster after the snapshot was taken\. Perhaps Amazon Redshift was still in the process of restoring the target cluster from the snapshot, or resizing the target cluster\. There were some new categories, events, dates, and venues added to the TICKIT database\. You now need to get this same data into the target cluster before you switch to use it going forward\. 

First, you’ll use the following COPY statements to load new data from Amazon S3 to the tables in your Amazon Redshift TICKIT database in the target cluster\. 

The sample data for this tutorial is provided in Amazon S3 buckets that are owned by Amazon Redshift\. The bucket permissions are configured to allow all authenticated AWS users read access to the sample data files\. To load the sample data, make sure you have the following for your IAM user: 

+  Your access key and secret access key\. If you do not know these, you can create new ones\. For more information, go to [ Administering Access Keys for IAM Users](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in *IAM User Guide*\. 

+  At least `LIST` and `GET` permissions to Amazon S3 resources\. You can grant your IAM user these permissions by attaching the `AmazonS3ReadOnlyAccess` managed policy to your IAM user or to the group to which your IAM user belongs\. For more information about attaching policies, go to [Managing IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage.html) in the *IAM User Guide*\. 
**Note**  
Without proper permissions to Amazon S3, you receive the following error message when running the COPY command: `S3ServiceException: Access Denied`\.

Replace *<access\-key\-id>* and *<secret\-access\-key>* with the access key and secret access key for your IAM user\. Then run the commands in your SQL client tool\.

```
copy venue from 's3://awssampledb/resize/etl_venue_pipe.txt' CREDENTIALS 'aws_access_key_id=<Your-Access-Key-ID>;aws_secret_access_key=<Your-Secret-Access-Key>' delimiter '|' region 'us-east-1';
copy category from 's3://awssampledb/resize/etl_category_pipe.txt' CREDENTIALS 'aws_access_key_id=<Your-Access-Key-ID>;aws_secret_access_key=<Your-Secret-Access-Key>' delimiter '|' region 'us-east-1';
copy date from 's3://awssampledb/resize/etl_date_pipe.txt' CREDENTIALS 'aws_access_key_id=<Your-Access-Key-ID>;aws_secret_access_key=<Your-Secret-Access-Key>' delimiter '|' region 'us-east-1';
copy event from 's3://awssampledb/resize/etl_events_pipe.txt' CREDENTIALS 'aws_access_key_id=<Your-Access-Key-ID>;aws_secret_access_key=<Your-Secret-Access-Key>' delimiter '|' timeformat 'YYYY-MM-DD HH:MI:SS' region 'us-east-1';
```

## Step 6: Rename the Source and Target Clusters<a name="rs-tutorial-rename-clusters"></a>

Once you verify that your target cluster has been brought up to date with any data needed from the ETL process, you can switch to the target cluster\. If you need to keep the same name as the source cluster, you’ll need to do a few manual steps to make the switch\. These steps involve renaming the source and target clusters, during which time they will be unavailable for a short period of time\. However, if you are able to update any data sources to use the new target cluster, you can skip this section\.

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster from this tutorial, click **examplecluster**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Modify** in the **Cluster** list\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1.  In the **Modify Cluster** window, type **examplecluster\-source** in the **New Cluster Identifier** box, and then click **Modify**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-modify-rename.png)

1.  In the navigation pane, click **Clusters**, and then click **examplecluster\-target**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Modify** in the **Cluster** list\. 

1.  In the **Modify Cluster** window, type **examplecluster** in the **New Cluster Identifier** box, and then click **Modify**\. 

If you had any queries running in the source cluster, you’ll need to start them over and run them to completion on the target cluster\. 

## Step 7: Delete the Source Cluster<a name="rs-tutorial-delete-source-cluster"></a>

After you are sure that you no longer need the source cluster, you can delete it\. In a production environment, whether you decide to keep a final snapshot depends on your data policies\. In this tutorial, you’ll delete the cluster without a final snapshot because you are using sample data\. 

**Important**  
 You are charged for any clusters until they are deleted\. 

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster names from this tutorial, click **examplecluster\-source**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Delete** in the **Cluster** list\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1.  In the **Delete Cluster** window, click **No** for **Create final snapshot**, and then click **Delete**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-delete-nosnapshot.png)

## Step 8: Clean Up Your Environment<a name="rs-tutorial-resize-clean-up"></a>

After you have completed this tutorial, you can clean up your environment by deleting the target cluster\. To do this, follow the steps in [Step 7: Delete the Source Cluster](#rs-tutorial-delete-source-cluster) and instead delete the target cluster\. Doing this will return your environment back to the state it was in before you started the tutorial\. Returning the environment to the original state is important to help reduce any costs associated with having clusters running\. 

**Important**  
 You are charged for any clusters until they are deleted\. 