# Step 2: Configure SAML Assertions for Your IdP<a name="configuring-saml-assertions"></a>

After you create the IAM role, you need to define a claim rule in your IdP application that maps users or groups in your organization to the IAM role\. For more information, see [Configuring SAML Assertions for the Authentication Response](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_saml_assertions.html)\.

If you choose to use the optional GetClusterCredentials parameters DbUser, AutoCreate, and DbGroups, you can set the values for the parameters with your JDBC or ODBC connection or you can set the values by adding SAML Attribute elements to your IdP\. For more information about the DbUser, AutoCreate, and DbGroups parameters, see [Step 5: Configure a JDBC or ODBC Connection to Use IAM Credentials](generating-iam-credentials-configure-jdbc-odbc.md)\.

**Note**  
If you use an IAM policy variable `${redshift:DbUser}`, as described in [Resource Policies for GetClusterCredentials](redshift-iam-access-control-identity-based.md#redshift-policy-resources.getclustercredentials-resources) the value for `DbUser` is replaced with the value retrieved by the API operation's request context\. The Amazon Redshift drivers use the value for the `DbUser` variable provided by the connection URL, rather than the value supplied as a SAML attribute\.   
To help secure this configuration, we recommend that you use a condition in an IAM policy to validate the `DbUser` value with the `RoleSessionName`\. You can find examples of how to set a condition using an IAM policy in [Example Policy for Using GetClusterCredentials](redshift-iam-access-control-identity-based.md#redshift-policy-examples-getclustercredentials)\.

To configure your IdP to set the DbUser, AutoCreate, and DbGroups parameters, include the following Attribute elements:
+ An Attribute element with the Name attribute set to "https://redshift\.amazon\.com/SAML/Attributes/DbUser"

  Set the AttributeValue to the name of a user that will connect to the Amazon Redshift database\.

  The value in the AttributeValue element must be lowercase, begin with a letter, contain only alphanumeric characters, underscore \('\_'\), plus sign \('\+'\), dot \('\.'\), at \('@'\), or hyphen \('\-'\), and be less than 128 characters\. Typically, the user name is a user ID \(for example, bobsmith\) or an email address \(for example bobsmith@example\.com\)\. The value can't include a space \(for example, a user's display name such as Bob Smith\)\.

  ```
  <Attribute Name="https://redshift.amazon.com/SAML/Attributes/DbUser">
      <AttributeValue>user-name</AttributeValue>
  </Attribute>
  ```
+ An Attribute element with the Name attribute set to "https://redshift\.amazon\.com/SAML/Attributes/AutoCreate"

  Set the AttributeValue element to true to create a new database user if one doesn’t exist\. Set the AttributeValue to false to specify that the database user must exist in the Amazon Redshift database\.

  ```
  <Attribute Name="https://redshift.amazon.com/SAML/Attributes/AutoCreate">
      <AttributeValue>true</AttributeValue>
  </Attribute>
  ```
+ An Attribute element with the Name attribute set to "https://redshift\.amazon\.com/SAML/Attributes/DbGroups"

  This element contains one or more AttributeValue elements\. Set each AttributeValue element to a database group name that the DbUser joins for the duration of the session when connecting to the Amazon Redshift database\.

  ```
  <Attribute Name="https://redshift.amazon.com/SAML/Attributes/DbGroups">
      <AttributeValue>group1</AttributeValue>
      <AttributeValue>group2</AttributeValue>
      <AttributeValue>group3</AttributeValue>
  </Attribute>
  ```