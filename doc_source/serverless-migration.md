# Migrating a provisioned cluster to Amazon Redshift Serverless<a name="serverless-migration"></a>

To migrate from a provisioned cluster to Amazon Redshift Serverless, see the following steps\.

## Creating a snapshot of your provisioned cluster<a name="serverless-migration-snapshots"></a>

 To transfer data from your provisioned cluster to Amazon Redshift Serverless, create a snapshot of your provisioned cluster, and then restore the snapshot in Amazon Redshift Serverless\. 

To create a snapshot of your provisioned cluster

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, **Snapshots**, and then choose **Create snapshot**\.

1. Enter the properties of the snapshot definition, then choose **Create snapshot**\. It might take some time for the snapshot to be available\. 

To restore a provisioned cluster snapshot to an Amazon Redshift Serverless instance

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. Start on the Amazon Redshift provisioned cluster console and navigate to the **Clusters**, **Snapshots** page\.

1. Choose a snapshot to use\.

1. Choose **Restore snapshot**, **Restore to serverless namespace**\.

1. Choose a namespace to restore your snapshot to\.

1. Confirm you want to restore from your snapshot\. This action replaces all the databases in your serverless endpoint with the data from your provisioned cluster\. Choose **Restore**\.

For more information about provisioned cluster snapshots, see [Amazon Redshift snapshots](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-snapshots.html)\.

## Using a driver endpoint<a name="serverless-migration-drivers"></a>

 To connect to Amazon Redshift Serverless with your preferred SQL client, you can use the Amazon Redshift provided JDBC driver version 2 driver\. We recommend connecting using JDBC driver version 2\.1\.x or later\. The port number is optional\. If you don’t include it, Amazon Redshift Serverless defaults to port number 5439\. You can change to another port from the port range of 5431\-5455 or 8191\-8215\. To change the default port for a serverless endpoint, use the AWS CLI and Amazon Redshift API\. 

 To find the exact endpoint to use for the JDBC or ODBC driver, see **Workgroup configuration** in your Amazon Redshift Serverless instance\. You can also use the Amazon Redshift Serverless API operation `GetWorkgroup` operation or the AWS CLI operation `get-workgroups` to return information about your workgroup, and then connect\. 

### Connecting using password\-based authentication<a name="serverless-migration-drivers-password-auth"></a>

To connect using password\-based authentication, use the following syntax\.

```
jdbc:redshift://<workgroup-name>.<account-number>.<aws-region>.redshift-serverless.amazonaws.com:5439/?username=<username>&password=<password>
```

### Connecting using IAM<a name="serverless-migration-drivers-iam"></a>

 If you prefer logging in with IAM, use the following driver endpoint\. This driver endpoint lets you connect to a specific database and uses the Amazon Redshift Serverless [https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/API_GetCredentials.html](https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/API_GetCredentials.html) API operation\. 

```
jdbc:redshift:iam://<workgroup-name>.<account-number>.<aws-region>.redshift-serverless.amazonaws.com:5439/<database-name>
```

This driver endpoint doesn’t support customizing `dbUser`, `dbGroup` and `auto-create`\. By default, the driver automatically creates database users at login and assigns them to the groups according to the IAM user groups you defined in IAM\. Note: IAM user group names you specify in IAM must contain only lowercase letters, numbers, underscore \('\_'\), plus sign \('\+'\), period \(dot\), at symbol \(@\), or hyphen \('\-'\)\. Otherwise, the driver might not connect to `dbGroup`\.

Ensure that your AWS identity has the correct IAM policy for the `RedshiftServerlessGetCredentials` action\. The following is an example IAM policy that grants the correct permissions to an AWS identity to connect to Amazon Redshift Serverless\. For more information about IAM permissions, see [ Add IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-policies-console)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Action": "redshift-serverless:GetCredentials",
            "Resource": "*"
        }
    ]
}
```

### Connecting using IAM with dbUser and dbGroup<a name="serverless-migration-drivers-iam-dbuser-group"></a>

 If you want to use custom dbUser and dbGroup, use the following driver endpoint\. Like the other Amazon Redshift Serverless driver endpoint, this syntax automatically creates database users at login\. This driver endpoint uses the Amazon Redshift [https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html](https://docs.aws.amazon.com/redshift/latest/APIReference/API_GetClusterCredentials.html) API operation\. dbUser must begin with a letter, must contain only alphanumeric characters, underscore \('\_'\), plus sign \('\+'\), dot \('\.'\), at \('@'\), or hyphen \('\-'\), and must be less than 128 characters\. dbGroup must contain only lowercase letters, numbers, underscore \('\_'\), plus sign \('\+'\), period \(dot\), at symbol \(@\), or hyphen\. 

```
jdbc:redshift:iam://redshift-serverless-<name>:aws-region/<database-name>
```

### Connecting using ODBC<a name="serverless-migration-drivers-odbc"></a>

To connect using ODBC, use the following syntax\.

```
Driver={Amazon Redshift (x64)}; Server=<workgroup-name>.<account-number>.<aws-region>.redshift-serverless.amazonaws.com; Database=dev
```

## Using the Amazon Redshift Serverless SDK<a name="serverless-migration-sdk"></a>

If you wrote any management scripts using the Amazon Redshift SDK, you must use the new Amazon Redshift Serverless SDK to manage your Amazon Redshift Serverless instance and resources\. For more information about available API operations, see the [Amazon Redshift Serverless API Reference guide](https://docs.aws.amazon.com/redshift-serverless/latest/APIReference/Welcome.html)\.