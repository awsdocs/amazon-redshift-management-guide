# Authorizing Amazon Redshift to access other AWS services on your behalf<a name="authorizing-redshift-service"></a>

Some Amazon Redshift features require Amazon Redshift to access other AWS services on your behalf\. For example, the [COPY](https://docs.aws.amazon.com/redshift/latest/dg/r_COPY.html) and [UNLOAD](https://docs.aws.amazon.com/redshift/latest/dg/r_UNLOAD.html) commands can load or unload data into your Amazon Redshift cluster using an Amazon Simple Storage Service \(Amazon S3\) bucket\. Amazon Redshift Spectrum can use a data catalog in Amazon Athena or AWS Glue\. For your Amazon Redshift clusters to act on your behalf, you supply security credentials to your clusters\. The preferred method to supply security credentials is to specify an AWS Identity and Access Management \(IAM\) role\. For COPY and UNLOAD, you can provide AWS access keys\. 

Following, find out how to create an IAM role with the appropriate permissions to access other AWS services\. You also need to associate the role with your cluster and specify the Amazon Resource Name \(ARN\) of the role when you execute the Amazon Redshift command\. For more information, see [Authorizing COPY, UNLOAD, and CREATE EXTERNAL SCHEMA operations using IAM roles](copy-unload-iam-role.md)\.

## Creating an IAM role to allow your Amazon Redshift cluster to access AWS services<a name="authorizing-redshift-service-creating-an-iam-role"></a>

To create an IAM role to permit your Amazon Redshift cluster to communicate with other AWS services on your behalf, take the following steps\. The values used in this section are examples, you can choose values based on your needs\.<a name="create-iam-role-for-aws-services"></a>

**To create an IAM role to allow Amazon Redshift to access AWS services**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home?#home)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. Choose **AWS service**, and then choose **Redshift**\.

1. Under **Select your use case**, choose **Redshift \- Customizable** and then choose **Next: Permissions**\. The **Attach permissions policy** page appears\.

1. For access to Amazon S3 using COPY, as an example, you can use **AmazonS3ReadOnlyAccess** and append\. For access to Amazon S3 using COPY or UNLOAD, we suggest that you can create managed policies that restrict access to the desired bucket and prefix accordingly\. For both read and write operations, we recommend enforcing the least privileges and restricting to only the Amazon S3 buckets and key prefixes that Amazon Redshift requires\.

   For Redshift Spectrum, in addition to Amazon S3 access, add **AWSGlueConsoleFullAccess** or **AmazonAthenaFullAccess**\.

   Choose **Next: Tags**\.

1. The **Add tags** page appears\. You can optionally add tags\. Choose **Next: Review**\.

1. For **Role name**, type a name for your role, for example **RedshiftCopyUnload**\. Choose ****Create role****\.

1. The new role is available to all users on clusters that use the role\. To restrict access to only specific users on specific clusters, or to clusters in specific regions, edit the trust relationship for the role\. For more information, see [Restricting access to IAM roles](#authorizing-redshift-service-database-users)\.

1. Associate the role with your cluster\. You can associate an IAM role with a cluster when you create the cluster, or you add the role to an existing cluster\. For more information, see [Associating IAM roles with clusters](copy-unload-iam-role.md#copy-unload-iam-role-associating-with-clusters)\.
**Note**  
To restrict access to specific data, use an IAM role that grants the least privileges required\.

## Restricting access to IAM roles<a name="authorizing-redshift-service-database-users"></a>

By default, IAM roles that are available to an Amazon Redshift cluster are available to all users on that cluster\. You can choose to restrict IAM roles to specific Amazon Redshift database users on specific clusters or to specific regions\. 

To permit only specific database users to use an IAM role, take the following steps\.<a name="identify-db-users-for-iam-role"></a>

**To identify specific database users with access to an IAM role**

1. Identify the Amazon Resource Name \(ARN\) for the database users in your Amazon Redshift cluster\. The ARN for a database user is in the format: `arn:aws:redshift:region:account-id:dbuser:cluster-name/user-name`\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/home?#home) at [url="https://console\.aws\.amazon\.com/](https://console.aws.amazon.com/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the IAM role that you want to restrict to specific Amazon Redshift database users\.

1. Choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\. A new IAM role that allows Amazon Redshift to access other AWS services on your behalf has a trust relationship as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "redshift.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Add a condition to the `sts:AssumeRole` action section of the trust relationship that limits the `sts:ExternalId` field to values that you specify\. Include an ARN for each database user that you want to grant access to the role\.

   For example, the following trust relationship specifies that only database users `user1` and `user2` on cluster `my-cluster` in region `us-west-2` have permission to use this IAM role\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
     {
       "Effect": "Allow",
       "Principal": { 
         "Service": "redshift.amazonaws.com" 
       },
       "Action": "sts:AssumeRole",
       "Condition": {
         "StringEquals": {
           "sts:ExternalId": [
             "arn:aws:redshift:us-west-2:123456789012:dbuser:my-cluster/user1",
             "arn:aws:redshift:us-west-2:123456789012:dbuser:my-cluster/user2"
           ]
         }
       }
     }]
   }
   ```

1. Choose **Update Trust Policy**\.

## Restricting an IAM role to an AWS Region<a name="authorizing-redshift-service-regions"></a>

You can restrict an IAM role to only be accessible in a certain AWS Region\. By default, IAM roles for Amazon Redshift are not restricted to any single region\.

To restrict use of an IAM role by region, take the following steps\.<a name="identify-regionsfor-iam-role"></a>

**To identify permitted regions for an IAM role**

1. Open the [IAM console](https://console.aws.amazon.com/iam/home?#home) at [https://console\.aws\.amazon\.com](https://console.aws.amazon.com/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the role that you want to modify with specific regions\.

1. Choose the **Trust Relationships** tab and then choose **Edit Trust Relationship**\. A new IAM role that allows Amazon Redshift to access other AWS services on your behalf has a trust relationship as follows:

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "redshift.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Modify the `Service` list for the `Principal` with the list of the specific regions that you want to permit use of the role for\. Each region in the `Service` list must be in the following format: `redshift.region.amazonaws.com`\.

   For example, the following edited trust relationship permits the use of the IAM role in the `us-east-1` and `us-west-2` regions only\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": [
             "redshift.us-east-1.amazonaws.com",
             "redshift.us-west-2.amazonaws.com"
           ]
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Choose **Update Trust Policy**

## Chaining IAM roles in Amazon Redshift<a name="authorizing-redshift-service-chaining-roles"></a>

When you attach a role to your cluster, your cluster can assume that role to access Amazon S3, Athena, and AWS Glue on your behalf\. If a role attached to your cluster doesn't have access to the necessary resources, you can chain another role, possibly belonging to another account\. Your cluster then temporarily assumes the chained role to access the data\. You can also grant cross\-account access by chaining roles\. Each role in the chain assumes the next role in the chain, until the cluster assumes the role at the end of chain\. You can chain a maximum of 10 roles\. 

For example, suppose Company A wants to access data in an Amazon S3 bucket that belongs to Company B\. Company A creates an AWS service role for Amazon Redshift named `RoleA` and attaches it to their cluster\. Company B creates a role named `RoleB` that's authorized to access the data in the Company B bucket\. To access the data in the Company B bucket, Company A runs a COPY command using an `iam_role` parameter that chains `RoleA` and `RoleB`\. For the duration of the COPY operation, `RoleA` temporarily assumes `RoleB` to access the Amazon S3 bucket\. 

To chain roles, you establish a trust relationship between the roles\. A role that assumes another role \(for example, `RoleA`\) must have a permissions policy that allows it to assume the next chained role \(for example, `RoleB`\)\. In turn, the role that passes permissions \(`RoleB`\) must have a trust policy that allows it to pass its permissions to the previous chained role \(`RoleA`\)\. For more information, see [Using IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use.html) in the IAM User Guide\. 

The first role in the chain must be a role attached to the cluster\. The first role, and each subsequent role that assumes the next role in the chain, must have a policy that includes a specific statement\. This statement has the `Allow` effect on the `sts:AssumeRole `action and the Amazon Resource Name \(ARN\) of the next role in a `Resource` element\. In our example, `RoleA` has the following permission policy that allows it to assume `RoleB`, owned by AWS account `210987654321`\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1487639602000",
            "Effect": "Allow",
            "Action": [
                "sts:AssumeRole"
            ],
            "Resource": "arn:aws:iam::210987654321:role/RoleB"        
		}
    ]
}
```

A role that passes to another role must establish a trust relationship with the role that assumes the role or with the AWS account that owns the role\. In our example, `RoleB` has the following trust policy to establish a trust relationship with `RoleA`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::role/RoleA"
      }
  ]
}
```

The following trust policy establishes a trust relationship with the owner of `RoleA`, AWS account `123456789012`\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      }
    }      
  ]
}
```

