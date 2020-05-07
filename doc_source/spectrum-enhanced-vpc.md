# Using Amazon Redshift Spectrum with enhanced VPC routing<a name="spectrum-enhanced-vpc"></a>

Amazon Redshift enhanced VPC routing routes specific traffic through your VPC\. All traffic between your cluster and your Amazon S3 buckets is forced to pass through your Amazon VPC\. Redshift Spectrum runs on AWS\-managed resources that are owned by Amazon Redshift\. Because these resources are outside your VPC, Redshift Spectrum doesn't use enhanced VPC routing\. 

When your cluster is configured to use enhanced VPC routing, traffic between Redshift Spectrum and Amazon S3 is securely routed through the AWS private network, outside of your VPC\. In\-flight traffic is signed using Amazon Signature Version 4 protocol \(SIGv4\) and encrypted using HTTPS\. This traffic is authorized based on the IAM role that is attached to your Amazon Redshift cluster\. To further manage Redshift Spectrum traffic, you can modify your cluster's IAM role and your policy attached to the Amazon S3 bucket\. You might also need to configure your VPC to allow your cluster to access AWS Glue or Athena, as detailed following\. 

## Considerations for using enhanced VPC routing for Redshift Spectrum<a name="spectrum-enhanced-vpc-considerations"></a>

Following are considerations when using Redshift Spectrum enhanced VPC routing: 
+ [Bucket access policies](#spectrum-enhanced-vpc-considerations-policies)
+ [Cluster IAM role](#spectrum-enhanced-vpc-considerations-cluster-role)
+ [Logging and auditing Amazon S3 access](#spectrum-enhanced-vpc-considerations-logging-s3)
+ [Access to AWS Glue or Amazon Athena](#spectrum-enhanced-vpc-considerations-glue-access)

### Bucket access policies<a name="spectrum-enhanced-vpc-considerations-policies"></a>

You can control access to data in your Amazon S3 buckets by using a bucket policy attached to the bucket and by using an IAM role attached to the cluster\. 

Redshift Spectrum can't access data stored in Amazon S3 buckets that use a bucket policy that restricts access to only specified VPC endpoints\. Instead, use a bucket policy that restricts access to only specific principals, such as a specific AWS account or specific users\. 

For the IAM role that is granted access to the bucket, use a trust relationship that allows the role to be assumed only by the Amazon Redshift service principal\. When attached to your cluster, the role can be used only in the context of Amazon Redshift and can't be shared outside of the cluster\. For more information, see [Restricting access to IAM roles](authorizing-redshift-service.md#authorizing-redshift-service-database-users)\.

The following example bucket policy permits access to the specified bucket only from traffic originated by Redshift Spectrum owned by AWS account `123456789012`\. 

```
{
  "Version":"2012-10-17",
  "Statement":[
  {
     "Sid”:”BucketPolicyForSpectrum",
     "Effect":"Allow",
     "Principal": {"AWS": ["arn:aws:iam::123456789012:root"]},
     "Action":[“s3:GetObject",”s3:List*"],
     "Resource":["arn:aws:s3:::examplebucket/*"],
     "Condition":{"StringEquals":{"aws:UserAgent": "AWS Redshift/Spectrum"]}}
   }
  ]
}
```

### Cluster IAM role<a name="spectrum-enhanced-vpc-considerations-cluster-role"></a>

The role attached to your cluster should have a trust relationship that permits it to be assumed only by the Amazon Redshift service, as shown following\.

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

You can add a policy to the cluster role that prevents COPY and UNLOAD access to a specific bucket\. The following policy permits traffic to the specified bucket only from Redshift Spectrum\. 

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": ["s3:Get*", "s3:List*"],
        "Resource": "arn:aws:s3:::myBucket/*",
                "Condition": {"StringEquals": {"aws:UserAgent": "AWS Redshift/Spectrum"}}
    }]
}
```

For more information, see [IAM Policies for Amazon Redshift Spectrum](https://docs.aws.amazon.com/redshift/latest/dg/c-spectrum-iam-policies.html) in the *Amazon Redshift Database Developer Guide*\.

### Logging and auditing Amazon S3 access<a name="spectrum-enhanced-vpc-considerations-logging-s3"></a>

One benefit of using Amazon Redshift enhanced VPC routing is that all COPY and UNLOAD traffic is logged in the VPC flow logs\. Traffic originating from Redshift Spectrum to Amazon S3 doesn't pass through your VPC, so it isn't logged in the VPC flow logs\. When Redshift Spectrum accesses data in Amazon S3, it performs these operations in the context of the AWS account and respective role privileges\. You can log and audit Amazon S3 access using server access logging in AWS CloudTrail and Amazon S3\. 

**AWS CloudTrail Logs** 

To trace all access to objects in Amazon S3, including Redshift Spectrum access, enable CloudTrail logging for Amazon S3 objects\. 

You can use CloudTrail to view, search, download, archive, analyze, and respond to account activity across your AWS infrastructure\. For more information, see [Getting Started with CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-getting-started.html)\. 

By default, CloudTrail tracks only bucket\-level actions\. To track object\-level actions \(such as `GetObject`\), enable data and management events for each logged bucket\. 

**Amazon S3 Server Access Logging** 

Server access logging provides detailed records for the requests that are made to a bucket\. Access log information can be useful in security and access audits\. For more information, see [How to Enable Server Access Logging](https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html#server-access-logging-overview) in the *Amazon Simple Storage Service Developer Guide\.*

For more information, see the AWS Security blog post [How to Use Bucket Policies and Apply Defense\-in\-Depth to Help Secure Your Amazon S3 Data](https://aws.amazon.com/blogs/security/how-to-use-bucket-policies-and-apply-defense-in-depth-to-help-secure-your-amazon-s3-data/)\. 

### Access to AWS Glue or Amazon Athena<a name="spectrum-enhanced-vpc-considerations-glue-access"></a>

Redshift Spectrum accesses your data catalog in AWS Glue or Athena\. Another option is to use a dedicated Hive metastore for your data catalog\. 

To enable access to AWS Glue or Athena, configure your VPC with an internet gateway or NAT gateway\. Configure your VPC security groups to allow outbound traffic to the public endpoints for AWS Glue and Athena\. Alternatively, you can configure an interface VPC endpoint for AWS Glue to access your AWS Glue Data Catalog\. When you use a VPC interface endpoint, communication between your VPC and AWS Glue is conducted within the AWS network\. For more information, see [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint)\.

You can configure the following pathways in your VPC: 
+ **Internet gateway** –To connect to AWS services outside your VPC, you can attach an [internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) to your VPC subnet, as described in the *Amazon VPC User Guide\.* To use an internet gateway, your cluster must have a public IP address to allow other services to communicate with your cluster\. 
+ **NAT gateway **–To connect to an Amazon S3 bucket in another AWS Region or to another service within the AWS network, configure a [network address translation \(NAT\) gateway](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html), as described in the *Amazon VPC User Guide\.* Use this configuration also to access a host instance outside the AWS network\.

For more information, see [Amazon Redshift enhanced VPC routing](enhanced-vpc-routing.md)\.