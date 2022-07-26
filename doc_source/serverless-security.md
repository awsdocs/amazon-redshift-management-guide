# Security and connections in Amazon Redshift Serverless<a name="serverless-security"></a>

Access to Amazon Redshift requires credentials that AWS can use to authenticate your requests\. 

## Identity and access management in Amazon Redshift Serverless<a name="serverless-iam"></a>

Access to Amazon Redshift requires credentials that AWS can use to authenticate your requests\. Those credentials must have permissions to access AWS resources, such as Amazon Redshift Serverless\. 

The following sections provide details about how you can use AWS Identity and Access Management \(IAM\) and Amazon Redshift to help secure your resources by controlling who can access them\. For more information, see [Identity and access management in Amazon Redshift](redshift-iam-authentication-access-control.md)\.

### Granting permissions to Amazon Redshift Serverless<a name="serverless-security-other-services"></a>

To access other AWS services, Amazon Redshift Serverless requires permissions\.

#### Authorizing Amazon Redshift Serverless to access other AWS services for you<a name="serverless-security-other-services"></a>

Some Amazon Redshift features require Amazon Redshift to access other AWS services on your behalf\. For your Amazon Redshift Serverless instance to act for you, supply security credentials to it\. The preferred method to supply security credentials is to specify an AWS Identity and Access Management \(IAM\) role\. You can also create an IAM role through the Amazon Redshift console and set it as the default\. For more information, see [Creating an IAM role as default for Amazon Redshift](#serverless-default-iam-role)\.

To access other AWS services, create an IAM role with the appropriate permissions\. You also need to associate the role with Amazon Redshift Serverless\. In addition, either specify the Amazon Resource Name \(ARN\) of the role when you run the Amazon Redshift command or specify the `default` keyword\.

When changing the trust relationship for the IAM role in the [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/), make sure that you use `redshift.amazonaws.com` as the service name\. For information about how to manage IAM roles to access other AWS services on your behalf, see [Authorizing Amazon Redshift to access other AWS services on your behalf](authorizing-redshift-service.md)\.

#### Creating an IAM role as default for Amazon Redshift<a name="serverless-default-iam-role"></a>

When you create IAM roles through the Amazon Redshift console, Amazon Redshift programmatically creates the roles in your AWS account\. Amazon Redshift also automatically attaches existing AWS managed policies to them\. This approach means that you can stay within the Amazon Redshift console and don't have to switch to the IAM console for role creation\.

The IAM role that you create through the console for your cluster has the `AmazonRedshiftAllCommandsFullAccess` managed policy automatically attached\. This IAM role allows Amazon Redshift to copy, unload, query, and analyze data for AWS resources in your IAM account\. The related commands include COPY, UNLOAD, CREATE EXTERNAL FUNCTION, CREATE EXTERNAL TABLE, CREATE EXTERNAL SCHEMA, CREATE MODEL, and CREATE LIBRARY\. For more information about how to create an IAM role as default for Amazon Redshift, see [Creating an IAM role as default for Amazon Redshift](#serverless-default-iam-role)\.

To get started creating an IAM role as default for Amazon Redshift, open the AWS Management Console, choose the Amazon Redshift console, and then choose **Try Amazon Redshift Serverless**\. On the Amazon Redshift Serverless console, choose **Customize settings**\. Under **Permissions**, follow the procedures in [Using the console to manage IAM role associations](copy-unload-iam-role.md#managing-iam-role-association-with-cluster-console)\.

When you already have Amazon Redshift Serverless and want to configure IAM roles for it, open the AWS Management Console\. Choose the Amazon Redshift console, and then choose **Go to serverless**\. On the Amazon Redshift Serverless console, choose **Serverless configuration** and then **Data access**\. Under **Permissions**, follow the procedures in [Using the console to manage IAM role associations](copy-unload-iam-role.md#managing-iam-role-association-with-cluster-console)\.

##### Assigning IAM roles to a namespace<a name="serverless-endpoint-iam-role-namespace"></a>

Each IAM role is an AWS identity with permissions policies that determine what actions each role can perform in AWS\. The role is intended to be assumable by anyone who needs it\. Additionally, each namespace is a collection of objects, like tables and schemas, and users\. When you use Amazon Redshift Serverless, you can associate multiple IAM roles with your namespace\. This makes it easier to structure your permissions appropriately for a collection of database objects, so roles can perform actions on both internal and external data\. For example, so you can run a `COPY` command in an Amazon Redshift database to retrieve data from Amazon S3 and populate a Redshift table\.

You can associate multiple roles to a namespace using the console, as described previously in this section\. You can also use the API command `CreateNamespace`, or the CLI command `create-namespace`\. With the API or CLI command, you can assign IAM roles to the namespace by populating `IAMRoles` with one or more roles\. Specifically, you add ARNs for specific roles to the collection\.

##### Managing namespace associated IAM roles<a name="serverless-endpoint-iam-role-namespace-console"></a>

On the AWS Management Console you can manage permissions policies for roles in AWS Identity and Access Management\. You can manage IAM roles for the namespace, using settings available under **Namespace configuration**\. For more information about setting up the Amazon Redshift Serverless instance and assigning IAM roles, see [Setting up Amazon Redshift Serverless for the first time](serverless-console-getting-started.md)\. For more information about namespaces and their use in Amazon Redshift Serverless, see [Overview of Amazon Redshift Serverless workgroups and namespaces](serverless-workgroup-namespace.md)\.

### Getting started with IAM credentials for Amazon Redshift<a name="serverless-iam-credentials"></a>

When you sign in to the Amazon Redshift console for the first time and first try out Amazon Redshift Serverless, you can either log on as an IAM user or an IAM role\. After you get started creating an Amazon Redshift Serverless instance, Amazon Redshift records your IAM user name or role name that you used when you signed in\. You can use the same IAM credentials to sign in to the Amazon Redshift console and the Amazon Redshift Serverless console\.

While creating the Amazon Redshift Serverless instance, you can create a database\. Use the query editor v2 to connect to the database with the temporary credentials option\.

To add a new admin user name and password that persist for the database, choose **Customize admin user credentials** and enter a new admin user name and admin user password\. 

To get started using Amazon Redshift Serverless and create a workgroup and namespace in the console for the first time, use an IAM user or IAM role\. Make sure that this user or role has either the administrator permission ` arn:aws:iam::aws:policy/AdministratorAccess` or the full Amazon Redshift permission `arn:aws:iam::aws:policy/AmazonRedshiftFullAccess` attached to the IAM policy that you used\.

The following scenarios outline how your IAM credentials are used by Amazon Redshift Serverless when you get started on the Amazon Redshift Serverless console:
+ If you choose **Use default settings** – Amazon Redshift Serverless translates your current IAM identity to a database superuser\. You can use the same IAM identity with the Amazon Redshift Serverless console to perform superuser actions in your database in Amazon Redshift Serverless\.
+ If you choose **Customize settings** without specifying the **Admin user name** and password Amazon Redshift Serverless, your current IAM credentials are used as your default admin user credentials\. 
+ If you choose **Customize settings** and specify **Admin user name** and password Amazon Redshift Serverless – Amazon Redshift Serverless translates your current IAM identity to a database superuser\. Amazon Redshift Serverless also creates another long\-term login username and password pair also as a superuser\. You can either use your current IAM identity or the created username and password pair to login in to your database as a superuser\. 