When you run an UNLOAD, COPY, or CREATE EXTERNAL SCHEMA command, you chain roles by including a comma\-separated list of role ARNs in the `iam_role` parameter\. The following shows the syntax for chaining roles in the `iam_role` parameter\. 

```
unload ('select * from venue limit 10') 
to 's3://acmedata/redshift/venue_pipe_'
IAM_ROLE 'arn:aws:iam::<aws-account-id-1>:role/<role-name-1>[,arn:aws:iam::<aws-account-id-2>:role/<role-name-2>][,...]';
```

**Note**  
The entire role chain is enclosed in single quotes and must not contain spaces\.

In the following examples, `RoleA` is attached to the cluster belonging to AWS account `123456789012`\. `RoleB`, which belongs to account `210987654321`, has permission to access the bucket named `s3://companyb/redshift/`\. The following example chains `RoleA` and `RoleB` to UNLOAD data to the s3://companyb/redshift/ bucket\. 

```
unload ('select * from venue limit 10') 
to 's3://companyb/redshift/venue_pipe_'
iam_role 'arn:aws:iam::123456789012:role/RoleA,arn:aws:iam::210987654321:role/RoleB';
```

The following example uses a COPY command to load the data that was unloaded in the previous example\.

```
copy venue 
from 's3://companyb/redshift/venue_pipe_'
iam_role 'arn:aws:iam::123456789012:role/RoleA,arn:aws:iam::210987654321:role/RoleB';
```

In the following example, CREATE EXTERNAL SCHEMA uses chained roles to assume the role `RoleB`\.

```
create external schema spectrumexample from data catalog 
database 'exampledb' region 'us-west-2' 
iam_role 'arn:aws:iam::123456789012:role/RoleA,arn:aws:iam::210987654321:role/RoleB';
```

## Related topics<a name="authorizing-redshift-related-topic"></a>
+ [Authorizing COPY, UNLOAD, and CREATE EXTERNAL SCHEMA operations using IAM roles](copy-unload-iam-role.md)