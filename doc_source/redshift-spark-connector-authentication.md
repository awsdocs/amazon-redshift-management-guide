# Authentication with the Spark connector<a name="redshift-spark-connector-authentication"></a>

The following diagram describes the authentication between Amazon S3, Amazon Redshift, the Spark driver, and Spark executors\.

![\[This is a diagram of the spark connector authentication.\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/spark-connector-authentication.png)

## Authentication between Redshift and Spark<a name="redshift-spark-authentication"></a>

 You can use the Amazon Redshift provided JDBC driver version 2 driver to connect to Amazon Redshift with the Spark connector by specifying a username and password\. To use IAM, [configure your JDBC url to use IAM authentication](https://docs.aws.amazon.com/redshift/latest/mgmt/generating-iam-credentials-configure-jdbc-odbc.html)\. To connect to a Redshift cluster from Amazon EMR or AWS Glue, make sure that your IAM role has the necessary permissions to retrieve temporary IAM credentials\. The following list describes all of the permissions that your IAM role needs to retrieve credentials and run Amazon S3 operations\. 
+ [ Redshift:GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) \(for provisioned Redshift clusters\)
+ [ Redshift:DescribeClusters](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusters.html) \(for provisioned Redshift clusters\)
+ [ Redshift:GetWorkgroup](https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/API_GetWorkgroup.html) \(for Amazon Redshift Serverless; workgroups\)
+ [ Redshift:GetCredentials](https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/API_GetCredentials.html) \(for Amazon Redshift Serverless workgroups\)
+ [ s3:GetBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_control_GetBucket.html)
+ [ s3:GetBucketLocation](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLocation.html)
+ [ s3:GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)
+ [ s3:PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)
+ [ s3:GetBucketLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLifecycleConfiguration.html)

 For more information about GetClusterCredentials, see [ Resource policies for GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-access-control-identity-based.html#redshift-policy-resources.getclustercredentials-resources)\. 

You also must make sure that Amazon Redshift can assume the IAM role during `COPY` and `UNLOAD` operations\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "redshift.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

If you’re using the latest JDBC driver, the driver will automatically manage the transition from an Amazon Redshift self\-signed certificate to an ACM certificate\. However, you must [specify the SSL options to the JDBC url](https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-configuration-options.html#jdbc20-ssl-option)\. 

 The following is an example of how to specify the JDBC driver URL and `aws_iam_role` to connect to Amazon Redshift\. 

```
df.write \
  .format("io.github.spark_redshift_community.spark.redshift ") \
  .option("url", "jdbc:redshift:iam://<the-rest-of-the-connection-string>") \
  .option("dbtable", "<your-table-name>") \
  .option("tempdir", "s3a://<your-bucket>/<your-directory-path>") \
  .option("aws_iam_role", "<your-aws-role-arn>") \
  .mode("error") \
  .save()
```

## Authentication between Amazon S3 and Spark<a name="spark-s3-authentication"></a>

 If you’re using an IAM role to authenticate between Spark and Amazon S3, use one of the following methods: 
+ The AWS SDK for Java will automatically attempt to find AWS credentials by using the default credential provider chain implemented by the DefaultAWSCredentialsProviderChain class\. For more information, see [ Using the Default Credential Provider Chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default)\.
+ You can specify AWS keys via [ Hadoop configuration properties](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aws/src/site/markdown/tools/hadoop-aws/index.md)\. For example, if your `tempdir` configuration points to a `s3n://` filesystem, set the `fs.s3n.awsAccessKeyId` and `fs.s3n.awsSecretAccessKey` properties in a Hadoop XML configuration file or call `sc.hadoopConfiguration.set()` to change Spark's global Hadoop configuration\.

For example, if you are using the s3n filesystem, add:

```
sc.hadoopConfiguration.set("fs.s3n.awsAccessKeyId", "YOUR_KEY_ID")
sc.hadoopConfiguration.set("fs.s3n.awsSecretAccessKey", "YOUR_SECRET_ACCESS_KEY")
```

For the s3a filesystem, add:

```
sc.hadoopConfiguration.set("fs.s3a.access.key", "YOUR_KEY_ID")
sc.hadoopConfiguration.set("fs.s3a.secret.key", "YOUR_SECRET_ACCESS_KEY")
```

If you’re using Python, use the following operations:

```
sc._jsc.hadoopConfiguration().set("fs.s3n.awsAccessKeyId", "YOUR_KEY_ID")
sc._jsc.hadoopConfiguration().set("fs.s3n.awsSecretAccessKey", "YOUR_SECRET_ACCESS_KEY")
```
+ Encode authentication keys in the `tempdir` URL\. For example, the URI `s3n://ACCESSKEY:SECRETKEY@bucket/path/to/temp/dir` encodes the key pair \(`ACCESSKEY`, `SECRETKEY`\)\.

## Authentication between Redshift and Amazon S3<a name="redshift-s3-authentication"></a>

 If you’re using the COPY and UNLOAD commands in your query, you also must grant Amazon S3 access to Amazon Redshift to run queries on your behalf\. To do so, first [authorize Amazon Redshift to access other AWS services](https://docs.aws.amazon.com/redshift/latest/mgmt/authorizing-redshift-service.html), then authorize the [ COPY and UNLOAD operations using IAM roles](https://docs.aws.amazon.com/redshift/latest/mgmt/copy-unload-iam-role.html)\. 

**Note**  
 Acknowledgement: This documentation contains sample code and language developed by the [Apache Software Foundation](http://www.apache.org/) licensed under the [Apache 2\.0 license](https://www.apache.org/licenses/LICENSE-2.0)\. 