# Tutorial: Using the Resize Operation to Resize a Cluster<a name="rs-tutorial-using-the-resize-operation"></a>

This section walks you through the process of resizing a cluster by using the resize operation in Amazon Redshift\. In this example, you’ll scale your cluster out by resizing from a single node cluster to a multinode cluster\. 

Complete this tutorial by performing the steps in the following: 

+ [Prerequisites](#rs-tutorial-resize-prereq)

+ [Step 1: Resize the Cluster](#rs-tutorial-resize-cluster-steps)

+ [Step 2: Delete the Sample Cluster](#rs-tutorial-delete-resized-cluster)

## Prerequisites<a name="rs-tutorial-resize-prereq"></a>

 Before you start this tutorial, make sure that you have the following prerequisites: 

+ A sample cluster\. In this example, you’ll start with the sample cluster that you created in the [Amazon Redshift Getting Started](http://docs.aws.amazon.com/redshift/latest/gsg/) exercise\. If you don't have a sample cluster to use for this tutorial, complete the Getting Started exercise to create one and then return to this tutorial\. 

## Step 1: Resize the Cluster<a name="rs-tutorial-resize-cluster-steps"></a>

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster from the [Amazon Redshift Getting Started](http://docs.aws.amazon.com/redshift/latest/gsg/) exercise, click **examplecluster**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Resize** in the **Cluster** list\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-menu.png)

1.  In the **Resize Cluster** window, select the following values: 

   +  **Node Type**: **dc1\.large**\. 

   +  **Cluster Type**: **Multi Node**\. 

   +  **Number of Nodes**: **2**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-resize.png)

1.  Click **Resize**\. 

1.  Click **Status**, and review the resize status information to see the resize progress\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-resize-status-example.png)

## Step 2: Delete the Sample Cluster<a name="rs-tutorial-delete-resized-cluster"></a>

 After you are sure that you no longer need the sample cluster, you can delete it\. In a production environment, whether you decide to keep a final snapshot depends on your data policies\. In this tutorial, you’ll delete the cluster without a final snapshot because you are using sample data\. 

**Important**  
 You are charged for any clusters until they are deleted\. 

1.  Open the Amazon Redshift console\. 

1.  In the navigation pane, click **Clusters**, and then click the cluster to open\. If you are using the same cluster names from this tutorial, click **examplecluster**\. 

1.  On the **Configuration** tab of the **Cluster** details page, click **Delete** in the **Cluster** list\. 

1.  In the **Delete Cluster** window, click **No** for **Create final snapshot**, and then click **Delete**\. 