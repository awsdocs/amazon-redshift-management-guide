# Managing cluster subnet groups using the console<a name="managing-cluster-subnet-group-console"></a>

You can manage your cluster subnet groups using the Amazon Redshift console\. You can create a cluster subnet group, manage an existing one, or delete one\. All of these tasks start from the cluster subnet group list\. You must select a cluster subnet group to manage it\.

You can provision a cluster on one of the subnets that you provide the subnet group\. A cluster subnet group enables you to specify a set of subnets in your virtual private cloud \(VPC\)\. 

## Creating a cluster subnet group<a name="create-cluster-subnet-group"></a>

You must have at least one cluster subnet group defined to provision a cluster in a VPC\.

**To create a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Configurations**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose **Create cluster subnet group** to display the create page\. 

1. Enter information for the subnet group, including which subnets to add\. 

1. Choose **Create cluster subnet group** to create the group with the subnets that you chose\. 

## Modifying a cluster subnet group<a name="describe-cluster-subnet-group"></a>

**To modify a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Configurations**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose the subnet group to modify\. 

1. For **Actions**, choose **Modify** to display the details of the subnet group\. 

1. Update information for the subnet group\. 

1. Choose **Save** to modify the group\. 

## Deleting a cluster subnet group<a name="modify-cluster-subnet-group"></a>

You can't delete a cluster subnet group that is used by a cluster\.

**To delete a cluster subnet group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Configurations**, then choose **Subnet groups**\. The list of subnet groups is displayed\. 

1. Choose the subnet group to delete, then choose **Delete**\. 