# Authorizing Amazon Redshift to Access Other AWS Services on Your Behalf<a name="authorizing-redshift-service"></a>

Some Amazon Redshift features require Amazon Redshift to access other AWS services on your behalf\. For example, the [COPY](http://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) and [UNLOAD](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) commands can load or unload data into your Amazon Redshift cluster using an Amazon Simple Storage Service \(Amazon S3\) bucket\. In order for your Amazon Redshift clusters to act on your behalf, you must supply security credentials to your clusters\. The security credentials can be AWS access keys, or you can use the preferred method of specifying an AWS Identity and Access Management \(IAM\) role\. 

This section describes how to create an IAM role with the appropriate permissions to access other AWS services\. You will also need to associate the role with your cluster and specify the Amazon Resource Name \(ARN\) of the role when you execute the Amazon Redshift command\. For more information, see [Authorizing COPY and UNLOAD Operations Using IAM Roles](copy-unload-iam-role.md)\.

## Creating an IAM Role to Allow Your Amazon Redshift Cluster to Access AWS Services<a name="authorizing-redshift-service-creating-an-iam-role"></a>

To create an IAM role to permit your Amazon Redshift cluster to communicate with other AWS services on your behalf, take the following steps\.

**To create an IAM role to allow Amazon Redshift to access AWS services**

1. Open the [IAM Console](https://console.aws.amazon.com/iam/home?#home)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. Choose **AWS service**, and then choose **Redshift**\.

1. Under **Select your use case**, choose **Redshift \- Customizable** and then choose **Next: Permissions**\.

1. The **Attach permissions policy** page appears\. Choose **AmazonS3ReadOnlyAccess** and **AWSGlueConsoleFullAccess**\. Choose **Next: Review**\.

1. For **Role name**, type a name for your role, for example **RedshiftCopyUnload**\. Choose ****Create role****\.

1. The new role is available to all users on clusters that use the role\. To restrict access to only specific users on specific clusters, or to clusters in specific regions, edit the trust relationship for the role\. For more information, see [Restricting Access to IAM Roles](#authorizing-redshift-service-database-users)\.

1. Associate the role with your cluster\. You can associate an IAM role with a cluster when you create the cluster, or you add the role to an existing cluster\. For more information, see [Associating IAM Roles with Clusters](copy-unload-iam-role.md#copy-unload-iam-role-associating-with-clusters)\.

## Restricting Access to IAM Roles<a name="authorizing-redshift-service-database-users"></a>

By default, IAM roles that are available to an Amazon Redshift cluster are available to all users on that cluster\. You can choose to restrict IAM roles to specific Amazon Redshift database users on specific clusters or to specific regions\. 

To permit only specific database users to use an IAM role, take the following steps\.

**To identify specific database users with access to an IAM role**

1. Identify the Amazon Resource Name \(ARN\) for the database users in your Amazon Redshift cluster\. The ARN for a database user is in the format: `arn:aws:redshift:region:account-id:dbuser:cluster-name/user-name`\.

1. Open the [IAM Console](https://console.aws.amazon.com/iam/home?#home) at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the IAM role that you want to restrict to specific Amazon Redshift database users\.

1. Choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\. A new IAM role that allows Amazon Redshift to access other AWS services on your behalf will have a trust relationship as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "redshift.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Add a condition to the `sts:AssumeRole` action section of the trust relationship that limits the `sts:ExternalId` field to values that you specify\. Include an ARN for each database user that you want to grant access to the role\.

   For example, the following trust relationship specifies that only database users `user1` and `user2` on cluster `my-cluster` in region `us-west-2` have permission to use this IAM role\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
     {
       "Effect": "Allow",
       "Principal": { 
         "Service": "redshift.amazonaws.com" 
       },
       "Action": "sts:AssumeRole",
       "Condition": {
         "StringEquals": {
           "sts:ExternalId": [
             "arn:aws:redshift:us-west-2:123456789012:dbuser:my-cluster/user1",
             "arn:aws:redshift:us-west-2:123456789012:dbuser:my-cluster/user2"
           ]
         }
       }
     }]
   }
   ```

1. Choose **Update Trust Policy**\.

## Restricting an IAM Role to an AWS Region<a name="authorizing-redshift-service-regions"></a>

You can restrict an IAM role to only be accessible in a certain AWS Region\. By default, IAM roles for Amazon Redshift are not restricted to any single region\.

To restrict use of an IAM role by region, take the following steps\.

**To identify permitted regions for an IAM role**

1. Open the [IAM Console](https://console.aws.amazon.com/iam/home?#home) at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the role that you want to modify with specific regions\.

1. Choose the **Trust Relationships** tab and then choose **Edit Trust Relationship**\. A new IAM role that allows Amazon Redshift to access other AWS services on your behalf will have a trust relationship as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "redshift.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Modify the `Service` list for the `Principal` with the list of the specific regions that you want to permit use of the role for\. Each region in the `Service` list must be in the following format: `redshift.region.amazonaws.com`\.

   For example, the following edited trust relationship permits the use of the IAM role in the `us-east-1` and `us-west-2` regions only\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "redshift.us-east-1.amazonaws.com",
             "redshift.us-west-2.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**

## Related Topics<a name="authorizing-redshift-related-topic"></a>

+ [Authorizing COPY and UNLOAD Operations Using IAM Roles](copy-unload-iam-role.md)