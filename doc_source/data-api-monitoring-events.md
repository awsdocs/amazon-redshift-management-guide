# Monitoring events for the Amazon Redshift Data API in Amazon EventBridge<a name="data-api-monitoring-events"></a>

You can monitor Data API events in EventBridge, which delivers a stream of real\-time data from your own applications, software\-as\-a\-service \(SaaS\) applications, and AWS services\. EventBridge routes that data to targets such as AWS Lambda and Amazon SNS\. These events are the same as those that appear in CloudWatch Events, which delivers a near\-real time stream of system events that describe changes in AWS resources\. Events are sent to the account that contains the Amazon Redshift database\. For example, if you assume a role in another account, events are sent to that account\. For more information, see [Amazon EventBridge events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-events.html) in the *Amazon EventBridge User Guide\.*\. 

Data API events are sent when the `ExecuteStatement` or `BatchExecuteStatement` API operation sets the `WithEvent` option to `true`\. The `state` field of the event contains one of the following values: 
+ ABORTED – The query run was stopped by the user\. 
+ FAILED – The query run failed\. 
+ FINISHED – The query has finished running\. 

Events are delivered on a guaranteed basis\. For more information, see [Events from AWS services](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html) in the *Amazon EventBridge User Guide*\. 

## Example for Data API finished event<a name="data-api-monitoring-events-finished"></a>

The following example shows an event for the Data API when the `ExecuteStatement` API operation finished\. In this example, a statement named `test.testtable` finished running\.

```
{
    "version": "0",
    "id": "18e7079c-dd4b-dd64-caf9-e2a31640dab0",
    "detail-type": "Redshift Data Statement Status Change",
    "source": "aws.redshift-data",
    "account": "123456789012",
    "time": "2020-10-01T21:14:26Z",
    "region": "us-east-1",
    "resources": [
        "arn:aws:redshift:us-east-1:123456789012:cluster:redshift-cluster-1"
    ],
    "detail": {
        "principal": "arn:aws:iam::123456789012:user/myuser",
        "statementName": "test.testtable",
        "statementId": "dd2e1ec9-2ee3-49a0-819f-905fa7d75a4a",
        "redshiftQueryId": -1,
        "state": "FINISHED",
        "rows": 1,
        "expireAt": 1601673265
    }
}
```