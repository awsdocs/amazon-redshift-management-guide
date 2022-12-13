# Collaborating and sharing as a team<a name="query-editor-v2-team"></a>

You can share queries with your team\. 

A team is defined for a set of users who collaborate and share query editor v2 resources\. An administrator can create a team by adding a tag to an IAM role\. For more information, see [Permissions required to use the query editor v2 ](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor-v2)\. 

## Saving, browsing for, and deleting queries<a name="query-editor-v2-save-delete-queries"></a>

Before you can share your query with your team, save your query\. You can view and delete saved queries\. 

**To save a query**

1. Prepare your query and choose **Save**\.

1. Enter a title for your query\.

1. Choose **Save**\. 

**To browse for saved queries**

1. Choose **Queries** from the navigation pane\.

1. You can view queries that are **My queries**, **Shared by me**, or **Shared to my team**\. These queries can appear as individual queries or within folders you created\.

**To delete a saved query**

1. Open the context \(right\-click\) menu for a saved query\.

1. Choose **Delete** and confirm the action\. 

**To organize your saved queries into folders**

1. Choose **Queries** from the navigation pane\.

1. Choose **New folder** and name the folder\. 

1. Choose **Create** to create the folder in the **Queries** tab\. 

   You can now move queries in and out of the folder using drag\-and\-drop\.

## Sharing a query<a name="query-editor-v2-query-share"></a>

You can share your queries with your team\. You can also view the history of saved queries and manage query versions\. 

To share a query with your team, make sure that you have the principal tag `sqlworkbench-team` set to the same value as the rest of your team members in your account\. For example, an administrator might set the value to `accounting-team` for everyone in the accounting department\.  For an example, see [Permissions required to use the query editor v2 ](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor-v2)\.

**To share a query with a team**

1. Choose **Queries** from the navigation pane\.

1. Open the context \(right\-click\) menu of the query that you want to share and choose **Share with my team**\.

1. Choose the team or teams that you want to share the query with and then choose **Save sharing options**\. 

Every time you save a SQL query, the query editor v2 saves it as a new version\. You can browse earlier query versions, save a copy of a query, or restore a query\. 

**To manage query versions**

1. Choose **Queries** from the navigation pane\.

1. Open the context \(right\-click\) menu for the query that you want to work with\.

1. Choose **Version history** to open a list of versions of the query\.

1. On the **Version history** page, you can do the following:
   + **Revert to selected** – Revert to the selected version and continue your work with this version\.
   + **Save selected as** – Create a new query in the editor\.