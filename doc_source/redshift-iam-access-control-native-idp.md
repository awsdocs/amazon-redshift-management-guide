# Native identity provider \(IdP\) federation for Amazon Redshift<a name="redshift-iam-access-control-native-idp"></a>

Managing identities and permissions for Amazon Redshift is made easier with native identity provider federation because it leverages your existing identity provider to simplify authentication and managing permissions\. It does this by making it possible to share identity metadata to Redshift from your identity provider\. For the first iteration of this feature, the supported identity provider is [Microsoft Azure Active Directory \(Azure AD\)](https://azure.microsoft.com/en-us/services/active-directory/)\. 

To configure Amazon Redshift so it can authenticate identities from the third\-party identity provider, you register the identity provider with Amazon Redshift\. Doing this enables Redshift to authenticate users and roles defined by the identity provider\. Thus you can avoid having to perform granular identity management in both your third\-party identity provider and in Amazon Redshift, because identity information is shared\.

## Setting up the identity provider on Amazon Redshift<a name="redshift-iam-access-control-native-idp-setup"></a>

This section shows the steps to configure the identity provider and Amazon Redshift to establish communication for native identity provider federation\. You need an active account with your identity provider\. Prior to configuring Amazon Redshift, you register Redshift as an application with your identity provider, granting administrator consent\.

Complete the following steps in Amazon Redshift:

1. You run a SQL statement to register the identity provider, including descriptions of the Azure application metadata\. To create the identity provider in Amazon Redshift, run the following command after replacing the parameter values *issuer*, *client\_id*, *client\_secret*, and *audience*\. These parameters are specific to Microsoft Azure AD\. Replace the identity provider name with a name of your choosing, and replace the namespace with a unique name to contain users and roles from your identity provider directory\.

   ```
   CREATE IDENTITY PROVIDER oauth_standard TYPE azure
   NAMESPACE 'aad'
   PARAMETERS '{
   "issuer":"https://sts.windows.net/2sdfdsf-d475-420d-b5ac-667adad7c702/",
   "client_id":"<client_id>",
   "client_secret":"BUAH~ewrqewrqwerUUY^%tHe1oNZShoiU7",
   "audience":["https://analysis.windows.net/powerbi/connector/AmazonRedshift"]
   }'
   ```

   The type `azure` indicates that the provider specifically facilitates communication with Microsoft Azure AD\. This is currently the only supported third\-party identity provider\.
   + *issuer* \- The issuer ID to trust when a token is received\. The unique identifier for the *tenant\_id* is appended to the issuer\.
   + *client\_id* \- The unique, public identifier of the application registered with the identity provider\. This can be referred to as the application ID\.
   + *client\_secret* \- A secret identifier, or password, known only to the identity provider and the registered application\.
   + *audience* \- The Application ID that is assigned to the application in Azure\.

   

   Instead of using a shared client secret, you can set parameters to specify a certificate, a private key, and a private key password when you create the identity provider\.

   ```
   CREATE IDENTITY PROVIDER example_idp TYPE azure 
   NAMESPACE 'example_aad' 
   PARAMETERS '{"issuer":"https://sts.windows.net/2sdfdsf-d475-420d-b5ac-667adad7c702/", 
   "client_id":"<client_id>", 
   "audience":["https://analysis.windows.net/powerbi/connector/AmazonRedshift"], 
   "client_x5t":"<certificate thumbprint>", 
   "client_pk_base64":"<private key in base64 encoding>", 
   "client_pk_password":"test_password"}';
   ```

   The private key password, *client\_pk\_password*, is optional\.

1. Optional: Run SQL commands in Amazon Redshift to pre\-create users and roles\. This facilitates granting permissions in advance\. The role name in Amazon Redshift is like the following: *<Namespace>:<GroupName on Azure AD>*\. For example, when you create a group in Microsoft Azure AD called `rsgroup` and a namespace called `aad`, the role name is `aad:rsgroup`\. The user name and group memberships are mapped to a user and roles in Amazon Redshift, in the identity provider namespace\.

   The mapping for roles and users includes verifying their `external_id` value, to ensure it's up to date\. The external ID maps to the identifier of the group or user in the identity provider\. For example, a role's external ID maps to the corresponding Azure AD group ID\. Similarly, each user's external ID maps to their ID in the identity provider\.

   ```
   create role "aad:rsgroup";
   ```

1. Grant relevant permissions to roles per your requirements\. For example:

   ```
   GRANT SELECT on all tables in schema public to role "aad:rsgroup";
   ```

1. You can also grant permissions to a specific user\.

   ```
   GRANT SELECT on table foo to aad:alice@example.com
   ```

   Note that a federated external user's role membership is available only in that user's session\. This has implications for creating database objects\. When a federated external user creates any view or stored procedure, for instance, the same user can't delegate permission of those objects to other users and roles\.

**An explanation of namespaces**

A namespace maps a user or role to a specific identity provider\. For example, the prefix for users created in AWS IAM is `iam:`\. This prefix prevents user name collisions and makes support for multiple identity stores possible\.  If a user alice@example\.com from the identity source registered with *aad* namespace logs in, the user `aad:alice@example.com` is created in Redshift if it doesn't already exist\. Note that a user and role namespace has a different function than an Amazon Redshift cluster namespace, which is a unique identifier associated with a cluster\.

## How login works with native identity provider \(IdP\) federation<a name="redshift-iam-access-control-native-idp-login"></a>

 To complete the preliminary setup between the identity provider and Amazon Redshift, you perform a couple of steps: First, you register Amazon Redshift as a third\-party application with your identity provider, requesting the necessary API permissions\. Then you create users and groups in the identity provider\. Last, you register the identity provider with Amazon Redshift, using SQL statements, which set authentication parameters that are unique to the identity provider\. As part of registering the identity provider with Redshift, you assign a namespace to make sure users and roles are grouped correctly\. 

 With the identity provider registered with Amazon Redshift, communication is set up between Redshift and the identity provider\. A  client can then pass tokens and authenticate to Redshift as an identity provider entity\. Amazon Redshift uses the IdP group membership information to map to Redshift roles\. If the user doesn't previously exist in Redshift, the user is created\. Roles are created that map to identity provider groups, if they don't exist\. The Amazon Redshift administrator grants permission on the roles, and users can run queries and perform other database tasks\. 

The following steps outline how native identity provider federation works, when a user logs in:

1. When a user logs in using the native IdP option, from the client, the identity provider token is sent from the client to the driver\.

1. The user is authenticated\. If the user doesn't already exist in Amazon Redshift, a new user is created\. Redshift maps the user's identity provider groups to Redshift roles\. 

1. Permissions are assigned, based on the user's Redshift roles\. These are granted to users and roles by an administrator\.

1. The user can query Redshift\.

## Using desktop client tools to connect to Amazon Redshift<a name="redshift-iam-access-control-native-idp-oauth"></a>

For instructions on how to use native identity provider federation to connect to Amazon Redshift with Power BI, see the blog post [Integrate Amazon Redshift native IdP federation with Microsoft Azure Active Directory \(AD\) and Power BI](http://aws.amazon.com/blogs/big-data/integrate-amazon-redshift-native-idp-federation-with-microsoft-azure-ad-and-power-bi/)\. It describes a step\-by\-step implementation of the Amazon Redshift native IdP setup with Azure AD\. It details the steps to set up the client connection for either Power BI Desktop or the Power BI service\. The steps include application registration, configuring permissions, and configuring credentials\.

To learn how to integrate Amazon Redshift native IdP federation with Azure AD, using Power BI Desktop and JDBC Client\-SQL Workbench/J, watch the following video:

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/S3MQLvZ-NiI/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/S3MQLvZ-NiI)

For instructions on how to use native identity provider federation to connect to Amazon Redshift with a SQL client, specifically DBeaver or SQL Workbench/J, see the blog post [Integrate Amazon Redshift native IdP federation with Microsoft Azure AD using a SQL client](http://aws.amazon.com/blogs/big-data/integrate-amazon-redshift-native-idp-federation-with-microsoft-azure-ad-using-a-sql-client/)\.