# Amazon Redshift Enhanced VPC Routing<a name="enhanced-vpc-routing"></a>

When you use Amazon Redshift Enhanced VPC Routing, Amazon Redshift forces all [COPY](http://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) and [UNLOAD](http://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) traffic between your cluster and your data repositories through your Amazon VPC\. You can now use standard VPC features, such as [VPC security groups](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html), [network access control lists \(ACLs\)](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html), [VPC endpoints](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html), [VPC endpoint policies](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints-s3.html#vpc-endpoints-policies-s3), [Internet gateways](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html), and [Domain Name System \(DNS\)](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-dns.html) servers, to tightly manage the flow of data between your Amazon Redshift cluster and other resources\. When you use Enhanced VPC Routing to route traffic through your VPC, you can also use [VPC flow logs](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) to monitor COPY and UNLOAD traffic\.

If Enhanced VPC Routing is not enabled, Amazon Redshift routes traffic through the Internet, including traffic to other services within the AWS network\.

**Important**  
Because Enhanced VPC Routing affects the way that Amazon Redshift accesses other resources, COPY and UNLOAD commands might fail unless you configure your VPC correctly\. You must specifically create a network path between your cluster's VPC and your data resources, as described following\.

When you execute a COPY or UNLOAD command on a cluster that has Enhanced VPC Routing enabled, your VPC routes the traffic to the specified resource using the *strictest*, or most specific, network path available\. 

For example, you can configure the following pathways in your VPC:

+ ** VPC Endpoints ** – For traffic to an Amazon S3 bucket in the same region as your cluster, you can create a VPC endpoint to direct traffic directly to the bucket\. When you use VPC endpoints, you can attach an endpoint policy to manage access to Amazon S3\. For more information about using endpoints with Amazon Redshift, see [Working with VPC Endpoints](enhanced-vpc-working-with-endpoints.md)\.

+ **NAT gateway** – To connect to an Amazon S3 bucket in another region or to another service within the AWS network, or to access a host instance outside the AWS network, you can configure a [ network address translation \(NAT\) gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-gateway.html)\. 

+ **Internet gateway** – To connect to AWS services outside your VPC, you can attach an [Internet gateway](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Internet_Gateway.html) to your VPC subnet\. To use an Internet gateway, your cluster must have a public IP to allow other services to communicate with your cluster\.

For more information, see [VPC Endpoints](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) in the Amazon VPC User Guide\.

There is no additional charge for using Enhanced VPC Routing\. You might incur additional data transfer charges for certain operations, such as UNLOAD to Amazon S3 in a different region or COPY from Amazon EMR or SSH with public IP addresses\. For more information about pricing, see [Amazon EC2 Pricing](https://aws.amazon.com//ec2/pricing/)\.


+ [Working with VPC Endpoints](enhanced-vpc-working-with-endpoints.md)
+ [Enabling Enhanced VPC Routing](enhanced-vpc-enabling-cluster.md)