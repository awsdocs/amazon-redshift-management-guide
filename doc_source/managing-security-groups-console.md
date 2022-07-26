# Managing cluster security groups using the console<a name="managing-security-groups-console"></a>

You can create, modify, and delete cluster security groups by using the Amazon Redshift console\. You can also manage the default cluster security group in the Amazon Redshift console\. All of the tasks start from the cluster security group list\. You must choose a cluster security group to manage it\.

You can't delete the default cluster security group, but you can modify it by authorizing or revoking ingress access\. 

## Creating a cluster security group<a name="security-group-create"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-security-group-create"></a>

**To create a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Security groups** to display the **Cluster security groups** page\. 
**Note**  
You can only manage cluster security groups when logged in with an EC2\-Classic AWS account\.

1. Choose **Create cluster security group** to display the **Create cluster security group** window\. 

1. For the new security group, enter values for the following: 
   + **Name**
   + **Description**
   + **CIDR/IP range to authorize** in the form `nnn.nnn.nnn.nn/nn`
   + **AWS account ID** \(without hyphens\)
   + **EC2 security group name**

1. Choose **Create** to create the security group\. 

### Original console<a name="cluster-security-group-create-originalconsole"></a><a name="security-group-create-task"></a>

**To create a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, choose **Create Cluster Security Group**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-create-10.png)

1. In the **Create Cluster Security Group** dialog box, specify a cluster security group name and description\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-create-20.png)

1. Choose **Create**\.

   The new group is displayed in the list of cluster security groups\.

## Tagging a cluster security group<a name="security-group-tag"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-security-group-tag"></a>

**To tag a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Security groups** to display the **Cluster security groups** page\. 
**Note**  
You can only manage cluster security groups when logged in with an EC2\-Classic AWS account\.

1. Choose a cluster security group, then choose **Manage tags** to display the **Manage tags** page\. 

1. On the **Manage tags** page, add new tags, and update or delete existing tags\. For each new tag, provide information for **Key** and **Value**\. 

1. Choose **Apply** to save your tags\. 

### Original console<a name="cluster-security-group-tag-originalconsole"></a><a name="security-group-tag-task"></a>

**To tag a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, choose the cluster security group and choose **Manage Tags**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-revoke.png)

1. In the **Manage Tags** dialog box, do one of the following:
   + Remove a tag\.
     + In the **Applied Tags** section, choose **Delete** next to the tag you want to remove\.
     + Choose **Apply Changes**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-remove-tag.png)
   + Add a tag\.
     + In the **Add Tags** section, enter a key\-value pair for the tag\.
     + Choose **Apply Changes**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-add-tag.png)

     For more information about tagging an Amazon Redshift resource, see [How to manage tags in the Amazon Redshift console](rs-mgmt-tagging-console.md#rs-mgmt-console-tags-how-to)\.

## Managing ingress rules for a cluster security group<a name="security-group-modify"></a>

### \(Original console\) Manage ingress rules for a cluster security group<a name="cluster-security-group-ingress-rule-originalconsole"></a><a name="security-group-modify-task"></a>

**To manage ingress rules for a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, in the cluster security group list, choose the cluster security group whose rules you want to manage\.

1. On the **Security Group Connections** tab, choose **Add Connection Type**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-modify-10.png)

1. In the **Add Connection Type** dialog box, do one of the following:
   + Add an ingress rule based on CIDR/IP:
     + In the **Connection Type** box, choose **CIDR/IP**\.
     + In the **CIDR/IP to Authorize** box, specify the range\.
     + Choose **Authorize**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-modify-20.png)
   + Add an ingress rule based on an EC2 security group:
     + Under **Connection Type**, choose **EC2 Security Group**\.
     + Choose the AWS account to use\. By default, the account currently logged into the console is used\. If you choose **Another account**, specify the AWS account ID\. 
     + For **EC2 Security Group Name**, enter the name of the EC2 security group that you want\. 
     + Choose **Authorize**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-modify-30.png)

## Revoking ingress rules for a cluster security group<a name="security-group-revoke"></a>

### \(Original console\) Revoke ingress rules for a cluster security group<a name="cluster-security-group-ingress-rule-revoke-originalconsole"></a><a name="security-group-revoke-task"></a>

**To revoke ingress rules for a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, in the cluster security group list, choose the cluster security group whose rules you want to manage\.

1. On the **Security Group Connections** tab, choose the rule that you want to remove and choose **Revoke**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-revoke.png)

## Tagging ingress rules for a cluster security group<a name="security-rule-tag"></a>

### \(Original console\) Tag ingress rules for a cluster security group<a name="cluster-security-group-ingress-rule-tag-originalconsole"></a><a name="security-rule-tag-task"></a>

**To tag ingress rules for a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, choose the cluster security group whose rules you want to manage\.

1. On the **Security Group Connections** tab, choose the rule that you want to tag and choose **Manage Tags**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-tag-rule.png)

1. In the **Manage Tags** dialog box, do one of the following:
   + Remove a tag:
     + In the **Applied Tags** section, choose **Delete** next to the tag that you want to remove\.
     + Choose **Apply Changes**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-remove-tag.png)
   + Add a tag\.
**Note**  
Tagging an EC2 security group rule only tags that rule, not the EC2 security group itself\. If you want the EC2 security group tagged also, do that separately\.
     + In the **Add Tags** section, enter a key\-value pair for the tag\.
     + Choose **Apply Changes**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-add-tag.png)

   For more information about tagging an Amazon Redshift resource, see [How to manage tags in the Amazon Redshift console](rs-mgmt-tagging-console.md#rs-mgmt-console-tags-how-to)\.

## Deleting a cluster security group<a name="security-group-delete"></a>

If a cluster security group is associated with one or more clusters, you can't delete it\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-security-group-delete"></a>

**To delete a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Security groups** to display the **Cluster security groups** page\. 
**Note**  
You can only manage cluster security groups when logged in with an EC2\-Classic AWS account\.

1. Choose the security group that you want to delete, then choose **Delete**\. 

### Original console<a name="cluster-security-group-create-originalconsole"></a><a name="security-group-delete-task"></a>

**To delete a cluster security group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**\.

1. On the **Security Groups** tab, choose the cluster security group that you want to delete, and then choose **Delete**\.

   One row must be selected for the **Delete** button to be enabled\.
**Note**  
You can't delete the `default` cluster security group\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-delete-10.png)

1. In the **Delete Cluster Security Groups** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-delete-20.png)

   If the cluster security group is used by a cluster, you can't delete it\. The following example shows that `securitygroup1` is used by `examplecluster2`\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/security-group-delete-30.png)

## Associating a cluster security group with a cluster<a name="security-group-associate"></a>

If you are on the EC2\-VPC platform, see [Managing VPC security groups for a cluster](managing-vpc-security-groups.md) for more information about associating VPC security groups with your cluster\. We recommend that you launch your cluster in an EC2\-VPC platform\. However, you can restore an EC2\-Classic snapshot to an EC2\-VPC cluster using the Amazon Redshift console\. For more information, see [Restoring a cluster from a snapshot](managing-snapshots-console.md#snapshot-restore)\.

Each cluster you provision on the EC2\-Classic platform has one or more cluster security groups associated with it\. You can associate a cluster security group with a cluster when you create the cluster, or you can associate a cluster security group later by modifying the cluster\. For more information, see [Creating a cluster by using a launch cluster](managing-clusters-console.md#create-cluster-task) and [To modify a cluster](managing-clusters-console.md#modify-cluster-task)\. 