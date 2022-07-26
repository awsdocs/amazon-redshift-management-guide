# Configuring the Amazon Redshift Python connector<a name="python-redshift-driver"></a>

By using the Amazon Redshift connector for Python, you can integrate work with [the AWS SDK for Python \(Boto3\)](https://github.com/boto/boto3), and also pandas and Numerical Python \(NumPy\)\. For more information on pandas, see the [pandas GitHub repository](https://github.com/pandas-dev/pandas)\. For more information on NumPy, see the [NumPy GitHub repository](https://github.com/numpy/numpy)\.  

The Amazon Redshift Python connector provides an open source solution\. You can browse the source code, request enhancements, report issues, and provide contributions\. 

To use the Amazon Redshift Python connector, make sure that you have Python version 3\.6 or later\. For more information, see the [Amazon Redshift Python driver license agreement](https://github.com/aws/amazon-redshift-python-driver/blob/master/LICENSE)\. 

The Amazon Redshift Python connector provides the following:
+ AWS Identity and Access Management \(IAM\) authentication\. For more information, see [Identity and access management in Amazon Redshift](redshift-iam-authentication-access-control.md)\.
+ Identity provider authentication using federated API access\. Federated API access is supported for corporate identity providers such as the following:
  + Azure AD\. For more information, see the AWS Big Data blog post [Federate Amazon Redshift access with Microsoft Azure AD single sign\-on](http://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-microsoft-azure-ad-single-sign-on/)\.
  + Active Directory Federation Services\. For more information, see the AWS Big Data blog post [Federate access to your Amazon Redshift cluster with Active Directory Federation Services \(AD FS\): Part 1](http://aws.amazon.com/blogs/big-data/federate-access-to-your-amazon-redshift-cluster-with-active-directory-federation-services-ad-fs-part-1/)\. 
  + Okta\. For more information, see the AWS Big Data blog post [Federate Amazon Redshift access with Okta as an identity provider](http://aws.amazon.com/blogs/big-data/federate-amazon-redshift-access-with-okta-as-an-identity-provider/)\.
  + PingFederate\. For more information, see the [PingFederate site](https://www.pingidentity.com/en/software/pingfederate.html)\.
  + JumpCloud\. For more information, see the [JumpCloud site](https://jumpcloud.com/)\.
+ Amazon Redshift data types\.

The Amazon Redshift Python connector implements Python Database API Specification 2\.0\. For more information, see [PEP 249—Python Database API Specification v2\.0](https://www.python.org/dev/peps/pep-0249/) on the Python website\.

**Topics**
+ [Installing the Amazon Redshift Python connector](python-driver-install.md)
+ [Configuration options for the Amazon Redshift Python connector](python-configuration-options.md)
+ [Importing the Python connector](python-start-import.md)
+ [Integrating the Python connector with NumPy](python-connect-integrate-numpy.md)
+ [Integrating the Python connector with pandas](python-connect-integrate-pandas.md)
+ [Using identity provider plugins](python-connect-identity-provider-plugins.md)
+ [Examples of using the Amazon Redshift Python connector](python-connect-examples.md)
+ [API reference for the Amazon Redshift Python connector](python-api-reference.md)