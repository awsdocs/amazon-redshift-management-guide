# Defining database roles to grant to federated users in Amazon Redshift Serverless<a name="redshift-iam-access-federated-db-roles"></a>

When you're part of an organization, you have a collection of associated roles\. For instance, you have roles for your job function, like *programmer* and *manager*\. Your roles determine which applications and data you have access to\. Most organizations use an identity provider, such as Microsoft Active Directory, to assign roles to users and groups\. The use of roles to control resource access has grown, because organizations don't have to do as much management of individual users\.

Recently, role\-based access control was introduced in Amazon Redshift Serverless\. Using database roles, you can secure access to data and objects, like schemas or tables, for example\. Or you can use roles to define a set of elevated permissions, such as for a system monitor or database administrator\. But after you grant resource permissions to database roles, there is an additional step, which is to connect a user's roles from the organization to the database roles\. You can assign each user to their database roles upon initial sign in by running SQL statements, but it's a lot of effort\. An easier way is to define the database roles to grant and pass them to Amazon Redshift Serverless\. This has the advantage of simplifying the initial sign\-in process\.

You can pass roles to Amazon Redshift Serverless using `GetCredentials`\. When a user signs in for the first time to an Amazon Redshift Serverless database, an associated database user is created and mapped to the matching database roles\. This topic details the mechanism for passing roles to Amazon Redshift Serverless\.

Passing  database roles has a couple primary use cases:
+ When a user signs in through a third\-party identity provider, typically with federation configured, and passes the roles by means of a session tag\.
+ When a user signs in through IAM sign\-in credentials, and their roles are passed by means of a tag key and value\. 

For more information about role\-based access control, see [Role\-based access control \(RBAC\)](https://docs.aws.amazon.com/redshift/latest/dg/t_Roles.html)\.

## Configuring database roles<a name="redshift-iam-access-federated-db-roles-configuration"></a>

Before you can pass roles to Amazon Redshift Serverless, you must configure database roles in your database and grant them appropriate permissions on database resources\. For instance, in a simple scenario, you can create a database role named *sales* and grant it access to query tables with sales data\. For more information about how to create database roles and grant permissions, see [CREATE ROLE](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_ROLE.html) and [GRANT](https://docs.aws.amazon.com/redshift/latest/dg/r_GRANT.html)\.

## Use cases for defining database roles to grant to federated users<a name="redshift-iam-access-federated-db-roles-use-cases"></a>

These sections outline a couple use cases where passing database roles to Amazon Redshift Serverless can simplify access to database resources\.

### Signing in using an identity provider<a name="redshift-iam-access-federated-db-roles-idp-principal"></a>

The first use case assumes that your organization has user identities in an identity and access management service\. This service can be cloud based, for example JumpCloud or Okta, or on\-premises, such as Microsoft Active Directory\. The goal is to automatically map a user's roles from the identity provider to your database roles when they sign in to a client like Query editor V2, for instance, or with a JDBC client\. To set this up, you must complete a couple of configuration tasks\. These include the following:

1. Configure federated integration with your identity provider \(IdP\)  using a trust relationship\. This is a prerequisite\. When you set this up, the identity provider is responsible for authenticating the user via a SAML assertion and providing sign\-in credentials\. For more information, see [Integrating third party SAML solution providers with AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml_3rd-party.html)\. You can also find more information at [Federate access to Amazon Redshift query editor V2 with Active Directory Federation Services \(AD FS\)](http://aws.amazon.com/blogs//big-data/federate-access-to-amazon-redshift-query-editor-v2-with-active-directory-federation-services-ad-fs-part-3/) or [Federate single sign\-on access to Amazon Redshift query editor v2 with Okta](http://aws.amazon.com/blogs//big-data/federate-single-sign-on-access-to-amazon-redshift-query-editor-v2-with-okta/)\.

1. The user must have the following policy permissions: 
   + `GetCredentials` – Provides credentials for temporary authorization to log in to Amazon Redshift Serverless\.
   + `sts:AssumeRoleWithSAML` – Provides a mechanism for tying an enterprise identity store or directory to role\-based AWS access\.
   + `sts:TagSession` – Permission to the tag\-session action, on the identity provider principal\.

    In this case, `AssumeRoleWithSAML` returns a set of security credentials for users who have been authenticated via a SAML authenticated response\. This operation provides a mechanism for tying an identity store or directory to role\-based AWS access without user\-specific credentials\. For users with permission to `AssumeRoleWithSAML`, the identity provider is responsible for managing the SAML assertion that is used to pass the role information\.

   As a best practice, we recommend attaching permissions policies to an IAM role and then assigning it to users and groups as needed\. For more information, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

1. You configure the tag `RedshiftDbRoles` with the colon\-separated role values, in the format *role1:role2*\. For example, `manager:engineer`\. These can be retrieved from a session\-tag implementation configured in your identity provider\. The SAML authentication request passes the roles programmatically\. For more information about passing session tags, see [Passing session tags in AWS STS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html)\.

   In a case where you pass a role name that doesn't exist in the database, it's ignored\.

In this use case, when a user signs in  using a federated identity, their roles are passed in the authorization request through the session tag key and value\. Subsequently, following authorization, `GetCredentials` passes the roles to the database\. Upon a successful connection, the database roles are mapped and the user can perform database tasks corresponding with their role\. The essential part of the operation is that the `RedshiftDbRoles` session tag is assigned the roles in the initial authorization request\. For more information about passing session tags, see [Passing session tags using AssumeRoleWithSAML](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_session-tags.html#id_session-tags_adding-assume-role-saml)\.

### Signing in using IAM credentials<a name="redshift-iam-access-federated-db-roles-iam-creds"></a>

In the second use case, roles can be passed for a user and they can access a database client application through IAM credentials\.

1. The user who signs in in this case must be assigned policy permissions for the following actions:
   + `tag:GetResources` – Returns tagged resources associated with specified tags\.
   + `tag:GetTagKeys` – Returns tag keys currently in use\.

     As a best practice, we recommend attaching permissions policies to an IAM role and then assigning it to users and groups as needed\. For more information, see [Identity and access management in Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/redshift-iam-authentication-access-control.html)\.

1. Allow permissions are also required to access the database service, such as Amazon Redshift Serverless\.

1. For this use case, configure the tag values for your roles in AWS Identity and Access Management\. You can choose to **edit tags** and create a tag key called *RedshiftDbRoles* with an accompanying tag value string that contains the roles\. For example, *manager:engineer*\.

When a user logs in, their role is added to the authorization request and passed to the database\. It is mapped to an existing database role\.

## Additional resources<a name="redshift-iam-access-federated-db-roles-resources"></a>

As mentioned in the use cases, you can configure the trust relationship between your IdP and AWS\. For more information, see [Configuring your SAML 2\.0 IdP with relying party trust and adding claims](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_relying-party.html)\. 