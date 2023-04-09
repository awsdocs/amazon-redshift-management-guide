# Managing Multi\-AZ using the console<a name="multi-az-console"></a>

You can manage the settings for Multi\-AZ using the Amazon Redshift console\.

## Setting up Multi\-AZ when creating a new cluster<a name="create-cluster-multi-az"></a>

Use the following procedure to set up Multi\-AZ deployment when creating a new cluster\. 

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Provisioned clusters dashboard**, and choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. A banner displays on the **Clusters** list page that introduces preview mode\. Choose the button **Create preview cluster** to open the create cluster page\.

1. Enter properties for your cluster\. Choose the **Preview track** that contains the features you want to test\. Your cluster must be created with the preview track named **preview\_2022**\. We recommend entering a name for the cluster that indicates that it is on a preview track\. Choose options for your cluster, including options labeled as **\-preview**, for the features you want to test\. For general information about creating clusters, see [Creating a cluster](managing-clusters-console.md#create-cluster)\.

1. Choose one of the RA3 node types from the **Node type** drop\-down list\. The Multi\-AZ deployment option becomes available only when you chose an RA3 node type\.

1. Under **Multi\-AZ deployment**, choose **Yes**\.

1. Under **Number of nodes per AZ**, enter the number of nodes that you need for your cluster\.

1. Optionally, do one of the following to load sample data or bring your own data:
   + In **Sample data**, choose **Load sample data** to load the sample dataset into your Amazon Redshift cluster\. Amazon Redshift loads the sample dataset Tickit into the default dev database and public schema\. Amazon Redshift automatically loads the sample dataset into your Amazon Redshift cluster\. You can start using the query editor v2 to query data\.
   + To bring your own data to your Amazon Redshift cluster, follow the steps in [Bringing your own data to Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/bring-own-data.html)\.

1. Scroll down to **Additional configurations**, expand **Network and security**, and make sure that you either accept the default **Cluster subnet group** or choose another one\. If you choose another cluster subnet group, make sure that there are 3 Availability Zones in the subnet group you selected\.

1. Under **Additional configurations**, expand **Database configurations**\.

1. Under **Database encryption**, to use a custom KMS key other than the default AWS Key Management Service key, click **Customize encryption settings\. This option is deselected by default\.**

1. Under **Choose an KMS key**, you can either choose an AWS Key Management Service key or enter an ARN\. Or, you can click **Create an AWS Key Management Service key** in the AWS Key Management Service console\. For more information about creating KMS keys, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.

1. Click **Create cluster**\. When the cluster creation succeeds, you can view the details in the cluster details page\. You can use your SQL client to load and query data\.

## Setting up Multi\-AZ for a cluster restored from a snapshot<a name="restore-cluster-multi-az"></a>

Use the following procedure to set up Multi\-AZ for a cluster restored from a snapshot\.

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, then choose the snapshot to use\.

1. Choose **Restore snapshot**, **Restore to a provisioned cluster**\.

1. Enter properties for your cluster\. Choose **Preview**\. Then, choose the **Preview track** that contains the features you want to test\. We recommend entering a name for the cluster that indicates that it is on a preview track\. Choose options for your cluster, including options labeled as **\-preview**, for the features you want to test\. For general information about creating clusters, see [Creating a cluster](managing-clusters-console.md#create-cluster)\.

1. Choose one of the RA3 node types from the **Node type** drop\-down list\. The Multi\-AZ deployment option becomes available only when you chose an RA3 node type\.

1. Make sure that you choose one of the RA3 node types from the **Node type** drop\-down list\. The Multi\-AZ deployment option becomes available only when you chose an RA3 node type\.

1. Under **Multi\-AZ deployment**, choose **Yes**\.

1. Under **Number of nodes per AZ**, enter the number of nodes that you need for your cluster\.

1. Optionally, do one of the following to load sample data or bring your own data:
   + In **Sample data**, choose **Load sample data** to load the sample dataset into your Amazon Redshift cluster\. Amazon Redshift loads the sample dataset Tickit into the default dev database and public schema\. Amazon Redshift automatically loads the sample dataset into your Amazon Redshift cluster\. You can start using the query editor v2 to query data\.
   + To bring your own data to your Amazon Redshift cluster, follow the steps in [Bringing your own data to Amazon Redshift](https://docs.aws.amazon.com/https://docs.aws.amazon.com/redshift/latest/dg/bring-own-data.html)\.

1. Scroll down to **Additional configurations**, expand **Network and security**, and make sure that you either accept the default **Cluster subnet group** or choose another one\. If you choose another cluster subnet group, make sure that there are 3 Availability Zones in the subnet group you selected\.

1. Under **Additional configurations**, expand **Database configurations**\.

1. Under **Database encryption**, to use a custom KMS key other than the default AWS Key Management Service key, click **Customize encryption settings**\. This option is deselected by default\.

1. Under **Choose an KMS key**, you can either choose an AWS Key Management Service key or enter an ARN\. Or, you can click **Create an AWS Key Management Service key** in the AWS Key Management Service console\. For more information about creating KMS keys, see [Creating Keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.

1. Click **Restore cluster from snapshot**\. When the cluster restoration succeeds, you can view the details in the cluster details page\.

## Testing Multi\-AZ fault tolerance \(optional\)<a name="test-cluster-multi-az"></a>

Use the following procedure to test Multi\-AZ fault tolerance of your Amazon Redshift data warehouse using the **Inject failure** option\.

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Do one of the following:
   + On the navigation menu, choose **Clusters**\. Under **Clusters**, choose a cluster\. The cluster details page appears\.
   + From the cluster dashboard, choose a cluster\.

1. From **Actions**, choose **Inject failure \(Public preview\)**\.

1. When prompted, click **Confirm**\.

Amazon Redshift injects a failure into your Multi\-AZ cluster\. It will cause compute nodes in one Availability Zone to become unavailable\. Amazon Redshift detects this event and starts an automatic recovery\. When the cluster recovery successfully completes, Multi\-AZ becomes available\. Your Multi\-AZ cluster will also automatically provision new compute nodes in another Availability Zone as soon as it is available\.

During this process, the cluster status on the console shows as modifying for the entire time, as the cluster automatically recovers and reconfigures back to the Multi\-AZ deployment setup\. The cluster can accept new connections immediately\. Existing connections and inflight queries might be dropped\. You can retry them immediately\.

## Viewing queries and loads for Multi\-AZ clusters<a name="viewing-multi-az-queries-loads"></a>

The information shown on the Queries and loads page is populated with information from Amazon Redshift system tables \(SYS\_\* views\)\. This information lets you display additional information about your queries and offers rolling 7 days of retention\. You can view information on queries that ran in the past 7 days irrespective of the type, size, and status \(pause or resume\) of your cluster\. Query diagnostics become faster, letting you filter data by database, username, or SQL statement type\. To see these additional filters and information on all queries that ran, note the following prerequisites:
+ You must connect to a database by choosing **Connect to database**\.
+ Your database user must have the sys:operator or sys:monitor roles and permissions to perform query monitoring\. For information about system roles, see [Amazon Redshift system\-defined roles](https://docs.aws.amazon.com/redshift/latest/dg/r_roles-default.html) in the *Amazon Redshift Database Developer Guide*\.

You will see these additional filters and query information once you connect to a database\.

**To display query performance data from Queries and loads**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Queries and loads** to display the list of queries for your account\. 

1. You might have to connect to a database to see additional filter\. If required, click **Connect to database** and follow the prompts to connect to a database\.

   By default, the list displays queries for all your clusters over the past 24 hours\. You can change the scope of the displayed date in the console\. 

**To display query performance data from Query monitoring**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. Under **Clusters**, select a cluster\. 

1. Choose **Query monitoring**\.

1. Depending on the configuration or version of your cluster, you might have to connect to a database to see additional filters\. If required, click **Connect to database** and follow the prompts to connect to a database\.

## Monitoring a query in a Multi\-AZ deployment<a name="monitoring-multi-az-query"></a>

A Multi\-AZ deployment uses compute resources that are deployed in both Availability Zones and can continue operating in the event that the resources in a given Availability Zone aren't available\. All the compute resources will be used at all times\. This allows full operation across two Availability Zones in an active\-active fashion for both read and write operations\.

You can query SYS\_ views in pg\_catalog schema to monitor query runtime in a Multi\-AZ deployment\. The SYS\_ views display query runtime activities or statistics from primary and secondary clusters\. Following are the system tables in SYS\_ view list:
+ [SYS\_QUERY\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_QUERY_HISTORY.html)
+ [SYS\_QUERY\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_QUERY_DETAIL.html)
+ [SYS\_EXTERNAL\_QUERY\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_QUERY_DETAIL.html)
+ [SYS\_CONNECTION\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/SYS_CONNECTION_LOG.html)
+ [SYS\_DATASHARE\_CHANGE\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_CHANGE_LOG.html)
+ [SYS\_DATASHARE\_USAGE\_CONSUMER](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_USAGE_CONSUMER.html)
+ [SYS\_DATASHARE\_USAGE\_PRODUCER](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_USAGE_PRODUCER.html)
+ [SYS\_EXTERNAL\_QUERY\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_QUERY_DETAIL.html)
+ [SYS\_LOAD\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_LOAD_HISTORY.html)
+ [SYS\_LOAD\_ERROR\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_LOAD_ERROR_DETAIL.html)
+ [SYS\_UNLOAD\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_UNLOAD_HISTORY.html)

Follow these steps to monitor query runtime for each Availability Zone within the Multi\-AZ deployment:

1. Navigate to the Amazon Redshift console and connect to the database in your Multi\-AZ deployment and run queries through the query editor\.

1. Run any sample query on the Multi\-AZ Amazon Redshift deployment\.

1. For a Multi\-AZ deployment, you can identify a query and the Availability Zone where it is run by using the compute\_type column in the SYS\_QUERY\_HISTORY table\. *primary* stands for queries run on the primary cluster in the Multi\-AZ deployment, and *secondary* stands for queries run on the secondary cluster in the Multi\-AZ deployment\.

   The following query uses compute\_type column to monitor a query\.

   ```
   select (compute_type) as compute_type, left(query_text, 50) query_text from sys_query_history order by start_time desc;
       
    compute_type | query_text
   --------------+-------------------------
      secondary  | select count(*) from t1;
   ```

## Ending a query for Multi\-AZ clusters<a name="ending-multi-az-query"></a>

**To end a query**

You can also use the **Queries** page to end a query that is currently in progress\.

Your database user must have the sys:operator role and permissions to end a running query\. For information about system roles, see [Amazon Redshift system\-defined roles](https://docs.aws.amazon.com/redshift/latest/dg/r_roles-default.html) in the *Amazon Redshift Database Developer Guide*\.

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Queries and loads** to display the list of queries for your account\. 

1. Choose the running query that you want to end in the list, and then choose **Terminate query**\. 