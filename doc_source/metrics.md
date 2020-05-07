# Monitoring Amazon Redshift cluster performance<a name="metrics"></a>

Amazon Redshift provides performance metrics and data so that you can track the health and performance of your clusters and databases\. In this section, we discuss the types of data that you can work with in Amazon Redshift, specifically in the Amazon Redshift console\. 

**Topics**
+ [Overview](#metrics-overview)
+ [Amazon Redshift performance data](metrics-listing.md)
+ [Working with performance data in the Amazon Redshift console](performance-metrics-console.md)

## Overview<a name="metrics-overview"></a>

The performance data that you can use in the Amazon Redshift console falls into two categories:
+ **Amazon CloudWatch metrics – **Amazon CloudWatch metrics help you monitor physical aspects of your cluster, such as CPU utilization, latency, and throughput\. Metric data is displayed directly in the Amazon Redshift console\. You can also view it in the CloudWatch console\. Alternatively, you can consume it in any other way you work with metrics, such as with the AWS CLI or one of the AWS SDKs\. 
+ **Query/Load performance data – **Performance data helps you monitor database activity and performance\. This data is aggregated in the Amazon Redshift console to help you easily correlate what you see in CloudWatch metrics with specific database query and load events\. You can also create your own custom performance queries and run them directly on the database\. Query and load performance data is displayed only in the Amazon Redshift console\. It is not published as CloudWatch metrics\. 

Performance data is integrated into the Amazon Redshift console, yielding a richer experience in the following ways:
+ Performance data associated with a cluster is displayed contextually when you view a cluster, where you might need it to make decisions about the cluster such as resizing\.
+ Some performance metrics are displayed in more appropriately scaled units in the Amazon Redshift console as compared to CloudWatch\. For example, `WriteThroughput`, is displayed in GB/s \(as compared to bytes/s in CloudWatch\), which is a more relevant unit for the typical storage space of a node\.
+ You can easily display performance data for the nodes of a cluster together on the same graph\. This way, you can easily monitor the performance of all nodes of a cluster\. You can also view performance data for each node\. 

Amazon Redshift provides performance data \(both CloudWatch metrics and query and load data\) at no additional charge\. Performance data is recorded every minute\. You can access historical values of performance data in the Amazon Redshift console\. For detailed information about using CloudWatch to access the Amazon Redshift performance data that is exposed as CloudWatch metrics, see [What is CloudWatch?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatch.html.html) in the *Amazon CloudWatch User Guide*\. 