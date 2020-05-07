# Encryption in transit<a name="security-encryption-in-transit"></a>

You can configure your environment to protect the confidentiality and integrity data in transit\.

Encryption of data in transit between an Amazon Redshift cluster and SQL clients over JDBC/ODBC:
+ You can connect to Amazon Redshift clusters from SQL client tools over Java Database Connectivity \(JDBC\) and Open Database Connectivity \(ODBC\) connections\. 
+ Amazon Redshift supports Secure Sockets Layer \(SSL\) connections to encrypt data and server certificates to validate the server certificate that the client connects to\. The client connects to the leader node of an Amazon Redshift cluster\. For more information, see [Configuring security options for connections](connecting-ssl-support.md)\.
+ To support SSL connections, Amazon Redshift creates and installs AWS Certificate Manager \(ACM\) issued certificates on each cluster\. For more information, see [Transitioning to ACM certificates for SSL connections](connecting-transitioning-to-acm-certs.md)\. 
+ To protect your data in transit within the AWS Cloud, Amazon Redshift uses hardware accelerated SSL to communicate with Amazon S3 or Amazon DynamoDB for COPY, UNLOAD, backup, and restore operations\. 

Encryption of data in transit between an Amazon Redshift cluster and Amazon S3 or DynamoDB:
+ Amazon Redshift uses hardware accelerated SSL to communicate with Amazon S3 or DynamoDB for COPY, UNLOAD, backup, and restore operations\. 
+ Redshift Spectrum supports the Amazon S3 server\-side encryption \(SSE\) using your account's default key managed by the AWS Key Management Service \(KMS\)\. 
+ Encrypt Amazon Redshift loads with Amazon S3 and AWS KMS\. For more information, see https://aws\.amazon\.com/blogs/big\-data/encrypt\-your\-amazon\-redshift\-loads\-with\-amazon\-s3\-and\-aws\-kms/\.

Encryption and signing of data in transit between AWS CLI, SDK, or API clients and Amazon Redshift endpoints:
+ Amazon Redshift provides HTTPS endpoints for encrypting data in transit\. 
+ To protect the integrity of API requests to Amazon Redshift, API calls must be signed by the caller \(by an X\.509 certificate or the customer's AWS Secret Access Key\) according to the Signature Version 4 Signing Process \(Sigv4\)\. For more information, see [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. 
+  Use the AWS CLI or one of the AWS SDKs to make requests to AWS\. These tools automatically sign the requests for you with the access key that you specify when you configure the tools\. 