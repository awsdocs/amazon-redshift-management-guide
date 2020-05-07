# Getting started with the AWS Command Line Interface<a name="getting-started-cli"></a>

To help you get started using the AWS Command Line Interface \(AWS CLI\), this section shows how to perform basic administrative tasks for an Amazon Redshift cluster\. These tasks are very similar to those in the [Amazon Redshift Getting Started](https://docs.aws.amazon.com/redshift/latest/gsg/), but they are focused on the AWS CLI rather than the Amazon Redshift console\.

This section walks you through the process of creating a cluster, creating database tables, uploading data, and testing queries\. You use the AWS CLI to provision a cluster and to authorize necessary access permissions\. You will then use the SQL Workbench client to connect to the cluster and create sample tables, upload sample data, and execute test queries\.

## Step 1: Before you begin<a name="getting-started-cli.before-you-begin"></a>

If you don't already have an AWS account, you must sign up for one\. Then you'll need to set up the Amazon Redshift command line tools\. Finally, you'll need to download client tools and drivers in order to connect to your cluster\.

### Step 1\.1: Sign up for an AWS account<a name="getting-started-cli.before-you-begin.sign-up"></a>

For information about signing up for an AWS user account, see the [Amazon Redshift Getting Started Guide](https://docs.aws.amazon.com/redshift/latest/gsg/)\.

### Step 1\.2: Download and install the AWS CLI<a name="getting-started-cli.download-aws-cli"></a>

If you have not installed the AWS CLI, see [Setting up the Amazon Redshift CLI](setting-up-rs-cli.md)\.

### Step 1\.3: Download the client tools and drivers<a name="getting-started-cli.download-client-tools"></a>

You can use any SQL client tools to connect to an Amazon Redshift cluster with PostgreSQL JDBC or ODBC drivers\. If you do not currently have such software installed, you can use SQL Workbench, a free cross\-platform tool that you can use to query tables in an Amazon Redshift cluster\. The examples in this section will use the SQL Workbench client\.

To download SQL Workbench and the PostgreSQL drivers, see the [Amazon Redshift Getting Started Guide](https://docs.aws.amazon.com/redshift/latest/gsg/before-you-begin.html)\.

## Step 2: Launch a cluster<a name="getting-started-launch-cluster-cli"></a>

Now you're ready to launch a cluster by using the AWS CLI\.

**Important**  
The cluster that you're about to launch will be live \(and not running in a sandbox\)\. You will incur the standard usage fees for the cluster until you terminate it\. For pricing information, go to [the Amazon Redshift pricing page](https://aws.amazon.com/redshift/pricing/)\.  
If you complete the exercise described here in one sitting and terminate your cluster when you are finished, the total charges will be minimal\. 

The `create-cluster` command has a large number of parameters\. For this exercise, you will use the parameter values that are described in the following table\. Before you create a cluster in a production environment, we recommend that you review all the required and optional parameters so that your cluster configuration matches your requirements\. For more information, see [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster.html)


| Parameter name | Parameter value for this exercise | 
| --- | --- | 
|  Cluster Identifier |  `examplecluster`  | 
|  Master Username |  `masteruser`  | 
|  Master Password |  `TopSecret1`  | 
| Node Type  | ds2\.xlarge or the node size that you want to use\. For more information, see [Clusters and nodes in Amazon Redshift](working-with-clusters.md#rs-about-clusters-and-nodes) | 
| Cluster Type | single\-node | 

To create your cluster, enter the following command\.

```
aws redshift create-cluster --cluster-identifier examplecluster --master-username masteruser --master-user-password TopSecret1 --node-type ds2.xlarge --cluster-type single-node
```

The cluster creation process will take several minutes to complete\. To check the status, enter the following command\.

```
aws redshift describe-clusters --cluster-identifier examplecluster
```

The output will look similar to the following\.

```
{
    "Clusters": [
        {

       ...output omitted...

            "ClusterStatus": "creating", 
            "ClusterIdentifier": "examplecluster",

       ...output omitted...

}
```

When the **ClusterStatus** field changes from `creating` to `available`, your cluster is ready for use\.

In the next step, you will authorize access so that you can connect to the cluster\. 

## Step 3: Authorize inbound traffic for cluster access<a name="getting-started-authorize-access-cli"></a>

You must explicitly grant inbound access to your client in order to connect to the cluster\. Your client can be an Amazon EC2 instance or an external computer\.

When you created a cluster in the previous step, because you did not specify a security group, you associated the default cluster security group with the cluster\. The default cluster security group contains no rules to authorize any inbound traffic to the cluster\. To access the new cluster, you must add rules for inbound traffic, which are called ingress rules, to the cluster security group\.

### Ingress rules for applications running on the internet<a name="getting-started-authorize-access-cli.cidr"></a>

If you are accessing your cluster from the Internet, you will need to authorize a Classless Inter\-Domain Routing IP \(CIDR/IP\) address range\. For this example, we will use a CIDR/IP rule of 192\.0\.2\.0/24; you will need to modify this range to reflect your actual IP address and netmask\.

To allow network ingress to your cluster, enter the following command\.

```
aws redshift authorize-cluster-security-group-ingress --cluster-security-group-name default --cidrip 192.0.2.0/24
```

### Ingress rules for EC2 instances<a name="getting-started-authorize-access-cli.ec2"></a>

If you are accessing your cluster from an Amazon EC2 instance, you will need to authorize an Amazon EC2 security group\. To do so, you specify the security group name, along with the 12\-digit account number of the EC2 security group owner\.

You can use the Amazon EC2 console to determine the EC2 security group associated with your instance:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/cmdws-gsg-console-launch-cluster-wizard-150.png)

To find your AWS account number, go to [https://aws\.amazon\.com/](https://aws.amazon.com/) and sign in to the My Account page\. Your AWS account number is shown in the upper right\-hand corner of that page\.

For this example, we will use `myec2securitygroup` for the Amazon EC2 security group name, and `123456789012` for the account number\. You will need to modify these to suit your needs\.

To allow network ingress to your cluster, enter the following command\.

```
 aws redshift authorize-cluster-security-group-ingress --cluster-security-group-name default --ec2-security-group-name myec2securitygroup --ec2-security-group-owner 123456789012
```

## Step 4: Connect to your cluster<a name="getting-started-connect-to-the-cluster-cli"></a>

Now that you have added an ingress rule to the default cluster security group, incoming connections from a specific CIDR/IP or EC2 Security Group to `examplecluster` are authorized\.

You are now ready to connect to the cluster\.

For information about connecting to your cluster, go to the [Amazon Redshift getting started guide](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)\.

## Step 5: Create tables, upload data, and try example queries<a name="getting-started-create-sample-db-cli"></a>

For information about creating tables, uploading data, and issuing queries, go to the [Amazon Redshift Getting Started](https://docs.aws.amazon.com/redshift/latest/gsg/)\.

## Step 6: Delete your sample cluster<a name="getting-started-terminate-cluster-cli"></a>

After you have launched a cluster and it is available for use, you are billed for the time the cluster is running, even if you are not actively using it\. When you no longer need the cluster, you can delete it\.

When you delete a cluster, you must decide whether to create a final snapshot\. Because this is an exercise and your test cluster should not have any important data in it, you can skip the final snapshot\.

To delete your cluster, enter the following command\.

```
aws redshift delete-cluster --cluster-identifier examplecluster --skip-final-cluster-snapshot
```

Congratulations\! You successfully launched, authorized access to, connected to, and terminated a cluster\.