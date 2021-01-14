# Managing cluster relocation in Amazon Redshift<a name="managing-cluster-recovery"></a>

By using *relocation* in Amazon Redshift, you enable Amazon Redshift to move a cluster to another Availability Zone \(AZ\) without any loss of data or changes to your applications\. With relocation, you can continue operations when there is an interruption of service on your cluster with minimal impact\.  

When cluster relocation is enabled, Amazon Redshift might choose to relocate clusters in some situations\. In particular, this happens where issues in the current Availability Zone prevent optimal cluster operation or to improve service availability\. You can also invoke the relocation function in cases where resource constraints in a given Availability Zone are disrupting cluster operations\. An example is the ability to resume or resize a cluster\. Amazon Redshift offers the relocation feature at no extra charge\.

When an Amazon Redshift cluster is relocated to a new Availability Zone, the new cluster has the same endpoint as the original cluster\. Your applications can reconnect to the endpoint and continue operations without modifications or loss of data\. However, relocation might not always be possible due to potential resource constraints in a given Availability Zone\.

Amazon Redshift cluster relocation is supported for the RA3 instance types only, such as ra3\.16xlarge, ra3\.4xlarge, and ra3\.xlplus\. RA3 instance types use Redshift Managed Storage \(RMS\) as a durable storage layer\. The latest copy of a cluster's data is always available in other Availability Zones in an AWS Region\. In other words, you can relocate an Amazon Redshift cluster to another Availability Zone without any loss of data\. 

When you enable your cluster for relocation, Amazon Redshift migrates your cluster to be behind a proxy\. Doing this helps implement location\-independent access to a cluster's compute resources\. The migration causes the cluster to be rebooted\. When a cluster is relocated to another Availability Zone, an outage occurs while the new cluster is brought back online in the new Availability Zone\. However, you don't have to make any changes to your applications because the cluster endpoint remains unchanged even after the cluster is relocated to the new Availability Zone\. 

If you enable relocation and you currently use the leader node IP address to access your cluster, make sure to change that access\. Instead, use the IP address associated with the cluster's virtual private cloud \(VPC\) endpoint\. To find this cluster IP address, find and use the VPC endpoint in the **Network and security** section of the cluster details page\. To get more details on the VPC endpoint, sign in to the Amazon VPC console\. 

You can also use the AWS Command Line Interface \(AWS CLI\) command `describe-vpc-endpoints` to get the elastic network interface associated with the endpoint\. You can use the `describe-network-interfaces` command to get the associated IP address\. For more information on Amazon Redshift AWS CLI commands, see [ Available commands](https://docs.aws.amazon.com/cli/latest/reference/redshift/index.html) in the *AWS CLI Command Reference\.*  

## Enabling cluster relocation<a name="using-recovery"></a>

You can enable and manage cluster relocation from the Amazon Redshift console, AWS CLI, and Amazon Redshift API\.  

To enable cluster relocation, define a subnet group that includes multiple Availability Zones\. If Amazon Redshift identifies more than one accessible Availability Zone, Amazon Redshift automatically chooses from the list of accessible Availability Zones to relocate the cluster\.

After relocation is complete, you use the same endpoint to access the cluster\. Amazon Redshift deletes the original cluster's compute resources and returns them to the resource pool\.

## Limitations<a name="limitations-recovery"></a>

When using Amazon Redshift relocation, be aware of the following limitations:
+ Cluster relocation might not be possible in all scenarios due to potential resource limitations in a given Availability Zone\. If this happens, Amazon Redshift doesn't change the original cluster\.
+ Relocation isn't supported on DC1, DC2, or the DS2 instance families of products\.
+ Relocation isn't available for a publicly accessible Amazon Redshift cluster\.
+ You can't perform a relocation across AWS Regions\.
+ You can enable relocation only for a cluster that uses the default port setting \(5439\)\. Otherwise, enabling relocation fails\. 
+ If you have enabled relocation successfully and later attempt to modify the default port setting, the modify operation fails\.

## Managing relocation using the console<a name="cluster-recovery-console"></a>

You can manage the settings for cluster relocation using the Amazon Redshift console\.

### Enabling relocation when creating a new cluster<a name="enable-relocate-new-cluster."></a>

Use the following procedure to enable relocation when creating a new cluster\. 

**To enable relocation for a new cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **CLUSTERS**\. 

1. Choose **Create cluster** to create a new cluster\. For more information on how to create a cluster, see [Create a sample Amazon Redshift cluster](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-launch-sample-cluster.html) in *Amazon Redshift Getting Started*\.

1. Under **Backup**, for **Cluster relocation**, choose **Enable**\. Relocation is disabled by default\.

1. Under **Network and security**, for **Publicly accessible**, accept the default **No**\. If you choose **Yes**, Amazon Redshift returns an error\.

1. Choose **Create cluster**\.

### Modifying relocation for an existing cluster<a name="modify-relocate-cluster."></a>

Use the following procedure to change the relocation setting for an existing cluster\.

**To modify the relocation setting for an existing cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **CLUSTERS**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the name of the cluster that you want to modify from the list\. The cluster details page appears\.

1. Choose **Backup**, then choose **Edit**\.

1. Under **Backup**, choose **Enable**\. Relocation is disabled by default\. 

1. In the **Network and security** section, make sure to choose **No** for the **Publicly accessible** option\.

1. Choose **Modify cluster**\.

### Relocating a cluster<a name="relocate-cluster."></a>

Use the following procedure to manually relocate a cluster to another Availability Zone\. This is especially useful when you want to test your network setup in secondary Availability Zones or when you are running into resource constraints in the current Availability Zone\. 

**To relocate a cluster to another Availability Zone**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **CLUSTERS**\. The clusters for your account in the current AWS Region are listed\. A subset of properties of each cluster is displayed in columns in the list\.

1. Choose the name of the cluster that you want to move from the list\. The cluster details page appears\.

1. For **Actions**, choose **Relocate**\. The **Relocate cluster** page appears\.

1. \(Optional\) Choose an **Availability Zone**\. If you don't choose an Availability Zone, Amazon Redshift chooses one for you\.

Amazon Redshift starts the relocation and displays the cluster as relocating\. After the relocation completes, the cluster status changes to available\.

## Managing relocation using the Amazon Redshift CLI<a name="cluster-recovery-cli"></a>

You can manage the settings for cluster relocation using the AWS Command Line Interface \(CLI\)\.

With the AWS CLI, the following example command creates an Amazon Redshift cluster named **mycluster** that has relocation enabled\.

```
aws redshift create-cluster --cluster-identifier mycluster --number-of-nodes 2 --master-username adminuser --master-user-password TopSecret1 --node-type ra3.4xlarge --port 5439 --no-publicly-accessible --availability-zone-relocation
```

If your current cluster is using a different port, you have to modify it to use 5439 before modifying it to enable relocation\. The following example command modify the port in case your cluster doesn't use 5439\.

```
aws redshift modify-cluster --cluster-identifier mycluster --port 5439
```

The following example command enables the availability\-zone\-relocation parameter on the Amazon Redshift cluster\.

```
aws redshift modify-cluster --cluster-identifier mycluster --availability-zone-relocation
```

The following example command disables the availability\-zone\-relocation parameter on the Amazon Redshift cluster\.

```
aws redshift modify-cluster --cluster-identifier mycluster --no-availability-zone-relocation
```

The following example command invokes relocation on the Amazon Redshift cluster\.

```
aws redshift modify-cluster --cluster-identifier mycluster --availability-zone us-east-1b
```