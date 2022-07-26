# Working with performance metrics in the CloudWatch console<a name="using-cloudwatch-console"></a>

When working with Amazon Redshift metrics in the CloudWatch console, keep a couple of things in mind:
+ Query and load performance data is only available in the Amazon Redshift console\.
+ Some Metrics in the CloudWatch have different units than those used in the Amazon Redshift console\. For example, `WriteThroughput` is displayed in GB/s \(as compared to Bytes/s in CloudWatch\), which is a more relevant unit for the typical storage space of a node\.

When working with Amazon Redshift metrics in the CloudWatch console, command line tools, or an Amazon SDK, keep these concepts in mind:

1. First, specify the metric dimension to work with\. A dimension is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Amazon Redshift are `ClusterIdentifier` and `NodeID`\. In the CloudWatch console, the `Redshift Cluster` and `Redshift Node` views are provided to easily select cluster and node\-specific dimensions\. For more information about dimensions, see [Dimensions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_concepts.html#Dimension) in the *CloudWatch Developer Guide*\.

1. Then, specify the metric name, such as `ReadIOPS`\.

The following table summarizes the types of Amazon Redshift metric dimensions that are available to you\. Depending on the metric, data is available in either 1\-minute or 5\-minute intervals at no charge\. For more information, see [Amazon Redshift metrics](metrics-listing.md#redshift-metrics)\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/using-cloudwatch-console.html)

Working with gateway and volume metrics is similar to working with other service metrics\. Many of the common tasks are outlined in the CloudWatch documentation, including the following: 
+ [View available metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)
+ [Get statistics for a metric](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/getting-metric-statistics.html)
+ [Creating CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)