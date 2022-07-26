# Amazon Redshift API permissions reference<a name="redshift-policy-resources.resource-permissions"></a>

When you set up [Access control](redshift-iam-authentication-access-control.md#redshift-iam-accesscontrol), you write permission policies that you can attach to an IAM identity \(identity\-based policies\)\. For detailed reference information, see the following topics in the *Service Authorization Reference*:
+ [Actions, resources, and condition keys for Amazon Redshift](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonredshift.html)\.
+ [Actions, resources, and condition keys for Amazon Redshift Serverless](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonredshiftserverless.html)\.
+ [Actions, resources, and condition keys for Amazon Redshift Data API](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonredshiftdataapi.html)\.
+ [Actions, resources, and condition keys for AWS SQL Workbench](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssqlworkbench.html) which list actions for Amazon Redshift query editor v2\.

This reference contains information about which API operations can be used in an IAM policy\. It also includes the AWS resource for which you can grant the permissions, and condition keys that you can include for fine\-grained access control\. For more information about conditions, see [Using IAM policy conditions for fine\-grained access control](redshift-iam-access-control-overview.md#redshift-policy-resources.conditions)\. 

You specify the actions in the policy's `Action` field, the resource value in the policy's `Resource` field, and conditions in the policy's `Condition` field\. To specify an action for Amazon Redshift, use the `redshift:` prefix followed by the API operation name \(for example, `redshift:CreateCluster`\)\.