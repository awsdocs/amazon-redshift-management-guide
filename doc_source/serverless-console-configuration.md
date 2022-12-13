# Managing usage limits, query limits, and other administrative tasks<a name="serverless-console-configuration"></a>

You can configure settings in the console to control usage and limit cost\.

## Managing usage limits, including setting RPU limits<a name="serverless-workgroup-max-rpu"></a>

Under the **Limits** tab for a workgroup, you can add one or more usage limits to control the maximum RPUs you use in a given time period, or to set a data sharing usage limit\.

1. Choose **Manage usage limits**\. Choose **Add limit** on the **Redshift processing units \(RPUs\) maximum** panel\.

1. First, you choose a **Frequency**, which is either **Daily**, **Weekly**, or **Monthly**\. This sets the time period for the limit\. Choosing **Daily** in this instance gives you more detailed control\.

1. Set a usage limit, in number of hours\.

1. Set the action\. These are the following:
   + **Log to system table** \- Adds a record to a system table, which you can query to determine if the limit is exceeded\.
   + **Alert** \- Uses Amazon SNS to set up notification subscriptions and send notifications if a limit is breached\. You can choose an existing Amazon SNS topic or create a new one\.
   + **Turn off user queries** \- Disables queries to stop use of Amazon Redshift Serverless\. It also sends a notification\.

   The first two actions are informational, but the last turns off query processing\.

1. Optionally, you can set a **Cross\-Region data sharing usage limit**, which limits how much data transferred from producer Region to consumer Region consumers can query\. To do this, choose **Add limit** and follow the steps\.

1. Choose **Save changes** to save the limit\.

For more conceptual information about RPUs and billing, see [Billing for Amazon Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-billing.html)\.

## Managing query limits<a name="serverless-workgroup-query-limits"></a>

Under the **Limits** tab for a workgroup, you can add a limit to monitor performance and limits\. For more information about query monitoring limits, see [WLM query monitoring rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\.

1. Choose **Manage query limits**\. Choose **Add new limit** on the **Manage query limits** dialogue\.

1. Choose the limit type you want to set and enter a value for its corresponding limit\.

1. Choose **Save changes** to save the limit\.

When you change your query limit and configuration parameters, your database will restart\.

## Filtering queries<a name="serverless-dashboard-query-summary"></a>

You can use the filters available on the serverless dashboard\. To filter queries, perform the following steps\.

1. On the left of the **Query summary** panel, select the drop\-down list to filter by completed queries, failed queries, or both\.

1. On the right of the **Query summary** panel, select the drop\-down list to filter by running queries, queued queries, or both\.

## Changing your admin password<a name="serverless-console-configuration-admin-password"></a>

1. Choose **Namespace configuration**\. Then choose **Change admin password**\. A dialog appears\.

1. You can specify a **New admin username** and a **New admin user password**\.

1. Choose **Save**\.

## Checking Amazon Redshift Serverless summary data using the dashboard<a name="serverless-dashboard"></a>

The Amazon Redshift Serverless dashboard contains a collection of panels that show at\-a\-glance metrics and information about your workgroup and namespace\. These panels include the following: 
+ **Resources summary** \- Displays high\-level information about Amazon Redshift Serverless, such as the storage used and other metrics\.
+ **Query summary** \- Displays information about queries, including completed queries and running queries\. Choose **View details** to go to a screen that has additional filters\.
+ **RPU capacity used** \- Displays the overall capacity used over a given time period, like the previous ten hours, for instance\.
+ **Datashares** \- Shows the count of datashares, which are used to share data between, for example, AWS accounts\. The metrics show which datashares require authorization, and other information\.

From the dashboard you can quickly dive into these available metrics to check a detail regarding Amazon Redshift Serverless, or review queries, or track work items\.