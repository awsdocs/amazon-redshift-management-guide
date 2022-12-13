# Authoring and running notebooks<a name="query-editor-v2-notebooks"></a>

You can use notebooks to organize, annotate, and share multiple SQL queries in a single document\. You can add multiple SQL query and Markdown cells to a notebook\. Notebooks provide a way to group queries and explanations associated with a data analysis in a single document by using multiple query and Markdown cells\. You can add text and format the appearance using Markdown syntax to provide context and additional information for your data analysis tasks\. You can share your notebooks with team members\.

To use notebooks, you must add permission for notebooks to your IAM principal \(an IAM user or IAM role\)\. You can add the permission to one of the query editor v2 managed policies\. For more information, see [Accessing the query editor v2](query-editor-v2-getting-started.md#query-editor-v2-configure)\.

You can **Run all** the cells of a notebook sequentially\. The SQL query cell of a notebook has most of the same capabilities as a query editor tab\. For more information, see [Authoring and running queries](query-editor-v2-query-run.md)\. The following are differences between a query editor tab and a SQL cell in a notebook\.
+ There isn't a control to run `Explain` on a SQL statement in a notebook\.
+ You can create only one chart per SQL cell in a notebook\.

You can export and import notebooks to files created with query editor v2\. The file extension is `.ipynb` and the file size can be a maximum of 5 MB\. The SQL and Markdown cells are stored in the file\. A cluster or workgroup and database is not stored in the exported notebook\. When you open an imported notebook you choose the cluster or workgroup and the database where to run it\. After running SQL cells, you can then choose in the results tab whether to display the current page of results as a chart\. The result set of a query is not stored in the notebook\.

**To create a notebook**

1. From the navigator menu, choose ![\[Databases\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-database.png) **Database**\.

1. Choose ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) and then choose **Notebook**\.

   By default, a SQL query cell appears in the notebook\.

1. In the SQL query cell, do one of the following:
   + Enter a query\.
   + Paste a query that you copied\.

1. \(Optionally\) Choose the ![\[New Markdown cell\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) icon, then choose **Markdown** to add a Markdown cell where you can provide descriptive or explanatory text using standard Markdown syntax\. 

1. \(Optionally\) Choose the ![\[New SQL cell\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) icon, then choose **SQL** to insert a SQL cell\. 

You can rename notebooks using the ![\[Rename\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-edit.png) \(pencil\) icon\.

From the ![\[More\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-more.png) \(more\) menu, you can also perform the following operations on a notebook:
+ ![\[Share\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-share.png) **Share with my team** – To share the notebook with your team as defined by tags\. For more information, see [Sharing a query](query-editor-v2-team.md#query-editor-v2-query-share)
+ ![\[Export\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-export.png) **Export** – To export the notebook to a local file with the `.ipynb` extension\.
+  ![\[Save\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-floppy-disk.png) **Save version** – To create a version of the notebook\. To see versions of a notebook, navigate to your saved notebooks and open **Version history**\.
+  ![\[Duplicate\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-duplicate.png) **Duplicate** – To create a copy of the notebook and open it in a new notebook tab\. 
+  ![\[Shortcuts\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-key-command.png) **Shortcuts** – To display the shortcuts available when authoring a notebook\. 



**To open a saved notebook**

1. From the navigator menu, choose ![\[Notebooks\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-manual.png) **Notebooks**\. Your saved notebooks and notebook folders are displayed\.

1. Choose the notebook that you want to open and double\-click it\.

You can show **My notebooks**, notebooks that are **Shared by me**, and notebooks that are **Shared to my team** within the notebooks tab\.

To import a notebook from a local file to **My notebooks**, choose ![\[Import\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-import.png) **Import**, then navigate to the `.ipynb` file that contains your notebook\. The notebook is imported into the currently open notebook folder\. You can then open the notebook in the notebook editor\.

From the context menu \(right\-click\) of a notebook you can perform the following operations:
+ **Open notebook** – To open the notebook in the editor\.
+ **Save version** – To save a version of the notebook\.
+ **Version history** – To display the versions of a notebook\. From the **Version history** window you can delete and revert versions\. You can also create a notebook from the currently selected version\.
+ **Edit tags** – To create and edit tags on a notebook\.
+ **Share with my team** – To share a notebook with your team\.

  To share a notebook with your team, make sure that you have the principal tag `sqlworkbench-team` set to the same value as the rest of your team members in your account\. For example, an administrator might set the value to `accounting-team` for everyone in the accounting department\.  For an example, see [Permissions required to use the query editor v2 ](redshift-iam-access-control-identity-based.md#redshift-policy-resources.required-permissions.query-editor-v2)\.
+ **Export** – To export a notebook to a local file\.
+ **Rename** – To rename a notebook\.
+ **Duplicate** – To make a copy of a notebook\.
+ **Delete** – To delete a notebook\.

For a demo of notebooks, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/GNahyu7j98M/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/GNahyu7j98M)