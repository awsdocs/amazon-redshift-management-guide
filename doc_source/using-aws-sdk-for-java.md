# Using the AWS SDK for Java with Amazon Redshift<a name="using-aws-sdk-for-java"></a>

The AWS SDK for Java provides a class named `AmazonRedshiftClientBuilder`, which you can use to interact with Amazon Redshift\. For information about downloading the AWS SDK for Java, go to [AWS SDK for Java](https://aws.amazon.com/sdk-for-java/)\. 

**Note**  
The AWS SDK for Java provides thread\-safe clients for accessing Amazon Redshift\. As a best practice, your applications should create one client and reuse the client between threads\.

You can use the `AmazonRedshiftClientBuilder` and `AwsClientBuilder` classes to configure an endpoint and create an `AmazonRedshift` client\. You can then use the client object to create an instance of a `Cluster` object\. The `Cluster` object includes methods that map to underlying Amazon Redshift Query API actions\. \(These actions are described in the Amazon Redshift [API reference](https://docs.aws.amazon.com/redshift/latest/APIReference/API_Operations.html)\)\. When you call a method, you must create a corresponding request object\. The request object includes information that you must pass with the actual request\. The `Cluster` object provides information returned from Amazon Redshift in response to the request\. 

The following example illustrates using the `AmazonRedshiftClientBuilder` class to configure an endpoint and then create a 2\-node **ds2\.xlarge** cluster\. 

```
String endpoint = "https://redshift.us-east-1.amazonaws.com/";
String region = "us-east-1";
AwsClientBuilder.EndpointConfiguration config = new AwsClientBuilder.EndpointConfiguration(endpoint, region);
AmazonRedshiftClientBuilder clientBuilder = AmazonRedshiftClientBuilder.standard();
clientBuilder.setEndpointConfiguration(config);
AmazonRedshift client = clientBuilder.build();

CreateClusterRequest request = new CreateClusterRequest()
  .withClusterIdentifier("exampleclusterusingjava")
  .withMasterUsername("masteruser")
  .withMasterUserPassword("12345678Aa")
  .withNodeType("ds2.xlarge")
  .withNumberOfNodes(2);

Cluster createResponse = client.createCluster(request);
System.out.println("Created cluster " + createResponse.getClusterIdentifier());
```

## Running Java examples for Amazon Redshift using Eclipse<a name="setting-up-and-testing-sdk-java"></a>

**General process of running Java code examples using Eclipse**

1. Create a new **AWS Java Project** in Eclipse\. 

   Follow the steps in [Setting up the AWS Toolkit for Eclipse](https://docs.aws.amazon.com/AWSToolkitEclipse/latest/GettingStartedGuide/tke_setup.html) in the *AWS Toolkit for Eclipse Getting Started Guide*\.

1. Copy the sample code from the section of this document that you are reading and paste it into your project as a new Java class file\.

1. Run the code\.

## Running Java examples for Amazon Redshift from the command line<a name="setting-up-and-testing-sdk-java-commandline"></a>

**General process of running Java code examples from the command line**

1. Set up and test your environment as follows:

   1. Create a directory to work in and in it create `src`, `bin`, and `sdk` subfolders\. 

   1. Download the AWS SDK for Java and unzip it to the `sdk` subfolder you created\. After you unzip the SDK, you should have four subdirectories in the `sdk` folder, including a `lib` and `third-party` folder\.

   1.  Supply your AWS credentials to the SDK for Java\. For more information, go to [Providing AWS credentials in the AWS SDK for Java](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) in the *AWS SDK for Java Developer Guide*\. 

   1. Ensure that you can run the Java program compiler \(`javac`\) and the Java application launcher \(`java`\) from your working directory\. You can test by running the following commands:

      ```
      javac -help
      java -help
      ```

1. Put the code that you want to run in a \.java file, and save the file in the `src` folder\. To illustrate the process, we use the code from [Managing cluster security groups using the AWS SDK for Java](managing-security-groups-java.md) so that the file in the `src` directory is ` CreateAndModifyClusterSecurityGroup.java`\.

1. Compile the code\. 

   ```
   javac -cp sdk/lib/aws-java-sdk-1.3.18.jar -d bin src\CreateAndModifyClusterSecurityGroup.java
   ```

   If you are using a different version of the AWS SDK for Java, adjust the classpath \(`-cp`\) for your version\.

1. Run the code\. In the following command, line breaks are added for readability\.

   ```
   java -cp "bin;
             sdk/lib/*;
             sdk/third-party/commons-logging-1.1.1/*;
             sdk/third-party/httpcomponents-client-4.1.1/*;
             sdk/third-party/jackson-core-1.8/*" 
             CreateAndModifyClusterSecurityGroup
   ```

   Change the class path separator as needed for your operating system\. For example, for Windows, the separator is ";" \(as shown\), and for Unix, it is ":"\. Other code examples may require more libraries than are shown in this example, or the version of the AWS SDK you are working with may have different third\-party folder names\. For these cases, adjust the classpath \(`-cp`\) as appropriate\.

   To run samples in this document, use a version of the AWS SDK that supports Amazon Redshift\. To get the latest version of the AWS SDK for Java, go to [AWS SDK for Java](https://aws.amazon.com/sdkforjava/)\.

## Setting the endpoint<a name="setting-sdk-java-endpoint"></a>

By default, the AWS SDK for Java uses the endpoint `https://redshift.us-east-1.amazonaws.com/`\. You can set the endpoint explicitly with the `client.setEndpoint` method as shown in the following Java code snippet\.

**Example**  

```
client = new AmazonRedshiftClient(credentials);
client.setEndpoint("https://redshift.us-east-1.amazonaws.com/");
```

For a list of supported AWS regions where you can provision a cluster, go to the [Regions and endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#redshift_region) section in the *Amazon Web Services Glossary*\. 