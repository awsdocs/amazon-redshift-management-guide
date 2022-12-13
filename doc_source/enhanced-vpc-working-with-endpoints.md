# Working with VPC endpoints<a name="enhanced-vpc-working-with-endpoints"></a>

You can use a VPC endpoint to create a managed connection between your Amazon Redshift cluster in a VPC and Amazon Simple Storage Service \(Amazon S3\)\. When you do, COPY and UNLOAD traffic between your database and your data on Amazon S3 stays in your Amazon VPC\. You can attach an endpoint policy to your endpoint to more closely manage access to your data\. For example, you can add a policy to your VPC endpoint that permits unloading data only to a specific Amazon S3 bucket in your account\.

**Important**  
Currently, Amazon Redshift supports VPC endpoints only for connecting to Amazon S3\. When Amazon VPC adds support for other AWS services to use VPC endpoints, Amazon Redshift will support those VPC endpoint connections also\. To connect to an Amazon S3 bucket using a VPC endpoint, the Amazon Redshift cluster and the Amazon S3 bucket that it connects to must be in the same AWS Region\.

To use VPC endpoints, create a VPC endpoint for the VPC that your data warehouse is in and then turn on enhanced VPC routing\. You can turn on enhanced VPC routing when you create your cluster or workgroup, or you can modify a cluster or workgroup in a VPC to use enhanced VPC routing\.

A VPC endpoint uses route tables to control the routing of traffic between a cluster or workgroup in the VPC and Amazon S3\. All clusters and workgroups in subnets associated with the specified route tables automatically use that endpoint to access the service\.

Your VPC uses the most specific, or most restrictive, route that matches your traffic to determine how to route the traffic\. For example, suppose that you have a route in your route table for all internet traffic \(0\.0\.0\.0/0\) that points to an internet gateway and an Amazon S3 endpoint\. In this case, the endpoint route takes precedence for all traffic destined for Amazon S3\. This is because the IP address range for the Amazon S3 service is more specific than 0\.0\.0\.0/0\. In this example, all other internet traffic goes to your internet gateway, including traffic that's destined for Amazon S3 buckets in other AWS Regions\.

For more information about creating endpoints, see [Create a VPC endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/create-interface-endpoint.html) in the *Amazon VPC User Guide\.*

You use endpoint policies to control access from your cluster or workgroup to the Amazon S3 buckets that hold your data files\. For more specific control, you can optionally attach a custom endpoint policy\. For more information, see [Control access to services using endpoint policies](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) in the *AWS PrivateLink Guide\.* 

There is no additional charge for using endpoints\. Standard charges for data transfer and resource usage apply\. For more information about pricing, see [Amazon EC2 Pricing](https://aws.amazon.com/redshift/pricing/#Data_Transfer)\.