# Compute capacity for Amazon Redshift Serverless<a name="serverless-capacity"></a>

## Understanding Amazon Redshift Serverless capacity<a name="serverless-rpu-capacity"></a>

**RPUs**

Amazon Redshift Serverless measures data warehouse capacity in Redshift Processing Units \(RPUs\)\. RPUs are resources used to handle workloads\. 

**Base capacity**

 This setting specifies the base data warehouse capacity Amazon Redshift uses to serve queries\. Base capacity is specified in RPUs\. You can set a base capacity in Redshift Processing Units \(RPUs\)\. One RPU provides 16 GB of memory\. Setting higher base capacity improves query performance, especially for data processing jobs that consume a lot of resources\.  The default base capacity for Amazon Redshift Serverless is 128 RPUs\. You can adjust the **Base capacity** setting from 8 RPUs to 512 RPUs in units of 8 \(8,16,24\.\.\.512\), using the AWS console, the `UpdateWorkgroup` API operation, or `update-workgroup` operation in the AWS CLI\. 

With a minimum capacity of 8 RPU, you now have more flexibility to run simpler to more complex workloads based on performance requirements\. The 8, 16, and 24 RPU base RPU capacities are targeted towards workloads that require less than 128TB of data\. If your data requirements are greater than 128 TB, you must use a minimum of 32 RPU\. For workloads that have tables with large number columns and higher concurrency, we recommend using 32 or more RPU\.

### Considerations and limitations for Amazon Redshift Serverless capacity<a name="w33aab8c13b3c15"></a>

The following are considerations and limitations for Amazon Redshift Serverless capacity\.
+ Configurations of 8 or 16 RPU support Redshift managed storage capacity of up to 128 TB\. If you're using more than 128 TB of managed storage, you can't downgrade to less than 32 RPU\.