# Managing resource tags using the console<a name="rs-mgmt-tagging-console"></a>

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="tags-manage"></a>

**To manage tags on your Amazon Redshift resources**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Manage tags**\. 

1. Enter your choices for the resources and choose which tags to add, modify, or delete\. Then choose **Manage tags of the resources that you chose**\. 

   Resources that you can tag include clusters, parameter groups, subnet groups, HSM client certificates, HSM connections, and snapshots\.

1. On the **Manage tags**navigation page, choose **Review and apply tag changes**, then choose **Apply** to save your changes\. 

## Original console<a name="tags-manage-originalconsole"></a>

 The following is an example of the **Manage Tags** window for an Amazon Redshift resource, such as a cluster or a parameter group\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-manage-tags-window.png)

 You use the **Add Tags** section to add key pairs to an Amazon Redshift resource\. When you begin entering a key pair in the **Add Tags** section, a new row will appear so that you can add another key pair, and so on\. For more information about allowed characters for keys and values, see [Tagging requirements](amazon-redshift-tagging.md#rs-tagging-requirements)\.

 If you decide that you don't want to add a particular tag to the resource, you can remove it from the **Add Tags** section by choosing the **X** in the row\. Once you have specified the key pairs that you want to add, you apply the changes so that they are associated with the resource\. 

 After you add key pairs to a resource, they display in the **Applied Tags** section; this is the tag set for the resource\. You can modify a tag value, but you can't modify the key name\. You can, however, delete a key if you no longer need it for the resource\. 

 You can view the tags for a resource by reviewing the **Applied Tags** section of the **Manage Tags** window\. Alternatively, you can quickly view tags by navigating to a resource type in the navigation pane, and then expanding the resource in the list to view the **Tags** section\. The following is an example of a cluster expanded to show various properties, including tags associated with the cluster\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-cluster-tag-list.png)

### How to open the manage tags window<a name="rs-mgmt-open-manage-tags-window"></a>

 The following table describes how to open the **Manage Tags** window for each of the Amazon Redshift resources that support tags\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/rs-mgmt-tagging-console.html)

### How to manage tags in the Amazon Redshift console<a name="rs-mgmt-console-tags-how-to"></a>

Use the table in the previous section to navigate to the resource that you want to work with\. Then use the procedures in this section to add, modify, delete, and view tags for the resource\. <a name="rs-mgmt-console-add-tags"></a>

**To add tags to a resource**

1. Navigate to the resource to which you want to add tags, and open the **Manage Tags** window\.

1.  Under **Add Tags**, type a key name in the **Key** box and the key value in the **Value** box\. For example, type `environment` in the **Key** box and `production` in the **Value** box\. Repeat this step to add any additional tags\. 

1.  Choose **Apply Changes**\. <a name="rs-mgmt-console-modify-tags"></a>

**To modify tags associated with a resource**

1. Navigate to the resource for which you want to modify tags, and open the **Manage Tags** window\.

1.  Under **Applied Tags**, locate the key that you want to modify\. In the **Value** box, type a new key value\. Repeat for any other tags that you want to modify\. 

1.  Choose **Apply Changes**\. <a name="rs-mgmt-console-delete-tags"></a>

**To delete tags associated with a resource**

1. Navigate to the resource from which you want to delete tags, and open the **Manage Tags** window\.

1.  Under **Applied Tags**, locate the key that you want to delete\. Select the **Delete** check box\. Repeat for any other tags that you want to delete\. 

1.  Choose **Apply Changes**\. 