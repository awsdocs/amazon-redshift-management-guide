# Querying the AWS Glue Data Catalog \(preview\)<a name="query-editor-v2-glue"></a>


|  | 
| --- |
| This is prerelease documentation for query AWS Glue, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only in test environments, and not in production environments\. Public preview will end on May 1, 2023\. Preview clusters and preview serverless workgroups and namespaces will be removed automatically two weeks after the end of the preview\. For preview terms and conditions, see Betas and Previews in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

You can use query editor v2 to query data cataloged in AWS Glue Data Catalog\. This feature is only available in preview clusters and preview workgroups\.

**Note**  
You can create an Amazon Redshift cluster in **Preview** to test new features of Amazon Redshift\. You can't use those features in production or move your **Preview** cluster to a production cluster or a cluster on another track\. For preview terms and conditions, see *Beta and Previews* in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.  
Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.
On the navigation menu, choose **Provisioned clusters dashboard**, and choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.
A banner displays on the **Clusters** list page that introduces preview\. Choose the button **Create preview cluster** to open the create cluster page\.
Enter properties for your cluster\. Choose the **Preview track** that contains the features you want to test\. We recommend entering a name for the cluster that indicates that it is on a preview track\. Choose options for your cluster, including options labeled as **\-preview**, for the features you want to test\. For general information about creating clusters, see [Creating a cluster](https://docs.aws.amazon.com/redshift/latest/mgmt/managing-clusters-console.html#create-cluster) in the *Amazon Redshift Management Guide*\.
Choose **Create preview cluster** to create a cluster in preview\.
When your preview cluster is available, use your SQL client to load and query data\.
Your cluster must be created with the preview track named: `preview_2022`\. Use a new cluster for testing, restoring a cluster into this track is not supported\.  
You can create an Amazon Redshift Serverless workgroup in **Preview** to test new features of Amazon Redshift Serverless\. You can't use those features in production or move your **Preview** workgroup to a production workgroup\. For preview terms and conditions, see *Beta and Previews* in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.  
Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.
On the navigation menu, choose **Severless dashboard**, and choose **Workgroup configuration**\. The workgroups for your account in the current AWS Region are listed\. A subset of properties of each workgroup is displayed in columns in the list\.
A banner displays on the **Workgroups** list page that introduces preview\. Choose the button **Create preview workgroup** to open the create workgroup page\.
Enter properties for your workgroup\. We recommend entering a name for the workgroup that indicates that it is in preview\. Choose options for your workgroup, including options labeled as **\-preview**, for the features you want to test\. Continue through the pages to enter options for your workgroup and namespace\. For general information about creating workgroups, see [Creating a workgroup with a namespace](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-console-workgroups-create-workgroup-wizard.html) in the *Amazon Redshift Management Guide*\.
Choose **Create preview workgroup** to create a workgroup in preview\.
When your preview workgroup is available, use your SQL client to load and query data\.
This preview is available in the following AWS Regions:  
US East \(Ohio\) Region \(us\-east\-2\)
US East \(N\. Virginia\) Region \(us\-east\-1\)
US West \(Oregon\) Region \(us\-west\-2\) 
Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)
Europe \(Stockholm\) Region \(eu\-north\-1\)
Europe \(Ireland\) Region \(eu\-west\-1\)

**To grant your IAM user or role permission to query the AWS Glue Data Catalog, follow these steps**

1. In the tree\-view pane, connect to your initial database in your preview cluster or preview workgroup using the **Database user name and password** authentication method\. For example, connect to the `dev` database using the admin user and password you used when you created the preview cluster or preview workgroup\.

1. In an editor tab, run the following SQL statement to grant an IAM user access to the AWS Glue Data Catalog\.

   ```
   GRANT USAGE ON DATABASE awsdatacatalog to "IAM:myIAMUser"
   ```

   Where *IAM:myIAMUser* is an IAM user that you want to grant usage privilege to the AWS Glue Data Catalog\. Alternatively, you can grant usage privilege to *IAMR:myIAMRole* for an IAM role\.

1. In the tree\-view pane, edit or delete the connection to the cluster or workgroup you previously created\. Connect to either your preview cluster or preview workgroup in one of the following ways:
   + To connect to the `awsdatacatalog` database from a preview cluster, you must use the authentication method **Temporary credentials using your IAM identity**\. For more information about this authentication method, see [Connecting to an Amazon Redshift database](query-editor-v2-using.md#query-editor-v2-connecting)\. Your query editor v2 administrator might need to configure the **Account settings** for the account to display this authentication methods on the connection window\.
   + To connect to the `awsdatacatalog` database from a preview workgroup, you must use the authentication method **Federated user**\. For more information about this authentication method, see [Connecting to an Amazon Redshift database](query-editor-v2-using.md#query-editor-v2-connecting)\.

1. With the granted privilege, you can use your IAM identity to run SQL against your AWS Glue Data Catalog\.

To see information about the AWS Glue database objects cataloged in Amazon Redshift, you can query the following views\. For more information about Amazon Redshift views, see [System views](https://docs.aws.amazon.com/redshift/latest/dg/c_intro_system_views.html) in the *Amazon Redshift Database Developer Guide*\.
+ [SVV\_EXTERNAL\_DATABASES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_DATABASES.html) – contains information about the `awsdatacatalog` database that points to the AWS Glue Data Catalog\.
+ [SVV\_ALL\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_SCHEMAS.html) – contains information about the AWS Glue databases that are catalog as schemas in Amazon Redshift\.
+ [SVV\_ALL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_TABLES.html) and [SVV\_EXTERNAL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_TABLES.html) – contains information about the AWS Glue tables\.
+ [SVV\_ALL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_COLUMNS.html) and [SVV\_EXTERNAL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_COLUMNS.html) – contains information about the AWS Glue columns\.

After connecting, you can use query editor v2 to query data cataloged in AWS Glue Data Catalog\. On the query editor v2 tree\-view pane, choose the cluster or workgroup and `awsdatacatalog` database\. In the editor or notebook pane, confirm the correct cluster or workgroup is chosen\. The database chosen should be the initial Amazon Redshift database such as `dev`\. For information about authoring queries, see [Authoring and running queries](query-editor-v2-query-run.md) and [Authoring and running notebooks](query-editor-v2-notebooks.md)\. The database named `awsdatacatalog` is reserved to reference the external Data Catalog database in your account\. Queries against the `awsdatacatalog` database can only be read\-only\. Use three\-part notation to reference the table in your SELECT statement\. Where the first part is the database name, the second part is the AWS Glue database name, and the third part is the AWS Glue table name\.

```
SELECT * FROM awsdatacatalog.<aws-glue-db-name>.<aws-glue-table-name>;
```

You can perform various scenarios that read the AWS Glue Data Catalog data and populate Amazon Redshift tables\.

The following example SQL joins two tables that are defined in AWS Glue\.

```
SELECT pn.emp_id, alias, role, project_name 
FROM "awsdatacatalog"."empl_db"."project_name_table" pn, 
"awsdatacatalog"."empl_db"."project_alias_table" pa
WHERE pn.emp_id = pa.emp_id;
```

The following example SQL creates an Amazon Redshift table and populates it with data from a join of two AWS Glue tables\.

```
CREATE TABLE dev.public.cranberry AS
SELECT pn.emp_id, alias, role, project_name 
FROM "awsdatacatalog"."empl_db"."project_name_table" pn, 
"awsdatacatalog"."empl_db"."project_alias_table" pa
WHERE pn.emp_id = pa.emp_id;
```