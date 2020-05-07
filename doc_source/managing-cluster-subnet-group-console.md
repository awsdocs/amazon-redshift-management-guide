# Managing cluster subnet groups using the console<a name="managing-cluster-subnet-group-console"></a>

You can manage your cluster subnet groups using the Amazon Redshift console\. You can create a cluster subnet group, manage an existing one, or delete one\. All of these tasks start from the cluster subnet group list\. You must select a cluster subnet group to manage it\.

You can provision a cluster on one of the subnets that you provide the subnet group\. A cluster subnet group enables you to specify a set of subnets in your virtual private cloud \(VPC\)\. 

## Creating a cluster subnet group<a name="create-cluster-subnet-group"></a>

You must have at least one cluster subnet group defined to provision a cluster in a VPC\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-subnet-create"></a>

**To create a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose **Create cluster subnet group** to display the create page\. 

1. Enter information for the subnet group, including which subnets to add\. 

1. Choose **Create cluster subnet group** to create the group with the subnets that you chose\. 

### Original console<a name="cluster-subnet-create-originalconsole"></a><a name="create-cluster-subnet-group-task"></a>

**To create a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Subnet Groups** tab, choose **Create Cluster Subnet Group**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-05.png)

1. In the **Create Cluster Subnet Group** dialog box, add subnets to the group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-10.png)

   1. Specify a **Name**, **Description**, and **VPC ID** value for the cluster subnet group\.

   1. Add subnets to the group by doing one of the following:
      + Choose the **add all the subnets** link\.
      + Use the **Availability Zone** and **Subnet ID** boxes to choose a specific subnet and then choose **Add**\.

      The following example shows a cluster subnet group specified with one subnet group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-20.png)

1. Choose **Yes, Create**\.

   The new group is displayed in the list of cluster subnet groups\.

## Modifying a cluster subnet group<a name="describe-cluster-subnet-group"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-subnet-modify"></a>

**To modify a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose the subnet group to modify\. 

1. For **Actions**, choose **Modify** to display the details of the subnet group\. 

1. Update information for the subnet group\. 

1. Choose **Save** to modify the group\. 

### Original console<a name="cluster-subnet-modify-originalconsole"></a><a name="describe-cluster-subnet-group-task"></a>

**To modify a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Subnet Groups** tab, in the cluster subnet group list, choose the row of the group that you want to modify, and then choose **Edit**\.

   In the example following, `subnetgroup2` is the cluster subnet group to modify\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-modify-10.png)

1. In the **Cluster Subnet Group Details**, take one of the following actions\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-subnet-group-console.html)

## Deleting a cluster subnet group<a name="modify-cluster-subnet-group"></a>

You can't delete a cluster subnet group that is used by a cluster\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-subnet-delete"></a>

**To delete a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose the subnet group to delete, then choose **Delete**\. 

### Original console<a name="cluster-subnet-delete-originalconsole"></a><a name="delete-cluster-subnet-group-task"></a>

**To delete a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Subnet Groups** tab, in the cluster subnet group list, choose the row of the group that you want to delete\.

   In the example following, `my-subnet-group` is the cluster subnet group to delete\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-delete-10.png)

1. In the Delete Cluster Subnet Group dialog box, choose **Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-delete-20.png)