# Using the Amazon Redshift Management Interfaces<a name="using-aws-sdk"></a>


+ [Using the AWS SDK for Java with Amazon Redshift](using-aws-sdk-for-java.md)
+ [Signing an HTTP Request](amazon-redshift-signing-requests.md)
+ [Setting Up the Amazon Redshift CLI](setting-up-rs-cli.md)

Amazon Redshift supports several management interfaces that you can use to use to create, manage, and delete Amazon Redshift clusters; the AWS SDKs, the AWS command line interface, and the Amazon Redshift management API\.

**Amazon Redshift QUERY API — **is a Amazon Redshift management API you can call by submitting a Query request\. Query requests are HTTP or HTTPS requests that use the HTTP verbs `GET` or `POST` with a query parameter named `Action`\. Calling the Query API is the most direct way to access the Amazon Redshift service, but requires that your application handle low\-level details such as error handling and generating a hash to sign the request\.

+ For information about building and signing a Query API request, see [Signing an HTTP Request](amazon-redshift-signing-requests.md)\.

+ For information about the Query API actions and data types for Amazon Redshift, go to the [Amazon Redshift API Reference](http://docs.aws.amazon.com/redshift/latest/APIReference/Welcome.html)\.

**AWS SDKs — **Amazon Web Services provides Software Development Kits \(SDKs\) that you can use to perform Amazon Redshift cluster\-related operations\. Several of the SDK libraries wrap the underlying Amazon Redshift Query API\. They integrate the API functionality into the specific programming language and handle many of the low\-level details, such as calculating signatures, handling request retries, and error handling\. Calling the wrapper functions in the SDK libraries can greatly simplify the process of writing an application to manage an Amazon Redshift cluster\.

+ Amazon Redshift is supported by the AWS SDKs for Java, \.NET, PHP, Python, Ruby, and Node\.js\. The wrapper functions for Amazon Redshift are documented in the reference manual for each SDK\. For a list of the AWS SDKs and links to their documentation, go to [Tools for Amazon Web Services](https://aws.amazon.com/tools/)\.

+ This guide provides examples of working with Amazon Redshift using the Java SDK\. For more general AWS SDK code examples, go to [Sample Code & Libraries](https://aws.amazon.com/code/)\.

**AWS Command Line Interface \(CLI\) — **provides a set of command line tools that can be used to manage AWS services from Windows, Mac, and Linux computers\. The AWS CLI includes commands based on the Amazon Redshift Query API actions\.

+ For information about installing and setting up the Amazon Redshift CLI, see [Setting Up the Amazon Redshift CLI](setting-up-rs-cli.md)\.

+ For reference material on the Amazon Redshift CLI commands, go to [Amazon Redshift](http://docs.aws.amazon.com/cli/latest/reference/redshift/index.html) in the AWS CLI Reference\.