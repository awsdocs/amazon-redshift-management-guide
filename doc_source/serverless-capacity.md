# Compute capacity for Amazon Redshift Serverless<a name="serverless-capacity"></a>

## Understanding Amazon Redshift Serverless capacity<a name="serverless-rpu-capacity"></a>

**RPUs**

Amazon Redshift Serverless measures data warehouse capacity in Redshift Processing Units \(RPUs\)\. RPUs are resources used to handle workloads\.

**Base capacity**

 This setting specifies the base data warehouse capacity Amazon Redshift uses to serve queries\. Base capacity is specified in RPUs\. Setting higher base capacity improves query performance, especially for data processing jobs that consume a lot of resources\.  The default base capacity for Amazon Redshift Serverless is 128 RPUs\. You can adjust the **Base capacity** setting from 32 RPUs to 512 RPUs in units of 8 \(32,40,48\.\.\.512\), using the management console\. 