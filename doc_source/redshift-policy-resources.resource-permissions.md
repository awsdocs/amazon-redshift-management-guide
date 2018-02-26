# Amazon Redshift API Permissions Reference<a name="redshift-policy-resources.resource-permissions"></a>

When you are setting up [Access Control](redshift-iam-authentication-access-control.md#redshift-iam-accesscontrol) and writing permissions policies that you can attach to an IAM identity \(identity\-based policies\), you can use the following table as a reference\. The table lists each Amazon Redshift API operation, the corresponding actions for which you can grant permissions to perform the action, the AWS resource for which you can grant the permissions, and condition keys that you can include for fine\-grained access control \(for more information about conditions, see [Using IAM Policy Conditions for Fine\-Grained Access Control](redshift-iam-access-control-overview.md#redshift-policy-resources.conditions)\)\. You specify the actions in the policy's `Action` field, the resource value in the policy's `Resource` field, and conditions in the policy's `Condition` field\. 

**Note**  
To specify an action, use the `redshift:` prefix followed by the API operation name \(for example, `redshift:CreateCluster`\)\.

If you see an expand arrow \(**↗**\) in the upper\-right corner of the table, you can open the table in a new window \(to close the window, choose the close button \(**X**\) in the lower\-right corner\)\.


**Amazon Redshift API and Required Permissions for Actions**  
<a name="redshift-policy-actions-related-to-objects-table"></a>[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/redshift-policy-resources.resource-permissions.html)

Redshift also supports the following actions that are not based on the Amazon Redshift API: 

+ The `redshift:ViewQueriesInConsole` action controls whether a user can see queries in the Amazon Redshift console in the **Queries** tab of the **Cluster** section\. 

+ The `redshift:CancelQuerySession` action controls whether a user can terminate running queries and loads from the **Cluster** section in the Amazon Redshift console\.