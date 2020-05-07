# Querying a database using the query editor<a name="query-editor"></a>

Using the query editor is the easiest way to run queries on databases hosted by your Amazon Redshift cluster\. After creating your cluster, you can immediately run queries by using the query editor on the Amazon Redshift console\.

The following cluster node types support the query editor:
+ DC1\.8xlarge
+ DC2\.large
+ DC2\.8xlarge
+ DS2\.8xlarge
+ RA3\.4xlarge
+ RA3\.16xlarge

Using the query editor, you can do the following:
+ Run single SQL statement queries\.
+ Download result sets as large as 100 MB to a comma\-separated value \(CSV\) file\.
+ Save queries for reuse\. You can't save queries in the Europe \(Paris\) Region, the Asia Pacific \(Osaka\-Local\) Region, the Asia Pacific \(Hong Kong\) Region, or the Middle East \(Bahrain\) Region\.
+ View query execution details for user\-defined tables\.

## Query editor considerations<a name="query-editor-considerations"></a>

Be aware of the following considerations when you use the query editor on the Amazon Redshift console:
+ Up to 50 users can connect to a cluster with the query editor at the same time\.
+ The maximum number of users connecting to a cluster includes those connecting through the query editor\.
+ Up to 50 workload management \(WLM\) query slots can be active at the same time\. For more information about query slots, see [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)\.
+ The query editor only runs short queries that can complete within 10 minutes\. 
+ Query result sets are paginated with 100 rows per page\.
+ You can't use the query editor with enhanced VPC routing\. For more information, see [Amazon Redshift enhanced VPC routing](enhanced-vpc-routing.md)\. 
+ You can't use transactions in the query editor\. For more information about transactions, see [BEGIN](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html) in the *Amazon Redshift Database Developer Guide\.*
+ You can save a query up to 3000 characters long\. 

## Enabling access to the query editor<a name="query-cluster-configure"></a>

To access the query editor, you need permission\. To enable access, attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies for AWS Identity and Access Management \(IAM\) to the IAM user that you use to access your cluster\.

If you have already created an IAM user to access Amazon Redshift, you can attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies to that user\. If you haven't created an IAM user yet, create one and attach the policies to the IAM user\.

**Note**  
The AWS managed policy `AmazonRedshiftQueryEditor`, allows the action `redshift:GetClusterCredentials`, which by default gives a database user superuser access to the database\. To restrict access, you can do one of the following items:  
Create a custom policy that allows calling `redshift:GetClusterCredentials` and restricts the resource to a `DbUser`\.
Add a policy to the user that denies permission to `redshift:GetClusterCredentials` and then require users of the query editor to sign in with temporary credentials\. For example, the denial policy is similar to this policy:  

  ```
  {
    "Version": "2012-10-17",
    "Statement": {
      "Effect": "Deny",
      "Action": "redshift:GetClusterCredentials",
      "Resource": "*"
    }
  }
  ```
For more information, see [Create an IAM role or user role or user with permissions to call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\.

**To attach the required IAM policies for the query editor**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**\.

1. Choose the user that needs access to the query editor\.

1. Choose **Add permissions**\.

1. Choose **Attach existing policies directly**\.

1. For **Policy names**, choose **AmazonRedshiftQueryEditor** and **AmazonRedshiftReadOnlyAccess**\.

1. Choose **Next: Review**\.

1. Choose **Add permissions**\.

## Using the query editor<a name="using-query-editor"></a>

 In the following example, you use the query editor to perform the following tasks:
+ Run SQL commands\.
+ View query execution details\.
+ Save a query\.
+ Download a query result set\.

To complete the following example, you need an existing Amazon Redshift cluster\. If you don't have a cluster, create one by following the procedure described in [Creating a cluster](managing-clusters-console.md#create-cluster)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="query-editor-use"></a>

**To use the query editor on the Amazon Redshift console**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **EDITOR**, then connect to a database in your cluster\. 

1. For **Schema**, choose **public** to create a new table based on that schema\.

1. Enter the following in the query editor window and choose **Run** to create a new table\.

   ```
   create table shoes(
                   shoetype varchar (10),
                   color varchar(10));
   ```

1. Choose **Clear**\.

1. Enter the following command in the query editor window and choose **Run** to add rows to the table\.

   ```
   insert into shoes values 
   ('loafers', 'brown'),
   ('sandals', 'black');
   ```

1. Choose **Clear**\.

1. Enter the following command in the query editor window and choose **Run** to query the new table\.

   ```
   select * from shoes; 
   ```

   The **Query results** displays the results\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/query-editor.html)

1. Choose **Execution** to view the run details\.

1. Choose **Data**, then **Export** to download the query results as a file\.

### Original console<a name="query-editor-use-originalconsole"></a>

**To use the query editor**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Query Editor**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-qe-overview.png)

1. For **Schema**, choose **public **to create a new table based on that schema\.

1. Enter the following in the Query Editor window and choose **Run query** to create a new table\.

   ```
   create table shoes(
                   shoetype varchar (10),
                   color varchar(10));
   ```

1. Choose **Clear**\.

1. Enter the following command in the Query Editor window and choose **Run query** to add rows to the table\.

   ```
   insert into shoes values 
   ('loafers', 'brown'),
   ('sandals', 'black');
   ```

1. Choose **Clear**\.

1. Enter the following command in the Query Editor window and choose **Run query** to query the new table\.

   ```
   select * from shoes;                                       
   ```

   You should see the following results\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-qe-example1.png)

1. Choose **View execution** to view the execution details\.

1. Choose **Download CSV** to download the query results as a CSV file\.