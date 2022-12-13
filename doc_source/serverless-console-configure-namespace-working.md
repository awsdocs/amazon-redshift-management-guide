# Working with namespaces<a name="serverless-console-configure-namespace-working"></a>

In Amazon Redshift Serverless, a namespace defines a logical container for database objects\. It can hold tables, workgroups, and other database resources\. If you haven't created a workgroup and a namespace, and you are looking for instructions in how to get started with Amazon Redshift Serverless, see [Setting up Amazon Redshift Serverless for the first time](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-console-first-time-setup.html)\.

## Searching for a namespace<a name="serverless-console-configure-namespace"></a>

From the Amazon Redshift menu, you can choose from the **Namespaces** list in order to view or edit the properties for a namespace\. Information on the console includes the namespace name, the admin name, and other properties\.

A namespace's settings and properties are on several tabs\. These include the following:
+ **Workgroup** \- Shows workgroups associated with the namespace\.
+ **Data back up** \- You can configure and create snapshots, and configure recovery points\.
+ **Security and encryption** \- You can manage IAM role permissions and view or edit your security and encryption settings\. These include your encryption key status and your audit logging settings\.
+ **Datashares** \- Shows datashares\.

## Namespace properties<a name="serverless-console-namespace-config"></a>

In Amazon Redshift Serverless, a namespace defines a container for database objects\. You can choose **Namespace configuration** from the navigation list, choose a namespace from the list, and edit its settings\.

General information for the namespace includes the following:
+ **Namespace** \- The name\.
+ **Namespace ID** \- The unique identifier\.
+ **ARN** \- A unique identifier used to specify the resource across AWS\. It contains properties like the region and the service\.
+ **Status** \- The status, such as **Available**\.
+ **Date created** \- The date the namespace was created\.
+ **Storage used** \- The storage space used by the namespace and all of its objects\.
+ **Admin user name** \- The admin account\. This is typically the account used to create the namespace\.
+ **Database name** \- The name of the database contained by the namespace\.
+ **Total table count** \- The count of tables in all schemas\.

Additional settings and properties for the namespace are on several tabs\. These include the following:
+ **Workgroup** \- Shows the workgroup associated with the namespace\.
+ **Data back up** \- On this panel, you can configure and create snapshots, and configure recovery points\.
+ **Security and encryption** \- You can manage IAM role permissions and view or edit your security and encryption settings\. These include your encryption key status, and the setting to turn on audit logging\. For more information about audit logging for Amazon Redshift Serverless, see [Audit logging for Amazon Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-audit-logging.html)\.
+ **Datashares** \- Shows datashares\. With data sharing, you can provide access to data without the need to copy it or move it\. For more information about data sharing, see [Data sharing in Amazon Redshift Serverless](https://docs.aws.amazon.com/redshift/latest/mgmt/serverless-datasharing.html)\.