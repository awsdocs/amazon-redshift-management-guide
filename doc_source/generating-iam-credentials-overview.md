# Overview<a name="generating-iam-credentials-overview"></a>

Amazon Redshift provides the [GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) API operation to generate temporary database user credentials\. You can configure your SQL client with Amazon Redshift JDBC or ODBC drivers that manage the process of calling the `GetClusterCredentials` operation\. They do so by retrieving the database user credentials, and establishing a connection between your SQL client and your Amazon Redshift database\. You can also use your database application to programmatically call the `GetClusterCredentials` operation, retrieve database user credentials, and connect to the database\. 

If you already manage user identities outside AWS, you can use an identity provider \(IdP\) compliant with Security Assertion Markup Language \(SAML\) 2\.0 to manage access to Amazon Redshift resources\. You configure your IdP to permit your federated users access to an IAM role\. With that IAM role, you can generate temporary database credentials and log in to Amazon Redshift databases\. 

Your SQL client needs permission to call the `GetClusterCredentials` operation for you\. You manage those permissions by creating an IAM role and attaching an IAM permissions policy that grants or restricts access to the `GetClusterCredentials` operation and related actions\. 

The policy also grants or restricts access to specific resources, such as Amazon Redshift clusters, databases, database user names, and user group names\. 

**Note**  
We recommend using the Amazon Redshift JDBC or ODBC drivers to manage the process of calling the `GetClusterCredentials` operation and logging on to the database\. For simplicity, we assume that you are using a SQL client with the JDBC or ODBC drivers throughout this topic\.   
For specific details and examples of using the `GetClusterCredentials` operation or the parallel `get-cluster-credentials` CLI command, see [GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) and [get\-cluster\-credentials](https://docs.aws.amazon.com/cli/latest/reference/redshift/get-cluster-credentials.html)\.

To manage authentication and authorization centrally, Amazon Redshift supports database authentication with IAM, enabling user authentication through enterprise federation\. Instead of creating an IAM user, in this approach you use existing identities from AWS Directory Service, your enterprise user directory, or a web identity provider\. These are known as federated users\. AWS assigns a role to a federated user when access is requested through an IdP\. 

To provide federated access to a user or client application in your organization to call Amazon Redshift API operations, you can also use the JDBC or ODBC driver with SAML 2\.0 support to request authentication from your organization IdP\. In this case, your organization's users don't have direct access to Amazon Redshift\.