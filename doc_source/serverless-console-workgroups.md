# Viewing properties for a workgroup<a name="serverless-console-workgroups"></a>

In Amazon Redshift Serverless, a workgroup is a collection of resources available for use\. When you choose Amazon Redshift Serverless, in the AWS console, you can choose **Workgroup configuration** from the navigation menu to view a list\. You can use the **Search** box to find workgroups that meet your search criteria\. Each workgroup entry has a few properties displayed:
+ **Workgroup** \- The name of the workgroup\. You can select it to view and edit the workgroup's properties\.
+ **Status** \- Shows whether the workgroup is available\.
+ **Namespace** \- The namespace associated with the workgroup\. Each workgroup is associated with one namespace\.
+ **Creation date** \- The date the workgroup was created\.

## Workgroup properties<a name="serverless-workgroup-describe"></a>

You can list workgroups by choosing **Workgroup configuration** in the left menu\. Then you can choose a workgroup from the list\. Several panels show properties for the workgroup\. You can also perform actions\. **General information** displays the following:
+ **Workgroup** \- The name of the workgroup\.
+ **Namespace** \- The namespace associated with the workgroup\. You can choose it to view its properties\. A workgroup is associated with a single namespace\.
+ **Date created** \- When the workgroup was created\.
+ **Status** \- Indicates if the workgroup resources are available\. If it's available, you can connect with a client to the Amazon Redshift Serverless instance, in order to query data or create database resources, or you can connect with query editor v2\.
+ **Endpoint** \- The URL\.
+ **JDBC URL** \- The URL to establish JDBC client connections\. You can use this URL to connect with a JDBC driver for Amazon Redshift\. For more information, see [Configuring a connection for JDBC driver version 2\.1 for Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/mgmt/jdbc20-install.html)\.
+ **ODBC URL** \- The URL to establish ODBC client connections\. It contains properties, like the database and user ID, and their values\.

The **Data access** tab contains several panels:
+ **Network and security** \- You can see network properties, such as the **Virtual private cloud \(VPC\)** identifier, **VPC security group** list, **Enhanced VPC routing**, and the **Publicly accessible** setting\. If you choose **Edit**, you can change these settings\. Additionally, you can select **Turn on enhanced VPC routing**, which routes network traffic between your serverless database and data repositories through a VPC, for enhanced privacy and security\. You can also select **Turn on Public Accessible**, which makes the database publicly accessible from outside the VPC, allowing instances and devices to connect\.
+ **Redshift managed VPC endpoints** \- You can create managed VPC endpoints to access Amazon Redshift Serverless from another VPC\.

The **Limits** tab has settings for controlling capacity and use limits for Amazon Redshift Serverless\. It contains the following panels:
+ **Base capacity in Redshift processing units \(RPUs\)** \- You can set the base capacity of the compute resources used to process your workload\. For more information, see [Understanding Amazon Redshift Serverless capacity](serverless-capacity.md#serverless-rpu-capacity)\.
+ **Usage limits** \- You can set maximum RPU hours at various frequencies\. For instance, you can set a daily RPU maximum\. These limits help control your cost and make it more predictable\.
+ **Query limits** \- You can set limits on queries, like the timeout setting\. These limits help you optimize cost and performance\.