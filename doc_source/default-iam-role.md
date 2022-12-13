# Creating an IAM role as default for Amazon Redshift<a name="default-iam-role"></a>

  When you create IAM roles through the Redshift console, Amazon Redshift programmatically creates the roles in your AWS account and automatically attaches existing AWS managed policies to them\. This approach means that you can stay within the Redshift console and don't have to switch to the IAM console for role creation\. For more granular control of permissions for an existing IAM role that was created in the Amazon Redshift console, you can attach a customized managed policy to the IAM role\. 

## Overview of IAM roles created in the console<a name="default-iam-role-overview"></a>

When you use the Amazon Redshift console to create IAM roles, Amazon Redshift tracks all IAM roles created through the console\. Amazon Redshift preselects the most recent default IAM role for creating all new clusters and restoring clusters from snapshots\.

You can create an IAM role through the console that has a policy with permissions to run SQL commands\. These commands include COPY, UNLOAD, CREATE EXTERNAL FUNCTION, CREATE EXTERNAL TABLE, CREATE EXTERNAL SCHEMA, CREATE MODEL, or CREATE LIBRARY\. Optionally, you can get more granular control of user access to your AWS resources by creating and attaching custom policies to the IAM role\.

When you created an IAM role and set it as the default for the cluster using console, you don't have to provide the IAM role's Amazon Resource Name \(ARN\) to perform authentication and authorization\.

## Using the IAM roles created in the console<a name="default-iam-role-usage"></a>

The IAM role that you create through the console for your cluster has the `AmazonRedshiftAllCommandsFullAccess` managed policy automatically attached\. This IAM role allows Amazon Redshift to copy, unload, query, and analyze data for AWS resources in your IAM account\. The managed policy provides access to [COPY](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-authorization.html), [UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html), [CREATE EXTERNAL FUNCTION](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_FUNCTION.html), [CREATE EXTERNAL SCHEMA](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_SCHEMA.html), [CREATE MODEL](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_MODEL.html), and [CREATE LIBRARY](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_LIBRARY.html) operations\. The policy also grants permissions to run SELECT statements for related AWS services, such as Amazon S3, Amazon CloudWatch Logs, Amazon SageMaker, and AWS Glue\.

