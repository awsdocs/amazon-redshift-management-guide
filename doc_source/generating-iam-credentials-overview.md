# Overview<a name="generating-iam-credentials-overview"></a>

Amazon Redshift provides the GetClusterCredentials API action to generate temporary database user credentials\. You can configure your SQL client with Amazon Redshift JDBC or ODBC drivers that manage the process of calling the GetClusterCredentials action\. They do so by retrieving the database user credentials, and establishing a connection between your SQL client and your Amazon Redshift database\. You can also use your database application to programmatically call the GetClusterCredentials action, retrieve database user credentials, and connect to the database\. 

If you already manage user identities outside of AWS, you can use a SAML 2\.0\-compliant identity provider \(IdP\) to manage access to Amazon Redshift resources\. You configure your IdP to permit your federated users access to an IAM Role\. With that IAM Role, you can generate temporary database credentials and log on to Amazon Redshift databases\. 

Your SQL client needs permission to call the GetClusterCredentials action on your behalf\. You manage those permissions by creating an IAM role and attaching an IAM permissions policy that grants or restricts access to the GetClusterCredentials action and related actions\. 

The policy also grants or restricts access to specific resources, such as Amazon Redshift clusters, databases, database user names, and user group names\. 

**Note**  
We recommend using the Amazon Redshift JDBC or ODBC drivers to manage the process of calling the GetClusterCredentials action and logging on to the database\. For simplicity, we assume that you are using a SQL client with the JDBC or ODBC drivers throughout this topic\. For specific details and examples of using the GetClusterCredentials action or the parallel get\-cluster\-credentials CLI action, see [GetClusterCredentials](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) and [get\-cluster\-credentials](https://docs.aws.amazon.com/cli/latest/reference/redshift/get-cluster-credentials.html)\.