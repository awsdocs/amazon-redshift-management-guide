# Managing Cluster Subnet Groups Using the Console<a name="managing-cluster-subnet-group-console"></a>

The section explains how to manage your cluster subnet groups using the Amazon Redshift console\. You can create a cluster subnet group, manage an existing one, or delete one\. All of these tasks start from the cluster subnet group list\. You must select a cluster subnet group to manage it\.

In the example cluster subnet group list below, there is one cluster subnet group\. By default, there are no cluster subnet groups defined for your AWS account\. Because `my-subnet-group` is selected \(highlighted\), you can edit or delete it\. The details of the selected security group are shown under **Cluster Subnet Group Details**\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-list-10.png)

## Creating a Cluster Subnet Group<a name="create-cluster-subnet-group"></a>

You must have at least one cluster subnet group defined to provision a cluster in a VPC\.

**To create a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Security**\.

1. On the **Subnet Groups** tab, click **Create Cluster Subnet Group**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-05.png)

1. In the** Create Cluster Subnet Group** dialog box, add subnets to the group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-10.png)

   1. Specify a **Name**, **Description**, and **VPC ID** for the cluster subnet group\.

   1. Add subnets to the group by doing one of the following:

      + Click **add all the subnets** link\. or

      + Use the **Availability Zone** and **Subnet ID** boxes to choose a specific subnet and then click **Add**\.

      The following example shows a cluster subnet group specified with one subnet group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-create-20.png)

1. Click **Yes, Create**\.

   The new group will be displayed in the list of cluster subnet groups\.

## Modifying a Cluster Subnet Group<a name="describe-cluster-subnet-group"></a>

**To modify a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Security**\.

1. On the **Subnet Groups** tab, in the cluster subnet group list, click the row of the group you want to modify, and then click **Edit**\.

   In the example below, `subnetgroup2` is the cluster subnet group we want to modify\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-modify-10.png)

1. In the **Cluster Subnet Group Details**, take one of the following actions\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/managing-cluster-subnet-group-console.html)

## Deleting a Cluster Subnet Group<a name="modify-cluster-subnet-group"></a>

You cannot delete a cluster subnet group that is used by a cluster\.

**To delete a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, click **Security**\.

1. On the **Subnet Groups** tab, in the cluster subnet group list, click the row of the group you want to delete\.

   In the example below, `my-subnet-group` is the cluster subnet group we want to delete\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-delete-10.png)

1. In the Delete Cluster Subnet Group dialog box, click **Delete**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/subnet-group-delete-20.png)