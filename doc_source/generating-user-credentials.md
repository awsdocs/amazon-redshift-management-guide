# Using IAM authentication to generate database user credentials<a name="generating-user-credentials"></a>

You can generate temporary database credentials based on permissions granted through an AWS Identity and Access Management \(IAM\) permissions policy to manage the access that your users have to your Amazon Redshift database\. 

Commonly, Amazon Redshift database users log in to the database by providing a database user name and password\. However, you don't have to maintain user names and passwords in your Amazon Redshift database\. As an alternative, you can configure your system to permit users to create user credentials and log in to the database based on their IAM credentials\.

For more information, see [Identity Providers and Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html) in the *IAM User Guide*\.

**Topics**
+ [Overview](generating-iam-credentials-overview.md)
+ [Creating temporary IAM user credentials](generating-iam-credentials-steps.md)
+ [Options for providing IAM credentials](options-for-providing-iam-credentials.md)