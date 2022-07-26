# Data sharing in Amazon Redshift Serverless<a name="serverless-datasharing"></a>

Use *data sharing* to share the most up\-to\-date and consistent information as it's updated in Amazon Redshift Serverless\.

## Data sharing in Amazon Redshift Serverless<a name="serverless_datasharing"></a>

With *data sharing*, you have live access to data so that your users can see the most up\-to\-date and consistent information as it's updated in Amazon Redshift Serverless\.

### Getting started with data sharing in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing"></a>

You can share data for read purposes across different Amazon Redshift Serverless instances within or across AWS accounts\.

You can get started with data sharing by using either the SQL interface or the Amazon Redshift console\. For more information, see either [Getting started data sharing using the SQL interface](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-sql.html) or [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide* in the *Amazon Redshift Database Developer Guide*\.

*Within an AWS account*, you can share data for read purposes from Amazon Redshift Serverless to another instance, from a provisioned cluster to Amazon Redshift Serverless, or from Amazon Redshift Serverless to a provisioned cluster\. For more information about sharing data within an AWS account using the SQL interface, see [Sharing data within an AWS account](https://docs.aws.amazon.com/redshift/latest/dg/within-account.html) in the *Amazon Redshift Database Developer Guide*\. 

*Across AWS accounts*, you can share data for read purposes from Amazon Redshift Serverless to another instance, from a provisioned cluster to Amazon Redshift Serverless, or from Amazon Redshift Serverless to a provisioned cluster\. For more information about sharing data across AWS accounts, see [Sharing data across AWS accounts](https://docs.aws.amazon.com/redshift/latest/dg/across-account.html) in the *Amazon Redshift Database Developer Guide*\. 

To get started sharing data within an AWS account, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Namespace configuration** and then **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

To get started sharing data across AWS accounts, open the AWS Management Console, and then choose the Amazon Redshift console\. Choose **Datashares**\. Follow the procedures in [Getting started data sharing using the console](https://docs.aws.amazon.com/redshift/latest/dg/getting-started-datashare-console.html) in the *Amazon Redshift Database Developer Guide*\.

To start querying data in a datashare, create a database in a namespace that has a workgroup associated with it\. From a specified datashare, choose a namespace that has a workgroup associated with it and create a database to query data\. Follow the procedures in [Creating databases from datashares](https://docs.aws.amazon.com/redshift/latest/dg/create-database-from-datashare-console.html)\.

### Data sharing considerations in Amazon Redshift Serverless<a name="getting_started_serverless_datasharing_usage"></a>

Following are considerations for working with data sharing in Amazon Redshift Serverless:
+ When sharing data with provisioned clusters, Amazon Redshift only supports data sharing on the ra3\.16xlarge, ra3\.4xlarge, and ra3\.xlplus instance types for producer and consumer clusters\.
+ Amazon Redshift Serverless is encrypted by default\.

For a list of datasharing limitations, see [Limitations for data sharing](https://docs.aws.amazon.com/redshift/latest/dg/limitations-datashare.html) in the *Amazon Redshift Database Developer Guide*\. 