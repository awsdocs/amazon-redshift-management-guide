# Working with Performance Metrics in the Amazon CloudWatch Console<a name="using-cloudwatch-console"></a>

When working with Amazon Redshift metrics in the Amazon CloudWatch console, there are couple of things you should keep in mind:

+ Query and load performance data is only available in the Amazon Redshift console\.

+ Some Metrics in the Amazon CloudWatch have different units than those used in the Amazon Redshift console\. For example, `WriteThroughput`, is displayed in GB/s \(as compared to Bytes/s in Amazon CloudWatch\) which is a more relevant unit for the typical storage space of a node\.

When working with Amazon Redshift metrics in the Amazon CloudWatch console, command line tools, or an Amazon SDK, there are two concepts to keep in mind\.

+ First, you specify the metric dimension to work with\. A dimension is a name\-value pair that helps you to uniquely identify a metric\. The dimensions for Amazon Redshift are `ClusterIdentifier` and `NodeID`\. In the Amazon CloudWatch console, the `Redshift Cluster` and `Redshift Node` views are provided to easily select cluster and node\-specific dimensions\. For more information about dimensions, see [Dimensions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/cloudwatch_concepts.html#Dimension) in the *Amazon CloudWatch Developer Guide*\.

+ Second, you specify the metric name, such as `ReadIOPS`\.

The following table summarizes the types of Amazon Redshift metric dimensions that are available to you\. All data is available in 1\-minute periods at no charge\. 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/using-cloudwatch-console.html)

Working with gateway and volume metrics is similar to working with other service metrics\. Many of the common tasks are outlined in the Amazon CloudWatch documentation and are listed below for your convenience: 

+ [View Available Metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html)

+ [Get Statistics for a Metric](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/getting-metric-statistics.html)

+ [Creating Amazon CloudWatch Alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)