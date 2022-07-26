# Querying a data lake<a name="query-editor-v2-querying-data-lake"></a>

You can query data in an Amazon S3 data lake\. First, you create an external schema to reference the external database in the [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/components-overview.html#data-catalog-intro)\. Then, you can query data in the Amazon S3 data lake\.

## Demo: Query a data lake<a name="query-editor-v2-example-data-lake-demo"></a>

To learn how to query a data lake, watch the following video\.

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/-pyy0qNmEKo/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/-pyy0qNmEKo)

## Prerequisites<a name="query-editor-v2-querying-data-lake-prerequisites"></a>



## Creating an IAM role<a name="query-editor-v2-create-role-lake-formation"></a>

You create an IAM role to access an AWS Glue Data Catalog enabled for AWS Lake Formation\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

   If this is your first time choosing **Policies**, the **Welcome to Managed Policies** page appears\. Choose **Get Started**\.

1. Choose **Create policy**\. 

1. Choose to create the policy on the **JSON** tab\. 

1. Paste in the following JSON policy document, which grants access to the Data Catalog but denies the administrator permissions for Lake Formation\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "RedshiftPolicyForLF",
               "Effect": "Allow",
               "Action": [
                   "glue:*",
                   "lakeformation:GetDataAccess"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. When you are finished, choose **Review** to review the policy\. The policy validator reports any syntax errors\.

1. On the **Review policy** page, for **Name** enter a name for the policy that you are creating, for example, *mydatalake\_policy*\. Enter a **Description** \(optional\)\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

   After you create a policy, you can create a role and apply the policy\. 

1. In the navigation pane of the IAM console, choose **Roles**, and then choose **Create role**\.

1. For **Select type of trusted entity**, choose **AWS service**\.

1. Choose the Amazon Redshift service to assume this role\.

1. Choose the **Redshift Customizable** use case for your service\. Then choose **Next: Permissions**\.

1. Choose the permissions policy that you created, `mydatalake_policy`, to attach to the role\.

1. Choose **Next: Tagging**\.

1. Choose **Next: Review**\. 

1. For **Role name**, enter a name for the role, for example, *mydatalake\_role*\. 

1. \(Optional\) For **Role description**, enter a description for the new role\.

1. Review the role, and then choose **Create role**\.

## Granting permissions<a name="query-editor-v2-grant-lake-formation-table"></a>

You grant SELECT permissions on the table to query in the Lake Formation database\.

1. Open the Lake Formation console at [https://console\.aws\.amazon\.com/lakeformation/](https://console.aws.amazon.com/lakeformation/)\.

1. In the navigation pane, choose **Permissions**, and then choose **Grant**\.

1. Provide the following information:
   + For **IAM role**, choose the IAM role you created, `myspectrum_role`\. When you run the Amazon Redshift Query Editor, it uses this IAM role for permission to the data\. 
**Note**  
To grant SELECT permission on the table in a Lake Formation–enabled Data Catalog to query, do the following:  
Register the path for the data in Lake Formation\. 
Grant users permission to that path in Lake Formation\. 
Created tables can be found in the path registered in Lake Formation\. 
   + For **Database**, choose your Lake Formation database\. 
   + For **Table**, choose a table within the database to query\. 
   + For **Columns**, choose **All Columns**\.
   + Choose the **Select** permission\.

1. Choose **Save**\.

**Important**  
As a best practice, allow access only to the underlying Amazon S3 objects through Lake Formation permissions\. To prevent unapproved access, remove any permission granted to Amazon S3 objects outside of Lake Formation\. If you previously accessed Amazon S3 objects before setting up Lake Formation, remove any IAM policies or bucket permissions that previously were set up\. For more information, see [Upgrading AWS Glue Data Permissions to the AWS Lake Formation Model](https://docs.aws.amazon.com/lake-formation/latest/dg/upgrade-glue-lake-formation.html) and [Lake Formation Permissions](https://docs.aws.amazon.com/lake-formation/latest/dg/lake-formation-permissions.html)\. 

## Creating the external schema<a name="query-editor-v2-create-external-schema"></a>

To query data in an Amazon S3 data lake, you create an external schema\. The external schema references the external database in the [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/components-overview.html#data-catalog-intro)\.

1. Choose ![\[Create\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/query-editor-new-tab.png)**Create**, and then choose **Schema**\.

1. Enter a schema name\.

1. To grant ownership of the database to a user, choose **Authorize user** and choose a user\. 

1. Choose **External**\.

1. Under **AWS Glue Data Catalog** details, Region defaults to the Region where your Redshift database is located\.

1. Choose the **AWS Glue database** that the external schema will map to\.

1. Choose an **IAM role** that has the required permissions to query data on Amazon S3\.

1. Choose **Create schema**\.

   The schema appears in the database browser\.

## Querying data in your Amazon S3 data lake<a name="query-editor-v2-query-data-lake"></a>

You use the schema that you created in the previous procedure\. 

1. In the database browser, choose the schema\.

1. To view a table definition, choose a table\.

   The table columns and data types display\.

1. To query a table, choose the table and use the context menu \(right\-click\) to choose **Select table**\.