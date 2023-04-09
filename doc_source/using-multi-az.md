# Managing Multi\-AZ deployment<a name="using-multi-az"></a>

Amazon Redshift Multi\-AZ supports two Availability Zones at a time\. Amazon Redshift selects the Availability Zones automatically based on the network connectivity configured\. Also, you can specify which Availability Zones to use explicitly\. To convert an existing single Availability Zone data warehouse to a Multi\-AZ deployment, you can restore from a snapshot to configure it into a Multi\-AZ data warehouse\. 

Using the Amazon Redshift console, you can easily create new Multi\-AZ deployments\. To create a new Multi\-AZ deployment using the Amazon Redshift console, select the Multi\-AZ option when creating the data warehouse\. Specify the number of compute nodes required in a single Availability Zone, and Amazon Redshift will deploy the number of nodes in each of two Availability Zones\. All nodes can perform read and write workload processing during normal operation\.

Multi\-AZ deployment only supports RA3 node types that use Amazon Redshift Managed Storage \(RMS\)\. Amazon Redshift stores data in RMS, which uses Amazon S3 and is accessible in all Availability Zones in an AWS Region, without having to replicate the data at the Amazon Redshift level\. RMS isn't supported on DC2 clusters\.