# Managing usage limits in Amazon Redshift<a name="managing-cluster-usage-limits"></a>

You can define limits to monitor and control your usage and associated cost of some Amazon Redshift features\. You can create daily, weekly, and monthly usage limits, and define actions that Amazon Redshift automatically takes if those limits are reached\. Actions include such things as logging an event to a system table to record usage exceeding your defined limits\. Other possible actions include raising alerts with Amazon SNS and Amazon CloudWatch to notify an administrator and disabling further usage to control costs\. 

You can define usage limits for each cluster\. After your cluster is created, you can define usage limits for the following features: 
+ Amazon Redshift Spectrum
+ Amazon Redshift Concurrency Scaling

Usage limits are available with release version 1\.0\.14677 or later in the AWS Regions where Amazon Redshift Spectrum and Amazon Redshift Concurrency Scaling are available\. 

A Redshift Spectrum limit specifies the threshold of the total amount of data scanned in 1\-TB increments\. A concurrency scaling limit specifies the threshold of the total amount of time used by concurrency scaling in 1\-minute increments\. A limit can be specified for a daily, weekly, or monthly period \(using UTC to determine the start and end of the period\)\. If you create a limit in the middle of a period, then the limit is measured from that point to the end of the period\. For example, if you create a monthly limit on March 15, then the first monthly period is measured from March 15 through March 31\. 

You can define multiple usage limits for each feature\. Each limit can have a different action\. Possible actions include the following:
+ **Log to system table** – This is the default action\. Information is logged to the STL\_USAGE\_CONTROL table\. Logging is helpful when evaluating past usage and in deciding on future usage limits\. For more information about what is logged, see [STL\_USAGE\_CONTROL](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_USAGE_CONTROL.html) in the *Amazon Redshift Database Developer Guide*\. 
+ **Alert** – Amazon Redshift emits CloudWatch metrics for available and consumed usage\. You can define up to three usage limits for each feature\. If you enable the alert action using the Amazon Redshift console, a CloudWatch alarm is automatically created on these metrics\. You can optionally attach an Amazon SNS subscription to that alarm\. If you are using an AWS CLI or API operation, make sure that you create the CloudWatch alarm manually\. When the threshold is reached, events are also logged to a system table\. 
+ **Disable feature** – When the threshold is reached, Amazon Redshift disables the feature until the quota is refreshed for the next time period \(daily, weekly, or monthly\)\. Only one limit for each feature can have the disable action\. Events are also logged to a system table, and alerts can be emitted\. 

Usage limits persist until the usage limit definition itself or the cluster is deleted\.  

You can define and manage usage limits with the new Amazon Redshift console, the AWS CLI, or with Amazon Redshift API operations\. To define a limit on the Amazon Redshift console, navigate to your cluster and choose **Configure usage limit** for **Actions**\. To view previously defined usage limits for your cluster, navigate to your cluster, and choose the **Maintenance and monitoring** tab, **Usage limits** section\. To view the amount of usage available and consumed for your cluster, navigate to your cluster\. Choose the **Cluster performance** tab, then view the graphs for the usage consumed for a feature\. 

You can use the following Amazon Redshift CLI operations to manage usage limits\. For more information, see the *AWS CLI Command Reference*\.
+ [create\-usage\-limit](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-usage-limit.html)
+ [describe\-usage\-limits](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-usage-limits.html)
+ [modify\-usage\-limit](https://docs.aws.amazon.com/cli/latest/reference/redshift/modify-usage-limit.html)
+ [delete\-usage\-limit](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-usage-limit.html)

You can use the following Amazon Redshift API operations to manage usage limits\. For more information, see the *Amazon Redshift API Reference*\.
+ [CreateUsageLimit](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateUsageLimit.html)
+ [DescribeUsageLimits](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeUsageLimits.html)
+ [ModifyUsageLimit](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyUsageLimit.html)
+ [DeleteUsageLimit](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteUsageLimit.html)

To learn how to create and monitor usage limits using the the Amazon Redshift console, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/bXg4xLiDqcM/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/bXg4xLiDqcM)