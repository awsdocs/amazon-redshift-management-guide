# Tagging resources overview<a name="serverless-tagging-resources"></a>

In AWS, tags are user\-defined labels that consist of key\-value pairs\. Amazon Redshift Serverless supports tagging to provide metadata about resources at a glance\. 

Tags are not required for resources, but they help provide context\. You might want to tag resources with metadata with information related to the resource\. For example, suppose you want to track which resources belong to a test environment and a production environment\. You could create a key named environment and provide the value test or production to identify the resources used in each environment\. If you use tagging in other AWS services or have standard categories for your business, we recommend that you create the same key\-value pairs for resources for consistency\. 

 If you delete a resource, any associated tags are deleted\. You can use both the AWS CLI and Amazon Redshift Serverless console to tag serverless resources\. Available API operations are `TagResource`, `UntagResource`, and `ListTagsForResource`\. 

Each resource has one tag set, which is a collection of one or more tags assigned to the resource\. Each resource can have up to 50 tags per tag set\. You can add tags when you create a resource and after a resource has been created\. You can add tags to the following serverless resource types: 
+ Workgroups
+ Namespaces

Tags have the following requirements:
+ Keys can't be prefixed with `aws:`\.
+ Keys must be unique per tag set\.
+ A key must be between 1 and 128 allowed characters\.
+ A value must be between 0 and 256 allowed characters\.
+ Values do not need to be unique per tag set\.
+ Allowed characters for keys and values are Unicode letters, digits, white space, and any of the following symbols: \_ \. : / = \+ \- @\. 
+ Keys and values are case sensitive\.

To manage tags of your Amazon Redshift Serverless resources

1. On the Amazon Redshift Serverless console, choose **Manage Tags**\.

1. Enter the resource type to search for and choose **Search resources**\. Choose the resource\(s\) for which you want to manage tags, then choose **Manage tags**\.

1. Specify the keys and optional values you want to add to the resource\(s\)\. When modifying a tag, you can change the tag's value, but not the key\.

1. After you're done adding, removing, or modifying tags, choose **Save changes**, then choose **Apply** to save your changes\.