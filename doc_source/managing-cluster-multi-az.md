# Configuring Multi\-AZ deployment \(preview\)<a name="managing-cluster-multi-az"></a>


|  | 
| --- |
| This is prerelease documentation for the Multi Availability Zones feature for Amazon Redshift, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only with test clusters, and not in production environments\. Public preview will end on April 26, 2023\. Preview clusters and preview serverless workgroups and namespaces will be removed automatically two weeks after the end of the preview\. For inquiries, email redshift\-maz@amazon\.com\. For preview terms and conditions, see Betas and Previews in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

By using *multiple Availability Zones \(Multi\-AZ\)* in Amazon Redshift, your Amazon Redshift data warehouse can continue operating in failure scenarios where an unexpected event happens in an Availability Zone\. 

For a Multi\-AZ deployment, Amazon Redshift deploys equal compute resources in two Availability Zones that can be accessed through a single endpoint\. In the event of an entire Availability Zone failure, the remaining compute resources in the second Availability Zone will be available to continue processing workloads\. Amazon Redshift Multi\-AZ also warrants that there is no data loss in the event of a failure\. Amazon Redshift charges the same hourly compute rates for RA3 when running a Multi\-AZ cluster\.

## Overview<a name="overview-multi-az"></a>

When setting up a Multi\-AZ deployment, specify the number of compute nodes to provision in each Availability Zone\. The nodes are used in both Availability Zones in an active\-active fashion, for both read and write operations\. Amazon Redshift will process an individual query using the compute resources residing in only one Availability Zone\. However, it will automatically distribute processing of multiple simultaneous queries to both Availability Zones to increase performance of high concurrency workloads\.

When migrating to a Multi\-AZ deployment from an existing Single\-AZ cluster, maintaining performance of a single query might require the same number of nodes used in the current Single\-AZ cluster to be provisioned in both Availability Zones\. This results in doubling the number of cluster nodes needed when migrating to Multi\-AZ, to facilitate that single query performance is maintained\.

In the event of a failure in an Availability Zone, Amazon Redshift continues operating by using the resources in the remaining Availability Zone automatically\. However, user connections might be lost and must be re\-established\. In addition, queries that were running in the failed Availability Zone will be stopped\. However, you can reconnect to your cluster and reschedule queries immediately\. Amazon Redshift will process the rescheduled queries in the remaining Availability Zone\. In the event that a query fails or a connection shuts down, Amazon Redshift re\-tries the failed query or re\-establishes connection immediately\. Queries issued at or after a failure occurs might experience runtime delays while the Multi\-AZ data warehouse is recovering\.

### Limitations<a name="limitations-multi-az"></a>

The following are limitations when working with Amazon Redshift Multi\-AZ:
+ You can't modify an existing Single\-AZ deployment to convert it into a Multi\-AZ deployment, or vice versa \(preview\)\.
+ You can't restore an existing Multi\-AZ deployment into a Single\-AZ deployment on a Current or Trailing maintenance track \(preview\)\.
+ You can't resize a cluster with Multi\-AZ deployment \(preview\)\.
+ You can't perform table\-level restore from a snapshot taken off a cluster with Multi\-AZ deployment \(preview\)\.
+ You can't create a serverless Multi\-AZ deployment or convert an existing serverless namespace into a Multi\-AZ deployment \(preview\)\.
+ Amazon Redshift doesn't display node\-level metrics for compute resources in the secondary Availability Zone within a Multi\-AZ deployment in the Amazon Redshift console \(preview\)\.
+ You can't use Amazon Redshift API or AWS Command Line Interface to manage your Multi\-AZ deployment \(preview\)\.
+ You can't preview automatic optimization feature on a Multi\-AZ deployment \(preview\)\.
+ You will lose any IAM roles added after the Multi\-AZ deployment was created whenever the cluster detects a failure and automatically recovers in the secondary AZ\. Any IAM roles or permissions added before the creation of the Multi\-AZ deployment will still persist \(preview\)\.
+ You can't access your Multi\-AZ cluster through a public endpoint\.
+ You can't create a single node Multi\-AZ deployment for any of the RA3 instance types\. Choose 2 or more nodes per Availability Zone while creating a Multi\-AZ deployment\.
+ You can't create an unencrypted Multi\-AZ cluster or restore an unencrypted Single\-AZ cluster to a Multi\-AZ cluster\.
+ Amazon Redshift requires a subnet configuration that can support three Availability Zones\. In other words, the configured subnet group requires three subnets\.
+ You can't relocate a Multi\-AZ deployment to another Availability Zone\. Relocation will be turned off when using Multi\-AZ deployment\.
+ You can't pause or resume a Multi\-AZ deployment, as it is designed to provide the highest level of availability\.
+ Amazon Redshift Multi\-AZ deployment is available in the following AWS Regions:
  + US East \(Ohio\) \(us\-east\-2\)
  + US East \(N\. Virginia\) \(us\-east\-1\)
  + US West \(Oregon\) \(us\-west\-2\)
  + Asia Pacific \(Tokyo\) \(ap\-northeast\-1\)
  + Europe \(Ireland\) \(eu\-west\-1\)
  + Europe \(Stockholm\) \(eu\-north\-1\)

**Topics**
+ [Overview](#overview-multi-az)
+ [Managing Multi\-AZ deployment](using-multi-az.md)
+ [Managing Multi\-AZ using the console](multi-az-console.md)