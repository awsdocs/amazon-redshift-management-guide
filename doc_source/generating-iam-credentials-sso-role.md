# Step 1: Create an IAM Role for IAM Single Sign\-On \(SSO\) Access<a name="generating-iam-credentials-sso-role"></a>

If you don't use an identity provider for single\-sign on access, you can skip this step\.

If you already manage user identities outside of AWS, you can authenticate users for access to an Amazon Redshift database by integrating IAM authentication and a third\-party SAML\-2\.0 identity provider \(IdP\), such as ADFS, PingFederate, or Okta\.

For more information, see [Identity Providers and Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers.html) in the *AWS IAM User Guide*\.

Before you can use Amazon Redshift IdP authentication, you need to create an AWS SAML identity provider\. You create an identity provider in the IAM console to inform AWS about the IdP and its configuration\. Doing this establishes trust between your AWS account and the IdP\. For steps to create a role, see [Creating a Role for SAML 2\.0 Federation \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp_saml.html?icmpid=docs_iam_console)\. 