The CREATE EXTERNAL FUNCTION, CREATE EXTERNAL SCHEMA, CREATE MODEL, and CREATE LIBRARY commands have a `default` keyword\. For this keyword for these commands, Amazon Redshift uses the IAM role that is set as the default and associated with the cluster when the command runs\. You can run the [DEFAULT\_IAM\_ROLE](https://docs.aws.amazon.com/redshift/latest/dg/r_DEFAULT_IAM_ROLE.html) command to check the current default IAM role that is attached to the cluster\. 

To control access privileges of the IAM role created and set as default for your Redshift cluster, use the ASSUMEROLE privilege\. This access control applies to database users and groups when they run commands such as the ones listed preceding\. After you grant the ASSUMEROLE privilege to a user or group for the IAM role, the user or group can assume that role when running these commands\. By using the ASSUMEROLE privilege, you can grant access to the appropriate commands as required\.

Using the Amazon Redshift console, you can do the following:
+ [Creating an IAM role as the default](#create-iam)
+ [Removing IAM roles from your cluster](#remove-iam)
+ [Associating IAM roles with your cluster](#associate-iam)
+ [Setting an IAM role as the default](#set-default-iam)
+ [Making an IAM role no longer default for your cluster](#clear-default-iam)

## Permissions of the AmazonRedshiftAllCommandsFullAccess managed policy<a name="default-iam-role-permissions"></a>

The following example shows the permissions in the `AmazonRedshiftAllCommandsFullAccess` managed policy that allow certain actions for the IAM role that is set as default for your cluster\. The IAM role with permission policies attached authorizes what a user or group can and can't do\. Given these permissions, you can run the COPY command from Amazon S3, run UNLOAD, and use the CREATE MODEL command\.  

```
{
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:GetBucketAcl",
                "s3:GetBucketCors",
                "s3:GetEncryptionConfiguration",
                "s3:GetBucketLocation",
                "s3:ListBucket",
                "s3:ListAllMyBuckets",
                "s3:ListMultipartUploadParts",
                "s3:ListBucketMultipartUploads",
                "s3:PutObject",
                "s3:PutBucketAcl",
                "s3:PutBucketCors",
                "s3:DeleteObject",
                "s3:AbortMultipartUpload",
                "s3:CreateBucket"
            ],
            "Resource": [
                "arn:aws:s3:::redshift-downloads",
                "arn:aws:s3:::redshift-downloads/*",
                "arn:aws:s3:::*redshift*",
                "arn:aws:s3:::*redshift*/*"
            ]
}
```

The following example shows the permissions in the `AmazonRedshiftAllCommandsFullAccess` managed policy that allow certain actions for the IAM role that is set as default for the cluster\. The IAM role with permission policies attached authorizes what a user or group can and can't do\. Given the following permissions, you can run the CREATE EXTERNAL FUNCTION command\.

```
{
    "Action": [
        "lambda:InvokeFunction"
    ],
    "Resource": "arn:aws:lambda:*:*:function:*redshift*"
}
```

The following example shows the permissions in the `AmazonRedshiftAllCommandsFullAccess` managed policy that allow certain actions for the IAM role that is set as default for the cluster\. The IAM role with permission policies attached authorizes what a user or group can and can't do\. Given the following permissions, you can run the CREATE EXTERNAL SCHEMA and CREATE EXTERNAL TABLE commands needed for Amazon Redshift Spectrum\. 

```
{
            "Effect": "Allow",
            "Action": [
                "glue:CreateDatabase",
                "glue:DeleteDatabase",
                "glue:GetDatabase",
                "glue:GetDatabases",
                "glue:UpdateDatabase",
                "glue:CreateTable",
                "glue:DeleteTable",
                "glue:BatchDeleteTable",
                "glue:UpdateTable",
                "glue:GetTable",
                "glue:GetTables",
                "glue:BatchCreatePartition",
                "glue:CreatePartition",
                "glue:DeletePartition",
                "glue:BatchDeletePartition",
                "glue:UpdatePartition",
                "glue:GetPartition",
                "glue:GetPartitions",
                "glue:BatchGetPartition"
            ],
            "Resource": [
                "arn:aws:glue:*:*:table/*redshift*/*",
                "arn:aws:glue:*:*:catalog",
                "arn:aws:glue:*:*:database/*redshift*"
            ]
}
```

The following example shows the permissions in the `AmazonRedshiftAllCommandsFullAccess` managed policy that allow certain actions for the IAM role set as default for the cluster\. The IAM role with permission policies attached authorizes what a user or group can and can't do\. Given the following permissions, you can run the CREATE EXTERNAL SCHEMA command using federated queries\. 

```
{
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetResourcePolicy",
                "secretsmanager:GetSecretValue",
                "secretsmanager:DescribeSecret",
                "secretsmanager:ListSecretVersionIds"
            ],
            "Resource": [
                "arn:aws:secretsmanager:*:*:secret:*Redshift*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetRandomPassword",
                "secretsmanager:ListSecrets"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "secretsmanager:ResourceTag/Redshift": "true"
                }
            }
},
```

## Managing IAM roles created for a cluster using the console<a name="managing-iam-role-console"></a>

To create, modify, and remove IAM roles created from the Amazon Redshift console, use the **Clusters** section in the console\.

### Creating an IAM role as the default<a name="create-iam"></a>

On the console, you can create an IAM role for your cluster that has the `AmazonRedshiftAllCommandsFullAccess` policy automatically attached\. The new IAM role that you create allows Amazon Redshift to copy, load, query, and analyze data from Amazon resources in your IAM account\.

There can only be one IAM role set as the default for the cluster\. If you create another IAM role as the cluster default when an existing IAM role is currently assigned as the default, the new IAM role replaces the other one as default\.

**To create a new cluster and an IAM role set as the default for the new cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose **Create cluster** to create a cluster\.

1. Follow the instructions on the console page to enter the properties for **Cluster configuration**\. 

   Choose one of the following methods to size your cluster:
**Note**  
The following step assumes an AWS Region that supports RA3 node types\. For a list of AWS Regions that support RA3 node types, see [Overview of RA3 node types](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html#rs-ra3-node-types) in the *Amazon Redshift Management Guide*\. 
   + If your AWS Region supports RA3 node types, choose either **Production** or **Free trial** to answer the question **What are you planning to use this cluster for?** 

     If your organization is eligible, you might be able to create a cluster under the Amazon Redshift free trial program\. To do this, choose **Free trial** to create a configuration with the dc2\.large node type\. For more information about choosing a free trial, see [Amazon Redshift free trial](http://aws.amazon.com/redshift/free-trial/)\. 
   + If you don't know how large to size your cluster, choose **Help me choose**\. Doing this starts a sizing calculator that asks you questions about the size and query characteristics of the data that you plan to store in your data warehouse\. 

     If you know the required size of your cluster \(that is, the node type and number of nodes\), choose **I'll choose**\. Then choose the **Node type** and number of **Nodes** to size your cluster\.

1. \(Optional\) Choose **Load sample data** to load the sample data set to your Amazon Redshift cluster to start using the query editor to query data\. 

   If you are behind a firewall, the database port must be an open port that accepts inbound connections\. 

1. Follow the instructions on the console page to enter properties for **Database configurations**\.

1. Under **Cluster permissions**, from **Manage IAM roles**, choose **Create IAM role**\.

1. Specify an Amazon S3 bucket for the IAM role to access by choosing one of the following methods:
   + Choose **No additional Amazon S3 bucket** to create the IAM role without specifying specific Amazon S3 buckets\.
   + Choose **Any Amazon S3 bucket** to allow users that have access to your Amazon Redshift cluster to also access any Amazon S3 bucket and its contents in your AWS account\.
   + Choose **Specific Amazon S3 buckets** to specify one or more Amazon S3 buckets that the IAM role being created has permission to access\. Then choose one or more Amazon S3 buckets from the table\.

1. Choose **Create IAM role as default**\. Amazon Redshift automatically creates and sets the IAM role as the default for your cluster\.

1. Choose **Create cluster** to create the cluster\. The cluster might take several minutes to be ready to use\.

### Removing IAM roles from your cluster<a name="remove-iam"></a>

You can remove one or more IAM roles from your cluster\.

**To remove IAM roles from your cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the cluster that you want to remove the IAM role from\.

1. Under **Cluster permissions**, choose one or more IAM roles that you want to remove from the cluster\.

1. From **Manage IAM roles**, choose **Remove IAM roles**\.

### Associating IAM roles with your cluster<a name="associate-iam"></a>

You can associate one or more IAM roles with your cluster\.

**To associate IAM roles with your cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the cluster that you want to associate IAM roles with\.

1. Under **Cluster permissions**, choose one or more IAM roles that you want to associate with the cluster\.

1. From **Manage IAM roles**, choose **Associate IAM roles**\.

1. Choose one ore more IAM roles to associate with your cluster\.

1. Choose **Associate IAM roles**\.

### Setting an IAM role as the default<a name="set-default-iam"></a>

You can set an IAM role as the default for your cluster\.

**To make an IAM role the default for your cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the cluster that you want to set a default IAM role for\.

1. Under **Cluster permissions**, from **Associated IAM roles**, choose an IAM role that you want make as default for the cluster\.

1. Under **Set default**, choose **Make default**\.

1. When prompted, choose **Set default** to confirm making the specified IAM role as the default\.

### Making an IAM role no longer default for your cluster<a name="clear-default-iam"></a>

You can make an IAM role no longer the default for your cluster\.

**To clear an IAM role as the default for your cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the cluster that you want to associate IAM roles with\.

1. Under **Cluster permissions**, from **Associated IAM roles**, choose the default IAM role\.

1. Under **Set default**, choose **Clear default**\.

1. When prompted, choose **Clear default** to confirm clearing the specified IAM role as the default\.

## Managing IAM roles created on the cluster using the AWS CLI<a name="managing-iam-role-association-with-cluster-cli"></a>

You can manage IAM roles created on the cluster using the AWS CLI\.

### To create an Amazon Redshift cluster with an IAM role set as default<a name="create-cluster-iam"></a>

To create an Amazon Redshift cluster with an IAM role set it as the default for the cluster, use the `aws redshift create-cluster` AWS CLI command\.

The following AWS CLI command creates an Amazon Redshift cluster and the IAM role named myrole1\. The AWS CLI command also sets myrole1 as the default for the cluster\.

```
aws redshift create-cluster \
    --node-type dc2.large \
    --number-of-nodes 2 \
    --master-username adminuser \
    --master-user-password TopSecret1 \
    --cluster-identifier mycluster \
    --iam-roles 'arn:aws:iam::012345678910:role/myrole1' 'arn:aws:iam::012345678910:role/myrole2' \
    --default-iam-role-arn 'arn:aws:iam::012345678910:role/myrole1'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",      
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "adding"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "adding"
            }
        ]
        ...
    }
}
```

### To add one or more IAM roles to an Amazon Redshift cluster<a name="modify-cluster-add-iam"></a>

To add one or more IAM roles associated to the cluster, use the `aws redshift modify-cluster-iam-roles` AWS CLI command\.

The following AWS CLI command adds `myrole3` and `myrole4` to the cluster\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier mycluster \
    --add-iam-roles 'arn:aws:iam::012345678910:role/myrole3' 'arn:aws:iam::012345678910:role/myrole4'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole3",
                "ApplyStatus": "adding"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole4",
                "ApplyStatus": "adding"
            }
        ],
        ...
    }
}
```

### To remove one or more IAM roles from an Amazon Redshift cluster<a name="modify-cluster-remove-iam"></a>

To remove one or more IAM roles associated to the cluster, use the `aws redshift modify-cluster-iam-roles` AWS CLI command\.

The following AWS CLI command removes `myrole3` and `myrole4` from the cluster\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier mycluster \
    --remove-iam-roles 'arn:aws:iam::012345678910:role/myrole3' 'arn:aws:iam::012345678910:role/myrole4'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole3",
                "ApplyStatus": "removing"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole4",
                "ApplyStatus": "removing"
            }
        ],
        ...
    }
}
```

### To set an associated IAM role as the default for the cluster<a name="modify-cluster-default-iam-associated"></a>

To set an associated IAM role as the default for the cluster, use the `aws redshift modify-cluster-iam-roles` AWS CLI command\.

The following AWS CLI command sets `myrole2` as the default for the cluster\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier mycluster \
    --default-iam-role-arn 'arn:aws:iam::012345678910:role/myrole2'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "in-sync"
            }
        ],
        ...
    }
}
```

### To set an unassociated IAM role as the default for the cluster<a name="modify-cluster-default-iam-not-associated"></a>

To set an unassociated IAM role as the default for the cluster, use the `aws redshift modify-cluster-iam-roles` AWS CLI command\.

The following AWS CLI command adds `myrole2` to the Amazon Redshift cluster and sets it as the default for the cluster\.

```
aws redshift modify-cluster-iam-roles \
    --cluster-identifier mycluster \
    --add-iam-roles 'arn:aws:iam::012345678910:role/myrole3' \
    --default-iam-role-arn 'arn:aws:iam::012345678910:role/myrole3'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole3",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "in-sync"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole3",
                "ApplyStatus": "adding"
            }
        ],
        ...
    }
}
```

### To restore a cluster from a snapshot and set an IAM role as the default for it<a name="restore-cluster-iam"></a>

When you restore your cluster from a snapshot, you can either associate an existing IAM role or create a new one and set it as the default for the cluster\.

To restore an Amazon Redshift cluster from a snapshot and set an IAM role as the cluster default, use the `aws redshift restore-from-cluster-snapshot` AWS CLI command\.

The following AWS CLI command restores the cluster from a snapshot and sets `myrole2` as the default for the cluster\.

```
aws redshift restore-from-cluster-snapshot \
    --cluster-identifier mycluster-clone \
    --snapshot-identifier my-snapshot-id
    --iam-roles 'arn:aws:iam::012345678910:role/myrole1' 'arn:aws:iam::012345678910:role/myrole2' \
    --default-iam-role-arn 'arn:aws:iam::012345678910:role/myrole1'
```

The following snippet is an example of the response\.

```
{
    "Cluster": {
        "ClusterIdentifier": "mycluster-clone",
        "NodeType": "dc2.large",
        "MasterUsername": "adminuser",
        "DefaultIamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
        "IamRoles": [
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole1",
                "ApplyStatus": "adding"
            },
            {
                "IamRoleArn": "arn:aws:iam::012345678910:role/myrole2",
                "ApplyStatus": "adding"
            }
        ],
        ...
    }
}
```