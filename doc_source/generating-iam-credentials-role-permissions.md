# Step 3: Create an IAM role or user with permissions to call GetClusterCredentials<a name="generating-iam-credentials-role-permissions"></a>

Your SQL client needs authorization to call the `GetClusterCredentials` operation on your behalf\. To provide that authorization, you create an IAM user or role and attach a policy that grants the necessary permissions\.

**To create an IAM role with permissions to call GetClusterCredentials**

1. Using the IAM service, create an IAM user or role\. You can also use an existing user or role\. For example, if you created an IAM role for identity provider access, you can attach the necessary IAM policies to that role\. 

1. Attach a permission policy with permission to call the `redshift:GetClusterCredentials` operation\. Depending on which optional parameters you specify, you can also allow or restrict additional actions and resources in your policy:
   + To permit your SQL client to retrieve cluster ID, AWS Region, and port, include permission to call the `redshift:DescribeClusters` operation with the Redshift cluster resource\. 
   + If you use the `AutoCreate` option, include permission to call `redshift:CreateClusterUser` with the `dbuser` resource\. The following Amazon Resource Name \(ARN\) specifies the Amazon Redshift `dbuser`\. Replace *`region`*, *`account-id`*, and *`cluster-name`* with the values for your AWS Region, account, and cluster\. For *`dbuser-name`*, specify the user name to use to log in to the cluster database\. 

     ```
     arn:aws:redshift:region:account-id:dbuser:cluster-name/dbuser-name
     ```
   + \(Optional\) Add an ARN that specifies the Amazon Redshift `dbname` resource in the following format\. Replace *`region`*, *`account-id`*, and *`cluster-name`* with the values for your AWS Region, account, and cluster\. For `database-name`, specify the name of a database that the user will log in to\. 

     ```
     arn:aws:redshift:region:account-id:dbname:cluster-name/database-name
     ```
   + If you use the `DbGroups` option, include permission to call the `redshift:JoinGroup` operation with the Amazon Redshift `dbgroup` resource in the following format\. Replace *`region`*, *`account-id`*, and *`cluster-name`* with the values for your AWS Region, account, and cluster\. For `dbgroup-name`, specify the name of a user group that the user joins at login\.

     ```
     arn:aws:redshift:region:account-id:dbgroup:cluster-name/dbgroup-name
     ```

For more information and examples, see [Resource policies for GetClusterCredentials](redshift-iam-access-control-identity-based.md#redshift-policy-resources.getclustercredentials-resources)\.

The following example shows a policy that allows the IAM role to call the `GetClusterCredentials` operation\. Specifying the Amazon Redshift `dbuser` resource grants the role access to the database user name `temp_creds_user` on the cluster named `examplecluster`\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "redshift:GetClusterCredentials",
    "Resource": "arn:aws:redshift:us-west-2:123456789012:dbuser:examplecluster/temp_creds_user"
  }
}
```

You can use a wildcard \(\*\) to replace all, or a portion of, the cluster name, user name, and database group names\. The following example allows any user name beginning with `temp_` with any cluster in the specified account\.

**Important**  
The statement in the following example specifies a wildcard character \(\*\) as part of the value for the resource so that the policy permits any resource that begins with the specified characters\. Using a wildcard character in your IAM policies might be overly permissive\. As a best practice, we recommend using the most restrictive policy feasible for your business application\. 

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "redshift:GetClusterCredentials",
    "Resource": "arn:aws:redshift:us-west-2:123456789012:dbuser:*/temp_*"
  }
}
```

The following example shows a policy that allows the IAM role to call the `GetClusterCredentials` operation with the option to automatically create a new user and specify groups the user joins at login\. The `"Resource": "*" `clause grants the role access to any resource, including clusters, database users, or user groups\.

```
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": [
             "redshift:GetClusterCredentials",
             "redshift:CreateClusterUser",
		"redshift:JoinGroup"
            ],
    "Resource": "*"
  }
}
```

For more information, see [Amazon Redshift ARN syntax](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html#arn-syntax-redshift)\.