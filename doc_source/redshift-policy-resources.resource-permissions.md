# Amazon Redshift API Permissions Reference<a name="redshift-policy-resources.resource-permissions"></a>

When you set up [Access Control](redshift-iam-authentication-access-control.md#redshift-iam-accesscontrol) and write permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table includes each Amazon Redshift API operation and the corresponding actions for which you can grant permissions to perform the action\. It also includes the AWS resource for which you can grant the permissions, and condition keys that you can include for fine\-grained access control\. \(For more information about conditions, see [Using IAM Policy Conditions for Fine\-Grained Access Control](redshift-iam-access-control-overview.md#redshift-policy-resources.conditions)\.\) You specify the actions in the policy's `Action` field, the resource value in the policy's `Resource` field, and conditions in the policy's `Condition` field\. 

**Note**  
To specify an action, use the `redshift:` prefix followed by the API operation name \(for example, `redshift:CreateCluster`\)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window \(to close the window, choose the close button \(**X**\) in the lower\-right corner\)\.


**Amazon Redshift API and Required Permissions for Actions**  
<a name="redshift-policy-actions-related-to-objects-table"></a>[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-policy-resources.resource-permissions.html)

Amazon Redshift also supports the following actions that are not based on the Amazon Redshift API: 
+ The `redshift:ViewQueriesInConsole` action controls whether a user can see queries in the Amazon Redshift console in the **Queries** tab of the **Cluster** section\. 
+ The `redshift:CancelQuerySession` action controls whether a user can terminate running queries and loads from the **Cluster** section in the Amazon Redshift console\.
+ The following actions control access to the Query Editor on the Amazon Redshift console:
  + `redshift:ListSchemas`
  + `redshift:ListTables`
  + `redshift:ListDatabases`
  + `redshift:ExecuteQuery`
  + `redshift:FetchResults`
  + `redshift:CancelQuery`
  + `redshift:DescribeQuery`
  + `redshift:DescribeTable`
  + `redshift:ViewQueriesFromConsole`
  + `redshift:ListSavedQueries`
  + `redshift:CreateSavedQuery`
  + `redshift:DeleteSavedQueries`
  + `redshift:ModifySavedQuery`