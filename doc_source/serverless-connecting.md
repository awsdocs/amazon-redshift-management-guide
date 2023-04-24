# Connecting to Amazon Redshift Serverless<a name="serverless-connecting"></a>

Once you've set up your Amazon Redshift Serverless instance, you can connect to it in a variety of methods, outlined below\. If you have multiple teams or projects and want to manage costs separately, you can use separate AWS accounts\.

For a list of AWS Regions where the Amazon Redshift Serverless is available, see the endpoints listed for [Redshift Serverless API](https://docs.aws.amazon.com/general/latest/gr/redshift-service.html) in the *Amazon Web Services General Reference*\.

Amazon Redshift Serverless connects to the serverless environment in your AWS account in the current AWS Region\. Amazon Redshift Serverless runs in a VPC within the port ranges port ranges 5431\-5455 and 8191\-8215\. The default is 5439\. Currently, you can only change ports with the API operation `UpdateWorkgroup` and the AWS CLI operation `update-workgroup`\.

## Connecting to Amazon Redshift Serverless<a name="serverless-connecting-endpoint"></a>

You can connect to a database \(named `dev`\) in Amazon Redshift Serverless with the following syntax\.

```
workgroup-name.account-number.aws-region.redshift-serverless.amazonaws.com:port/dev
```

For example, the following connection string specifies Region us\-east\-1\.

```
default.123456789012.us-east-1.redshift-serverless.amazonaws.com:5439/dev
```

## Connecting to Amazon Redshift Serverless through JDBC drivers<a name="serverless-connecting-driver"></a>

You can use one of the following methods to connect to Amazon Redshift Serverless with your preferred SQL client using the Amazon Redshift\-provided JDBC driver version 2 driver\.

To connect with sign\-in credentials for database authentication using JDBC driver version 2\.1\.x or later, use the following syntax\. The port number is optional; if not included, Amazon Redshift Serverless defaults to port number 5439\. You can change to another port from the port range of 5431\-5455 or 8191\-8215\. To change the default port for a serverless endpoint, use the AWS CLI and Amazon Redshift API\.

```
jdbc:redshift://workgroup-name.account-number.aws-region.redshift-serverless.amazonaws.com:5439/dev
```

For example, the following connection string specifies the workgroup default, the account ID 123456789012, and the Region us\-east\-2\.

```
jdbc:redshift://default.123456789012.us-east-2.redshift-serverless.amazonaws.com:5439/dev
```

To connect with IAM using JDBC driver version 2\.1\.x or later, use the following syntax\. The port number is optional; if not included, Amazon Redshift Serverless defaults to port number 5439\. You can change to another port from the port range of 5431\-5455 or 8191\-8215\. To change the default port for a serverless endpoint, use the AWS CLI and Amazon Redshift API\.

```
jdbc:redshift:iam://workgroup-name.account-number.aws-region.redshift-serverless.amazonaws.com:5439/dev
```

For example, the following connection string specifies the workgroup default, the account ID 123456789012, and the Region us\-east\-2\.

```
jdbc:redshift:iam://default.123456789012.us-east-2.redshift-serverless.amazonaws.com:5439/dev
```

For ODBC, use the following syntax\.

```
Driver={Amazon Redshift (x64)}; Server=workgroup-name.account-number.aws-region.redshift-serverless.amazonaws.com; Database=dev
```

If you are using a JDBC driver version prior to 2\.1\.0\.9 and connecting with IAM, you will need to use the following syntax\.

```
jdbc:redshift:iam://redshift-serverless-<name>:aws-region/database-name
```

For example, the following connection string specifies the workgroup default and the AWS Region us\-east\-1\.

```
jdbc:redshift:iam://redshift-serverless-default:us-east-1/dev
```

For more information about drivers, see [Configuring connections in Amazon Redshift](configuring-connections.md)\.  

### Finding your JDBC and ODBC connection string<a name="serverless-connecting-jdbc-odbc-string"></a>

To connect to your workgroup with your SQL client tool, you must have the JDBC or ODBC connection string\. You can find the connection string in the Amazon Redshift Serverless console, on a workgroup's details page\.

**To find the connection string for a workgroup**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Redshift Serverless**\.

1. On the navigation menu, choose **Workgroup configuration**, then choose the workgroup name from the list to open its details\.

1. The **JDBC URL** and **ODBC URL** connection strings are available, along with additional details, in the **General information** section\. Each string is based on the AWS Region where the workgroup runs\. Choose the icon next to the appropriate connection string to copy the connection string\.

## Connecting to Amazon Redshift Serverless with the Data API<a name="serverless-data-api"></a>

You can also use the Amazon Redshift Data API to connect to Amazon Redshift Serverless\. Use the `workgroup-name` parameter instead of the `cluster-identifier` parameter in your AWS CLI calls\. 

For more information about the Data API, see [Using the Amazon Redshift Data API](data-api.md)\.   For example code calling the Data API in Python and other examples, see [Getting Started with Redshift Data API](https://github.com/aws-samples/getting-started-with-amazon-redshift-data-api#readme) and look in the `quick-start` and `use-cases` folders in *GitHub*\. 

## Connecting with SSL to Amazon Redshift Serverless<a name="serverless-secure-ssl"></a>

### Configuring a secure connection to Amazon Redshift Serverless<a name="serverless_secure-ssl"></a>

Amazon Redshift supports Secure Sockets Layer \(SSL\) connections to encrypt queries and data\. To set up a secure connection, you can use the same configuration you use to set up a connection to a provisioned Redshift cluster\. Follow the steps in [Configuring security options for connections](https://docs.aws.amazon.com/redshift/latest/mgmt/connecting-ssl-support.html), which describes how to download and install the available SSL certificate bundle\. The bundle works for a connection to both a serverless Redshift instance and a provisioned cluster\. When connecting to an Amazon Redshift Serverless instance, you don't have to set any parameters to accept SSL connections\.

## Connecting to Amazon Redshift Serverless from an Amazon Redshift managed VPC endpoint<a name="serverless-secure-vpc"></a>

### Connecting to Amazon Redshift Serverless from other VPC endpoints<a name="serverless-vpc-connect"></a>

You can connect to Amazon Redshift Serverless from other VPC endpoints, including on\-premises and public VPC endpoints\.

#### Connecting to Amazon Redshift Serverless from a Redshift managed VPC endpoint<a name="database-connect-from-managed-vpc-endpoint"></a>

Amazon Redshift Serverless is provisioned in a VPC\. By creating a Redshift managed VPC endpoint, you privately access your Amazon Redshift Serverless from client applications in another VPC\. When you do this, the traffic doesn't pass through the internet and you don't use public IP addresses\. This provides for improved communication privacy and security\.

**Create a Redshift managed VPC endpoint using the console**

1. On the console, choose **Workgroup configuration**, and select a workgroup from the list\.

1. In **Redshift managed VPC endpoints**, choose **Create endpoint**\.

1. Enter the endpoint name\. Create a name that is meaningful for your organization\.

1. Choose the AWS account ID\. This is your 12\-digit account ID, or your account alias\.

1. Choose the AWS VPC where the endpoint is located\. Then choose a subnet ID\. In the most common use case, this is a subnet where you have a client that you want to connect to your Amazon Redshift Serverless instance\.

1. You can choose VPC security groups to add\. Each acts as a virtual firewall to control inbound and outbound traffic to specific virtual\-desktop instances, for instance\.

1. Choose **Create endpoint**\.

**Edit a Redshift managed VPC endpoint using the console**

1. On the console, choose **Workgroup configuration**, and select a workgroup from the list\.

1. In **Redshift managed VPC endpoints**, choose **Edit**\.

1. Add or remove VPC security groups\. This is the only setting you can change after creating a Redshift managed VPC endpoint\.

1. Choose **Save changes**\.

**Delete a Redshift managed VPC endpoint on the console**

1. On the console, choose **Workgroup configuration**, and select a workgroup from the list\.

1. In **Redshift managed VPC endpoints**, select the VPC endpoint to delete\.

1. Choose **Delete**\.

## Creating a publicly accessible Amazon Redshift Serverless instance and connecting to it<a name="serverless-publicly-accessible"></a>

### Connect to a publicly accessible Amazon Redshift Serverless instance<a name="database-connect-public-endpoint"></a>

You can configure your Amazon Redshift Serverless instance so you can query it from a SQL client in the public internet\. You don't always have to connect from the local VPC or from a subnet behind your firewall\. 

These steps walk you through configuring Amazon Redshift Serverless to accept connections from the internet\.

1. On the Redshift console, go to the Amazon Redshift Serverless main menu\. Choose **Create workgroup** and then follow the steps to give it a name\. Pick the associated VPC and subnet\. Choose **Next**\.

1. Complete the steps to create a namespace\. The process includes specifying a database and assigning an IAM role with permissions to perform database tasks\.

   If you already created a namespace, that works too\.

1. On the Amazon VPC service console, verify that your VPC has an internet gateway attached, with a custom route table\. For more information, see [Connect to the internet using an internet gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html)\.

1. After you complete the previous steps, or if you already have a configured namespace and workgroup, choose **Workgroup configuration**\. Choose the workgroup from the list\. Then, in the **Network and security** panel, choose **edit**\.

1. Select **Turn on Public Accessible**\. When you do this, the Amazon Redshift Serverless instance is made public by means of assigning to it a static IPv4 Elastic IP address\. This IP address is allocated to your AWS account\.

After you configure Amazon Redshift Serverless to accept connections from public clients, follow these steps to connect\.

1. On the Amazon Redshift console, select the Serverless dashboard, choose **Workgroup configuration**, and select the workgroup\. Under **Data access**, choose **Edit** to view the **Network and security** settings\. Note the VPC security group for the workgroup\. Go to Amazon VPC and choose **Security groups** from the menu\. Choose your security group ID in the list\. The security group has configuration settings that include **Inbound rules**\. Choose **Edit inbound rules** and create a rule that specifies the source IP address to allow, and the port\. 

1. On the Amazon VPC service console, verify that your VPC has the internet gateway attached\. Confirm that the internet gateway's target is set with source 0\.0\.0\.0/0 or a public IP CIDR\. The route table must be associated with the VPC subnet where your cluster resides\.

1. On your client, set an inbound firewall rule to accept traffic on the port you chose when you configured the workgroup and namespace\.

1. Connect with your client tool, such as [Amazon Redshift RSQL](https://docs.aws.amazon.com/redshift/latest/mgmt/rsql-query-tool.html)\. Using your Amazon Redshift Serverless domain as the host, enter the following:

   ```
   rsql -h workgroup-name.account-id.region.amazonaws.com -U admin -d dev -p 5439
   ```

When you turn on the publicly accessible setting, Amazon Redshift Serverless creates an Elastic IP address\. It's a static IP address that is associated with your AWS account\. Clients outside the VPC can use it to connect\. It gives you the ability to change your underlying network configuration without affecting client connections\. 

## Defining database roles to grant to federated users in Amazon Redshift Serverless<a name="serverless-roles-sso"></a>

You can define roles in your organization that determine which database roles to grant in Amazon Redshift Serverless\. For more information, see [Defining database roles to grant to federated users in Amazon Redshift Serverless](redshift-iam-access-federated-db-roles.md)\.

## Additional resources<a name="serverless-connecting-additional"></a>

For more information about secure connections to Amazon Redshift Serverless, including granting permissions, authorizing access to additional services, and creating IAM roles, see [Security and connections in Amazon Redshift Serverless](serverless-security.md)\.