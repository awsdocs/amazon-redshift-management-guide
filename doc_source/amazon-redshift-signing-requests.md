# Signing an HTTP request<a name="amazon-redshift-signing-requests"></a>

Amazon Redshift requires that every request you send to the management API be authenticated with a signature\. This topic explains how to sign your requests\. 

If you are using one of the AWS Software Development Kits \(SDKs\) or the AWS Command Line Interface, request signing is handled automatically, and you can skip this section\. For more information about using AWS SDKs, see [Using the Amazon Redshift management interfaces](using-aws-sdk.md)\. For more information about using the Amazon Redshift Command Line Interface, go to [Amazon Redshift command line reference](https://docs.aws.amazon.com/cli/latest/reference/redshift/index.html)\.

To sign a request, you calculate a digital signature by using a cryptographic hash function\. A cryptographic hash is a function that returns a unique hash value that is based on the input\. The input to the hash function includes the text of your request and your secret access key\. The hash function returns a hash value that you include in the request as your signature\. The signature is part of the `Authorization` header of your request\.

**Note**  
For API access, you need an access key ID and secret access key\. Use IAM user access keys instead of AWS account root user access keys\. For more information about creating access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\. 

After Amazon Redshift receives your request, it recalculates the signature by using the same hash function and input that you used to sign the request\. If the resulting signature matches the signature in the request, Amazon Redshift processes the request; otherwise, the request is rejected\. 

Amazon Redshift supports authentication using [AWS signature version 4](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html)\. The process for calculating a signature is composed of three tasks\. These tasks are illustrated in the example that follows\.
+   [Task 1: Create a canonical request](https://docs.aws.amazon.com/general/latest/gr/sigv4-create-canonical-request.html)

  Rearrange your HTTP request into a canonical form\. Using a canonical form is necessary because Amazon Redshift uses the same canonical form to calculate the signature it compares with the one you sent\. 
+   [Task 2: Create a string to sign](https://docs.aws.amazon.com/general/latest/gr/sigv4-create-string-to-sign.html)

  Create a string that you will use as one of the input values to your cryptographic hash function\. The string, called the *string to sign*, is a concatenation of the name of the hash algorithm, the request date, a *credential scope* string, and the canonicalized request from the previous task\. The *credential scope* string itself is a concatenation of date, region, and service information\.
+   [Task 3: Create a signature](https://docs.aws.amazon.com/general/latest/gr/sigv4-calculate-signature.html)

  Create a signature for your request by using a cryptographic hash function that accepts two input strings, your string to sign and a *derived key*\. The derived key is calculated by starting with your secret access key and using the credential scope string to create a series of hash\-based message authentication codes \(HMAC\-SHA256\)\. 

## Example signature calculation<a name="example-signature-calculation"></a>

The following example walks you through the details of creating a signature for [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster.html) request\. You can use this example as a reference to check your own signature calculation method\. Other reference calculations are included in the [Signature Version 4 test suite](https://docs.aws.amazon.com/general/latest/gr/signature-v4-test-suite.html) of the Amazon Web Services Glossary\. 

You can use a GET or POST request to send requests to Amazon Redshift\. The difference between the two is that for the GET request your parameters are sent as query string parameters\. For the POST request they are included in the body of the request\. The example below shows a POST request\.

The example assumes the following:
+ The time stamp of the request is `Fri, 07 Dec 2012 00:00:00 GMT`\.
+ The endpoint is US East \(Northern Virginia\) Region, `us-east-1`\.

The general request syntax is: 

```
https://redshift.us-east-1.amazonaws.com/
   ?Action=CreateCluster
   &ClusterIdentifier=examplecluster
   &MasterUsername=masteruser
   &MasterUserPassword=12345678Aa
   &NumberOfNode=2
   &NodeType=ds2.xlarge
   &Version=2012-12-01
   &x-amz-algorithm=AWS4-HMAC-SHA256
   &x-amz-credential=AKIAIOSFODNN7EXAMPLE/20121207/us-east-1/redshift/aws4_request
   &x-amz-date=20121207T000000Z
   &x-amz-signedheaders=content-type;host;x-amz-date
```

The canonical form of the request calculated for [Task 1: Create a Canonical Request](#SignatureCalculationTask1) is:

```
POST
/

content-type:application/x-www-form-urlencoded; charset=utf-8
host:redshift.us-east-1.amazonaws.com
x-amz-date:20121207T000000Z

content-type;host;x-amz-date
55141b5d2aff6042ccd9d2af808fdf95ac78255e25b823d2dbd720226de1625d
```

The last line of the canonical request is the hash of the request body\. The third line in the canonical request is empty because there are no query parameters for this API\. 

The string to sign for [Task 2: Create a String to Sign](#SignatureCalculationTask2) is:

```
AWS4-HMAC-SHA256
20121207T000000Z
20121207/us-east-1/redshift/aws4_request
06b6bef4f4f060a5558b60c627cc6c5b5b5a959b9902b5ac2187be80cbac0714
```

The first line of the *string to sign* is the algorithm, the second line is the time stamp, the third line is the *credential scope*, and the last line is a hash of the canonical request from [Task 1: Create a Canonical Request](#SignatureCalculationTask1)\. The service name to use in the credential scope is `redshift`\.

For [Task 3: Create a Signature](#SignatureCalculationTask3), the derived key can be represented as:

```
derived key = HMAC(HMAC(HMAC(HMAC("AWS4" + YourSecretAccessKey,"20121207"),"us-east-1"),"redshift"),"aws4_request")
```

The derived key is calculated as series of hash functions\. Starting from the inner HMAC statement in the formula above, you concatenate the phrase "AWS4" with your secret access key and use this as the key to hash the data "us\-east\-1"\. The result of this hash becomes the key for the next hash function\. 

After you calculate the derived key, you use it in a hash function that accepts two input strings, your string to sign and the derived key\. For example, if you use the secret access key `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` and the string to sign given earlier, then the calculated signature is as follows:

```
9a6b557aa9f38dea83d9215d8f0eae54100877f3e0735d38498d7ae489117920
```

The final step is to construct the `Authorization` header\. For the demonstration access key `AKIAIOSFODNN7EXAMPLE`, the header \(with line breaks added for readability\) is:

```
Authorization: AWS4-HMAC-SHA256 Credential=AKIAIOSFODNN7EXAMPLE/20121207/us-east-1/redshift/aws4_request, 
SignedHeaders=content-type;host;x-amz-date, 
Signature=9a6b557aa9f38dea83d9215d8f0eae54100877f3e0735d38498d7ae489117920
```