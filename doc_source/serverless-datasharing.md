# Data sharing in Amazon Redshift Serverless<a name="serverless-datasharing"></a>

Use *data sharing* to share the most up\-to\-date and consistent information as it's updated in Amazon Redshift Serverless\.

## Data sharing in Amazon Redshift Serverless<a name="serverless_datasharing"></a>

With *data sharing*, you have live access to data so that your users can see the most up\-to\-date and consistent information as it's updated in Amazon Redshift Serverless\.

### Getting started with data sharing in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing"></a>

You can share data for read purposes across different Amazon Redshift Serverless instances within or across AWS accounts\.

You can get started with data sharing by using either the SQL interface or the Amazon Redshift console\. For more information, see either [Getting started data sharing using the SQL interface](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-sql.html) or [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide* in the *Amazon Redshift Database Developer Guide*\.

With data sharing, Amazon Redshift Serverless namespaces and provisioned clusters can share live data with each other, whether they are within an AWS account across AWS accounts, or across AWS Regions\. For more information, see [Regions where data sharing is available](https://docs.aws.amazon.com/redshift/latest/dg/data_sharing_regions.html)\.

To get started sharing data within an AWS account, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Namespace configuration** and then **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

To get started sharing data across AWS accounts, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

To start querying data in a datashare, create a database in a namespace that has a workgroup associated with it\. From a specified datashare, choose a namespace that has a workgroup associated with it and create a database to query data\. Follow the procedures in [Creating databases from datashares](https://docs.aws.amazon.com/redshift/latest/dg/create-database-from-datashare-console.html)\.

### Granting access to view datashares using the console<a name="serverless_datasharing_permissions"></a>

A superuser can provide access to users who aren't superusers so that they can view the datashares created by all users\. 

To grant access to a datashare for a user, use the following command to provide datashare access for a user, where datashare\_name is the name of the datashare and user\-name is the name of the user for whom you want to provide access\.

```
grant share on datashare datashare_name to "IAM:test_user";
```

To grant access to a datashare for a user group, first create a user group with users\. For information on how to create user groups, see [CREATE GROUP](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_GROUP.html)\. Then, grant datashare access to a user using the following command, where datashare\_name is the name of the datashare and user\-group is the name of the user\-group to that you want to grant access\.

```
grant share on datashare datashare_name to group user_group;
```

For information on how to use the GRANT statement, see [GRANT](https://docs.aws.amazon.com/redshift/latest/dg/r_GRANT.html)\.

### Data sharing considerations in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing_usage"></a>

Following are considerations for working with data sharing in Amazon Redshift Serverless:
+ Amazon Redshift only supports provisioned clusters of instance type ra3\.16xlarge, ra3\.4xlarge, and ra3\.xlplus, and serverless endpoint as data sharing producers or consumers\.
+ Amazon Redshift Serverless is encrypted by default\.

For a list of datasharing limitations, see [Limitations for data sharing](https://docs.aws.amazon.com/redshift/latest/dg/limitations-datashare.html) in the *Amazon Redshift Database Developer Guide*\. 