# Monitoring queries and workloads with Amazon Redshift Serverless<a name="serverless-monitoring"></a>

## Monitoring queries and workload with Amazon Redshift Serverless<a name="serverless-monitoring-overview"></a>

You can monitor your Amazon Redshift Serverless queries and workload with the provided system views\. 

### Granting access to monitor queries<a name="serverless-monitor-access"></a>

A superuser can provide access to users who aren't superusers so that they can perform query monitoring for all users\. First, you add a policy for a user or a role to provide query monitoring access\. Then, you grant query monitoring permission to the user or role\. 

**To add the query monitoring policy**

1. Choose [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Under **Access management**, choose **Policies**\.

1. Choose **Create Policy**\.

1. Choose **JSON** and paste the following policy definition\.

   ```
   {
   "Version": "2012-10-17",
   "Statement": [
   {
   "Effect": "Allow",
   "Action": [
       "redshift-data:ExecuteStatement",
       "redshift-data:DescribeStatement",
       "redshift-data:GetStatementResult",
       "redshift-data:ListDatabases"
   ],
   "Resource": "*"
   },
   {
   "Effect": "Allow",
   "Action": "redshift-serverless:GetCredentials",
   "Resource": "*"
   }
   ]
   }
   ```

1. Choose **Review policy**\.

1. For **Name**, enter a name for the policy, such as `query-monitoring`\.

1. Choose **Create policy**\.

After you create the policy, you can grant the appropriate permissions\.

To provide access, add permissions to your users, groups, or roles:
+ Users and groups in AWS IAM Identity Center \(successor to AWS Single Sign\-On\):

  Create a permission set\. Follow the instructions in [Create a permission set](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.
+ Users managed in IAM through an identity provider:

  Create a role for identity federation\. Follow the instructions in [Creating a role for a third\-party identity provider \(federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\.
+ IAM users:
  + Create a role that your user can assume\. Follow the instructions in [Creating a role for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in the *IAM User Guide*\.
  + \(Not recommended\) Attach a policy directly to a user or add a user to a user group\. Follow the instructions in [Adding permissions to a user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

**To grant query monitoring permission for a user**

Users with `sys:monitor` permission can view all queries\. In addition, users with `sys:operator` permission can cancel queries, analyze query history, and perform vacuum operations\.

1. Enter the following command to provide system monitor access, where *user\-name* is the name of the user for whom you want to provide access\.

   ```
   grant role sys:monitor to "IAM:user-name";
   ```

1. \(Optional\) Enter the following command to provide system operator access, where *user\-name* is the name of the user for whom you want to provide access\.

   ```
   grant role sys:operator to "IAM:user-name";
   ```

**To grant query monitoring permission for a role**

Users with a role that has `sys:monitor` permission can view all queries\. In addition, users with a role that has `sys:operator` permission can cancel queries, analyze query history, and perform vacuum operations\.

1. Enter the following command to provide system monitor access, where *role\-name* is the name of the role for which you want to provide access\.

   ```
   grant role sys:monitor to "IAMR:role-name";
   ```

1. \(Optional\) Enter the following command to provide system operator access, where *role\-name* is the name of the role for which you want to provide access\.

   ```
   grant role sys:operator to "IAMR:role-name";
   ```

### Monitoring views<a name="serverless_views-monitoring"></a>

*Monitoring views* are system views in Amazon Redshift Serverless that are used to monitor query and workload usage\. These views are located in the `pg_catalog` schema\. The system views available have been designed to give you the information needed to monitor Amazon Redshift Serverless, which is much simpler than that needed for provisioned clusters\. The SYS system views have been designed to work with Amazon Redshift Serverless\. To display the information provided by these views, run SQL SELECT statements\.

System views are defined to support the following monitoring objectives\.

**Workload monitoring**  
You can monitor your query activities over time to:  
+ Understand workload patterns, so you know what is normal \(baseline\) and what is within business service level agreements \(SLAs\)\.
+ Rapidly identify deviation from normal, which might be a transient issue or something that warrants further action\.

**Data load and unload monitoring**  
Data movement in and out of Amazon Redshift Serverless is a critical function\. You use COPY and UNLOAD to load or unload data, and you must monitor progress closely in terms of bytes/rows transferred and files completed to track adherence to business SLAs\. This is normally done by running system table queries frequently \(that is, every minute\) to track progress and raise alerts for investigation/corrective action if significant deviations are detected\.

**Failure and problem diagnostics**  
There are cases where you must take action for query or runtime failures\. Developers rely on system tables to self\-diagnose issues and determine correct remedies\.

**Performance tuning**  
You might need to tune queries that are not meeting SLA requirements either from the start, or have degraded over time\. To tune, you must have runtime details including run plan, statistics, duration, and resource consumption\. You need baseline data for offending queries to determine the cause for deviation and to guide you how to improve performance\.

**User objects event monitoring**  
You need to monitor actions and activities on user objects, such as refreshing materialized views, vacuum, and analyze\. This includes system\-managed events like auto\-refresh for materialized views\. You want to monitor when an event ends if it is user initiated, or the last successful run if system initiated\.

**Usage tracking for billing**  
You can monitor your usage trends over time to:  
+ Inform budget planning and business expansion estimates\.
+ Identify potential cost\-saving opportunities like removing cold data\.

You can't query STL, STV, SVCS, SVL, and some SVV system tables and views with Amazon Redshift Serverless, except the following: 
+ [SVV\_ALL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_COLUMNS.html) 
+ [SVV\_ALL\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_SCHEMAS.html) 
+ [SVV\_ALL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALL_TABLES.html) 
+ [SVV\_ALTER\_TABLE\_RECOMMENDATIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ALTER_TABLE_RECOMMENDATIONS.html) 
+ [SVV\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_COLUMNS.html) 
+ [SVV\_COLUMN\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_COLUMN_PRIVILEGES.html) 
+ [SVV\_DATABASE\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATABASE_PRIVILEGES.html) 
+ [SVV\_DATASHARES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARES.html) 
+ [SVV\_DATASHARE\_CONSUMERS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARE_CONSUMERS.html) 
+ [SVV\_DATASHARE\_OBJECTS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARE_OBJECTS.html) 
+ [SVV\_DATASHARE\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DATASHARE_PRIVILEGES.html) 
+ [SVV\_DEFAULT\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_DEFAULT_PRIVILEGES.html) 
+ [SVV\_EXTERNAL\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_COLUMNS.html) 
+ [SVV\_EXTERNAL\_DATABASES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_DATABASES.html) 
+ [SVV\_EXTERNAL\_PARTITIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_PARTITIONS.html) 
+ [SVV\_EXTERNAL\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_SCHEMAS.html) 
+ [SVV\_EXTERNAL\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_EXTERNAL_TABLES.html) 
+ [SVV\_MV\_DEPENDENCY](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_MV_DEPENDENCY.html) 
+ [SVV\_MV\_INFO](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_MV_INFO.html) 
+ [SVV\_FUNCTION\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_FUNCTION_PRIVILEGES.html) 
+ [SVV\_IDENTITY\_PROVIDERS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_IDENTITY_PROVIDERS.html) 
+ [SVV\_INTERLEAVED\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_INTERLEAVED_COLUMNS.html) 
+ [SVV\_LANGUAGE\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_LANGUAGE_PRIVILEGES.html) 
+ [SVV\_ML\_MODEL\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ML_MODEL_PRIVILEGES.html) 
+ [SVV\_ML\_MODEL\_INFO](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ML_MODEL_INFO.html) 
+ [SVV\_REDSHIFT\_COLUMNS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_COLUMNS.html) 
+ [SVV\_REDSHIFT\_DATABASES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_DATABASES.html) 
+ [SVV\_REDSHIFT\_FUNCTIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_FUNCTIONS.html) 
+ [SVV\_REDSHIFT\_SCHEMAS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_SCHEMAS.html) 
+ [SVV\_REDSHIFT\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_REDSHIFT_TABLES.html) 
+ [SVV\_RELATION\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_RELATION_PRIVILEGES.html) 
+ [SVV\_RLS\_ATTACHED\_POLICY](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_RLS_ATTACHED_POLICY.html) 
+ [SVV\_RLS\_POLCY](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_RLS_POLCY.html) 
+ [SVV\_RLS\_RELATION](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_RLS_RELATION.html) 
+ [SVV\_ROLE\_GRANTS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ROLE_GRANTS.html) 
+ [SVV\_ROLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_ROLES.html) 
+ [SVV\_SCHEMA\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_SCHEMA_PRIVILEGES.html) 
+ [SVV\_SYSTEM\_PRIVILEGES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_SYSTEM_PRIVILEGES.html) 
+ [SVV\_TABLES](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TABLES.html) 
+ [SVV\_TABLE\_INFO](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TABLE_INFO.html) 
+ [SVV\_TRANSACTIONS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_TRANSACTIONS.html) 
+ [SVV\_USER\_INFO](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_USER_INFO.html) 
+ [SVV\_USER\_GRANTS](https://docs.aws.amazon.com/redshift/latest/dg/r_SVV_USER_GRANTS.html) 

You can query the following SYS system views to monitor Amazon Redshift Serverless\. 
+ [SYS\_CONNECTION\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/SYS_CONNECTION_LOG.html)
+ [SYS\_DATASHARE\_CHANGE\_LOG](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_CHANGE_LOG.html)
+ [SYS\_DATASHARE\_USAGE\_CONSUMER](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_USAGE_CONSUMER.html)
+ [SYS\_DATASHARE\_USAGE\_PRODUCER](https://docs.aws.amazon.com/redshift/latest/dg/SYS_DATASHARE_USAGE_PRODUCER.html)
+ [SYS\_QUERY\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_QUERY_HISTORY.html)
+ [SYS\_QUERY\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_QUERY_DETAIL.html)
+ [SYS\_EXTERNAL\_QUERY\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_EXTERNAL_QUERY_DETAIL.html)
+ [SYS\_LOAD\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_LOAD_DETAIL.html)
+ [SYS\_LOAD\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_LOAD_HISTORY.html)
+ [SYS\_LOAD\_ERROR\_DETAIL](https://docs.aws.amazon.com/redshift/latest/dg/SYS_LOAD_ERROR_DETAIL.html)
+ [SYS\_UNLOAD\_HISTORY](https://docs.aws.amazon.com/redshift/latest/dg/SYS_UNLOAD_HISTORY.html)
+ [SYS\_SERVERLESS\_USAGE](https://docs.aws.amazon.com/redshift/latest/dg/SYS_SERVERLESS_USAGE.html)