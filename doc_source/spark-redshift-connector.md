# Amazon Redshift integration for Apache Spark<a name="spark-redshift-connector"></a>

 [Apache Spark](https://aws.amazon.com/emr/features/spark/) is a distributed processing framework and programming model that helps you do machine learning, stream processing, or graph analytics\. Similar to Apache Hadoop, Spark is an open\-source, distributed processing system commonly used for big data workloads\. Spark has an optimized directed acyclic graph \(DAG\) execution engine and actively caches data in\-memory\. This can boost performance, especially for certain algorithms and interactive queries\. 

 This integration provides you with a Spark connector you can use to build Apache Spark applications that read from and write to data in Amazon Redshift and Amazon Redshift Serverless\. These applications don't compromise on application performance or transactional consistency of the data\. This integration is automatically included in [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/) and [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/), so you can immediately run Apache Spark jobs that access and load data into Amazon Redshift as part of your data ingestion and transformation pipelines\. 

Currently, you can use the versions 3\.3\.0 and 3\.3\.1 of Spark with this integration\.

 This integration provides the following: 
+  AWS Identity and Access Management \(IAM\) authentication\. For more information, see [ Identity and access management in Amazon Redshift\.](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html) 
+ Predicate and query pushdown to improve performance\.
+  Amazon Redshift data types\. 
+ Connectivity to Amazon Redshift and Amazon Redshift Serverless\.

## Considerations and limitations when using the Spark connector<a name="spark-redshift-connector-considerations"></a>
+ The parameter `unload_s3_format` supports the Parquet format when running read operations from Redshift, but write operations are currently unsupported\.
+  The tempdir URI points to an Amazon S3 location\. This temp directory is not cleaned up automatically and could add additional cost\. We recommend using [Amazon S3 lifecycle policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) to define the retention rules for the Amazon S3 bucket\. 
+  By default, copies between Amazon S3 and Redshift will not work if the S3 bucket and Redshift cluster are in different AWS Regions\. To use separate AWS Regions, set the `tempdir_region` parameter to the Region of the S3 bucket used for the `tempdir`\.
+ We recommend using [Amazon S3 server\-side encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html) to encrypt the Amazon S3 buckets used\. 
+ We recommend [ blocking public access to Amazon S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html)\. 
+  We recommend that the Amazon Redshift cluster should not be publicly accessible\. 
+  We recommend turning on [ Amazon Redshift audit logging](https://docs.aws.amazon.com/redshift/latest/mgmt/db-auditing.html)\. 
+  We recommend turning on [ Amazon Redshift at\-rest encryption](https://docs.aws.amazon.com/redshift/latest/mgmt/security-server-side-encryption.html)\. 
+  We recommend turning on SSL for the JDBC connection from Spark on Amazon EMR to Amazon Redshift\. 
+ We recommend passing an IAM role using the parameter `aws_iam_role` for the Amazon Redshift authentication parameter\.