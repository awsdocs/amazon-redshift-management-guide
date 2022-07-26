# Tagging resources<a name="serverless-tagging-resources"></a>

## Tagging resources overview<a name="serverless-tagging-resources"></a>

In AWS, tags are user\-defined labels that consist of key\-value pairs\. Amazon Redshift Serverless supports tagging to provide metadata about resources at a glance\. 

Tags are not required for resources, but they help provide context\. You might want to tag resources with metadata with information related to the resource\. For example, suppose you want to track which resources belong to a test environment and a production environment\. You could create a key named environment and provide the value test or production to identify the resources used in each environment\. If you use tagging in other AWS services or have standard categories for your business, we recommend that you create the same key\-value pairs for resources for consistency\. 

 If you delete a resource, any associated tags are deleted\. Currently, you can only use the AWS CLI to tag serverless resources\. Available operations are `TagResource`, `UntagResource`, and `ListTagsForResource`\. You cannot update a tag once it has been created\. 

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