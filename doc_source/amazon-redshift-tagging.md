# Tagging resources in Amazon Redshift<a name="amazon-redshift-tagging"></a>

**Topics**
+ [Tagging overview](#amazon-redshift-tagging-overview)
+ [Managing resource tags using the console](rs-mgmt-tagging-console.md)
+ [Managing tags using the Amazon Redshift API](rs-mgmt-tagging-cli-api.md)

## Tagging overview<a name="amazon-redshift-tagging-overview"></a>

In AWS, tags are user\-defined labels that consist of key\-value pairs\. Amazon Redshift supports tagging to provide metadata about resources at a glance, and to categorize your billing reports based on cost allocation\. To use tags for cost allocation, you must first activate those tags in the AWS Billing and Cost Management service\. For more information about setting up and using tags for billing purposes, see [Use cost allocation tags for custom billing reports](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html) and [Setting up your monthly cost allocation report](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/configurecostallocreport.html)\.

Tags are not required for resources in Amazon Redshift, but they help provide context\. You might want to tag resources with metadata about cost centers, project names, and other pertinent information related to the resource\. For example, suppose you want to track which resources belong to a test environment and a production environment\. You could create a key named `environment` and provide the value `test` or `production` to identify the resources used in each environment\. If you use tagging in other AWS services or have standard categories for your business, we recommend that you create the same key\-value pairs for resources in Amazon Redshift for consistency\. 

Tags are retained for resources after you resize a cluster, and after you restore a snapshot of a cluster within the same region\. However, tags are not retained if you copy a snapshot to another region, so you must recreate the tags in the new region\. If you delete a resource, any associated tags are deleted\.

Each resource has one *tag set*, which is a collection of one or more tags assigned to the resource\. Each resource can have up to 50 tags per tag set\. You can add tags when you create a resource and after a resource has been created\. You can add tags to the following resource types in Amazon Redshift: 
+ CIDR/IP
+ Cluster
+ Cluster security group
+ Cluster security group ingress rule
+ Amazon EC2 security group
+ Hardware security module \(HSM\) connection
+ HSM client certificate
+ Parameter group
+ Snapshot
+ Subnet group

### Tagging requirements<a name="rs-tagging-requirements"></a>

Tags have the following requirements: 
+ Keys can't be prefixed with `aws:`\.
+ Keys must be unique per tag set\.
+ A key must be between 1 and 128 allowed characters\.
+ A value must be between 0 and 256 allowed characters\.
+ Values do not need to be unique per tag set\.
+ Allowed characters for keys and values are Unicode letters, digits, white space, and any of the following symbols: \_ \. : / = \+ \- @\. 
+ Keys and values are case sensitive\.