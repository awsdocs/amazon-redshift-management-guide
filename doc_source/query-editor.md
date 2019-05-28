# Querying a Database Using the Query Editor<a name="query-editor"></a>

Using the Query Editor is the easiest way to run queries on databases hosted by your Amazon Redshift cluster\. After creating your cluster, you can immediately run queries by using the Query Editor on the Amazon Redshift console\.

The following cluster node types support the Query Editor:
+ DC1\.8xlarge
+ DC2\.large
+ DC2\.8xlarge
+ DS2\.8xlarge

Using the Query Editor, you can do the following:
+ Run single SQL statement queries\.
+ Download result sets as large as 100 MB to a comma\-separated value \(CSV\) file\.
+ Save queries for reuse\. You can't save queries in the EU \(Paris\) Region or the Asia Pacific \(Osaka\-Local\) Region\.
+ View query execution details for user\-defined tables\.

## Query Editor Considerations<a name="query-editor-considerations"></a>

Be aware of the following considerations when you use the Query Editor:
+ Up to 50 users can connect to a cluster with the Query Editor at the same time\.
+ The maximum number of users connecting to a cluster includes those connecting through the Query Editor\.
+ Up to 50 workload management \(WLM\) query slots can be active at the same time\. For more information about query slots, see [Implementing Workload Management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)\.
+ Query Editor only runs short queries that can complete within two minutes\. 
+ Query result sets are paginated with 100 rows per page\.
+ You can't use the Query Editor with Enhanced VPC Routing\. For more information, see [Amazon Redshift Enhanced VPC Routing](enhanced-vpc-routing.md)\. 
+ You can't use transactions in the Query Editor\. For more information about transactions, see [BEGIN](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html) in the *Amazon Redshift Database Developer Guide\.*
+ You can save a query up to 3000 characters long\. 

## Enabling Access to the Query Editor<a name="query-cluster-configure"></a>

To access the Query Editor, you need permission\. To enable access, attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies for AWS Identity and Access Management \(IAM\) to the IAM user that you use to access your cluster\.

If you have already created an IAM user to access Amazon Redshift, you can attach the `AmazonRedshiftQueryEditor` and `AmazonRedshiftReadOnlyAccess` policies to that user\. If you haven't created an IAM user yet, create one and attach the policies to the IAM user\.

**Note**  
The AWS managed policy `AmazonRedshiftQueryEditor`, allows the action `redshift:GetClusterCredentials`, which by default gives a database user superuser access to the database\. To restrict access, you can do one of the following items:  
Create a custom policy that allows calling `redshift:GetClusterCredentials` and restricts the resource to a `DbUser`\.
Add a policy to the user that denies permission to `redshift:GetClusterCredentials` and then require users of the Query Editor to sign in with temporary credentials\. For example, the denial policy is similar to this policy:  

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
For more information, see [Create an IAM Role or User With Permissions to Call GetClusterCredentials](generating-iam-credentials-role-permissions.md)\.

**To attach the required IAM policies for the Query Editor**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**\.

1. Choose the user that needs access to the Query Editor\.

1. Choose **Add permissions**\.

1. Choose **Attach existing policies directly**\.

1. For **Policy names**, choose **AmazonRedshiftQueryEditor** and **AmazonRedshiftReadOnlyAccess**\.

1. Choose **Next: Review**\.

1. Choose **Add permissions**\.

## Using the Query Editor<a name="using-query-editor"></a>

 In the following example, you use the Query Editor to perform the following tasks:
+ Run SQL commands\.
+ View query execution details\.
+ Save a query\.
+ Download a query result set\.

To complete the following example, you need an existing Amazon Redshift cluster\. If you don't have a cluster, create one by following the procedure described in [Creating a Cluster](managing-clusters-console.md#create-cluster)\.

**To use the Query Editor**

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