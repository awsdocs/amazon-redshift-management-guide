# Quotas and limits for Amazon Redshift Serverless<a name="serverless-limits"></a>

## Limits for Amazon Redshift Serverless objects<a name="serverless-limits-account"></a>

Amazon Redshift has quotas that limit the use of several object types in your Amazon Redshift Serverless instance\. There is a default value for each\.


| Quota name | AWS default value | Adjustable | Description | 
| --- | --- | --- | --- | 
| Number of databases | 100 | No | The maximum allowed count of databases in an Amazon Redshift Serverless instance\. | 
| Number of schemas | 9,900 | No | The maximum allowed count of schemas in an Amazon Redshift Serverless instance\. | 
| Number of tables | 100,000 | No | The maximum allowed count of tables in an Amazon Redshift Serverless instance\. | 
| Running time of a query | 4 hours | No | The maximum run time for a query before it times out\. | 
| Running time of an open transaction | 6 hours | No | The maximum run time for transaction before the session times out\. This takes precedence over any customized timeout settings\. | 
| Number of maximum connections | 2000 | No | The maximum number of connections allowed to connect to a workgroup\. | 

For more information about how Amazon Redshift Serverless billing is affected by timeout configuration, see [Billing for Amazon Redshift Serverless](serverless-billing.md)\.

## Cursor constraints<a name="serverless-limits-cursor-fetch"></a>


| Type | Maximum result set \(MB\) | 
| --- | --- | 
| Amazon Redshift Serverless | 150000 | 

To see a table that lists result\-set size specifications for a provisioned cluster, see [DECLARE](https://docs.aws.amazon.com/redshift/latest/dg/declare.html)\.

## Additional constraints<a name="serverless-limits-additional"></a>

*Maintenance window* \- There is no maintenance window with Amazon Redshift Serverless\. Software version updates are automatically applied\. There's no interruption for existing connection or query execution when Amazon Redshift switches versions\. New connections will always connect and work with Amazon Redshift Serverless instantly\.

*AQUA* \- AQUA \(Advanced Query Accelerator\) for Amazon Redshift is not supported for Amazon Redshift Serverless\.

*Availability Zone IDs* \- When you configure your Amazon Redshift Serverless instance, open **Additional considerations**, and make sure that the subnet IDs provided in **Subnet** contain at least three of the supported Availability Zone IDs\. To see the subnet to Availability Zone ID mapping, go to the VPC console and choose **Subnets** to see the list of subnet IDs with their Availability Zone IDs\. Verify that your subnet is mapped to a supported Availability Zone ID\. To create a subnet, see [Create a subnet in your VPC](https://docs.aws.amazon.com/vpc/latest/userguide/working-with-vpcs.html#AddaSubnet) in the *Amazon VPC User Guide*\. 

*Three subnets* \- You must have at least three subnets, and they must be span across three Availability Zones\. For example, you might use three subnets that map to the Availability Zones us\-east\-1a, us\-east\-2a, and us\-west\-2a\.

*Storage space after migration* \- When migrating small Amazon Redshift provisioned clusters to Amazon Redshift Serverless, you may see an increase in storage\-space allocation after migration\. This is a result of optimized storage\-space allocation, resulting in pre allocated storage space\. This space is used over period of time as data grows in Amazon Redshift Serverless\.

*Datasharing between Amazon Redshift Serverless and Amazon Redshift provisioned clusters * \- When datasharing where Amazon Redshift Serverless is the producer and a provisioned cluster is the consumer, the provisioned cluster must have a cluster version greater than 1\.0\.38214\. If you use a cluster version less than this, an error occurs when you run a query\. You can view the cluster version on the Amazon Redshift console on the **Maintenance ** tab\. You can also run `SELECT version();`\.

*Max query execution time * \- Elapsed execution time for a query, in seconds\. Execution time doesn't include time spent waiting in a queue\. If a query exceeds the set execution time, Amazon Redshift Serverless aborts the query\. Valid values are 0–86,399\.