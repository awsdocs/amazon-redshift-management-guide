# Overview of Amazon Redshift Serverless workgroups and namespaces<a name="serverless-workgroup-namespace"></a>

To isolate workloads and manage different resources in Amazon Redshift Serverless, you can create namespaces and workgroups and manage storage and compute resources separately\.

## Overview of Amazon Redshift Serverless workgroups and namespaces<a name="serverless-workgroups-and-namespaces"></a>

Namespace is a collection of database objects and users\. The storage\-related namespace groups together schemas, tables, users, or AWS Key Management Service keys for encrypting data\. Storage properties include the database name and password of the admin user, permissions, and encryption and security\. Other resources that are grouped under namespaces include datashares, recovery points, and usage limits\. You can configure these storage properties using the Amazon Redshift Serverless console, the AWS Command Line Interface, or the Amazon Redshift Serverless APIs for the specific resource\.

Workgroup is a collection of compute resources\. The compute\-related workgroup groups together compute resources like RPUs, VPC subnet groups, and security groups\. Properties for the workgroup include network and security settings\. Other resources that are grouped under workgroups include access and usage limits\. You can configure these compute properties using the Amazon Redshift Serverless console, the AWS Command Line Interface, or the Amazon Redshift Serverless APIs\.

You can create one or more namespaces and workgroups\. Each namespace can have only one workgroup associated with it\. Conversely, each workgroup can be associated with only one namespace\.

### Getting started with Amazon Redshift Serverless using the console<a name="serverless-workgroups-and-namespaces-console"></a>

Setting up Amazon Redshift Serverless involves walking through several configuration steps\. When you follow the steps to set up Amazon Redshift Serverless, you create a namespace and workgroup, and associate them with each other\. To get started setting Amazon Redshift Serverless configuration using the Amazon Redshift Serverless console, you can choose **Get started with Amazon Redshift Serverless** to set up Amazon Redshift Serverless and begin to interact with it\. You can choose an environment with default settings, which makes for quicker setup, or explicitly configure the settings per your organization's requirements\. During this process, you specify settings for your workgroup and namespace\.

After you set up the environment, [Workgroup properties](serverless-console-workgroups.md#serverless-workgroup-describe) and [Namespace properties ](serverless-console-configure-namespace-working.md#serverless-console-namespace-config) help you get familiar with the settings\.

### Managing Amazon Redshift Serverless using the AWS Command Line Interface and Amazon Redshift Serverless API<a name="serverless-workgroups-and-namespaces-cli"></a>

To create a namespace, you can use the AWS Command Line Interface operation `create-namespace` or the Amazon Redshift Serverless API operation `CreateNamespace`\. Amazon Redshift Serverless creates a default namespace with a default AWS Key Management Service key\. You can also specify another key to encrypt your data\. Alternatively, you can use the AWS Command Line Interface operation `restore-from-snapshot` to restore an existing namespace from a snapshot\. Amazon Redshift Serverless then populates the namespace with data restored from the specified snapshot\. You can also use the API operation `RestoreFromSnapshot`\.

To create a workgroup, ensure that you have an existing namespace\. You can use the AWS Command Line Interface operation `create-workgroup` or the Amazon Redshift Serverless API operation `CreateWorkgroup`\. Specify a target namespace that the workgroup is associated to\. You can specify any compute resources while creating the workgroup, such as subnets, security groups, or RPUs\. Once created, you can access the namespace that the workgroup is associated to\.

To update a namespace or workgroup, you can use the AWS Command Line Interface operations `update-namespace` or `update-workgroup` or the Amazon Redshift Serverless API operations `UpdateNamespace` or `UpdateWorkgroup`\.

To retrieve the list of instances of namespaces and workgroups, use the AWS Command Line Interface operations `list-namespaces` or `list-workgroups` or the Amazon Redshift Serverless API operations `ListNamespaces` or `ListWorkgroups`\.

To retrieve the contents of or metadata for namespaces or workgroups, use the AWS Command Line Interface operations `get-namespaces` or `get-workgroups` or use the Amazon Redshift Serverless API operations `GetNamespace` or `GetWorkgroup`\.

To delete a workgroup, use the AWS Command Line Interface operations `delete-workgroup` or the Amazon Redshift Serverless API operation `DeleteWorkgroup`\. You won't be able to access the database after deleting a workgroup\.

To delete a namespace, first delete the associated workgroup\. Use the AWS Command Line Interface operation `delete-namespace` or the Amazon Redshift Serverless API operation `DeleteNamespace`\. Amazon Redshift Serverless then deletes all the data in the specified namespace\. Prior to the namespace deletion, you can take a final snapshot on the specified namespace to allow you to restore data from the snapshot, if needed\. To do so, use the AWS Command Line Interface operation `create-snapshot` or the Amazon Redshift Serverless API operation `CreateSnapshot` before you use the AWS Command Line Interface operation `delete-namespace` or the Amazon Redshift Serverless API operation `DeleteNamespace`\.