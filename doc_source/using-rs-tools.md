# Accessing Amazon Redshift Clusters and Databases<a name="using-rs-tools"></a>

There are several management tools and interfaces you can use to create, manage, and delete Amazon Redshift clusters and the databases within the clusters\.

+ You work with Amazon Web Services management tools and interfaces to create, manage, and delete Amazon Redshift clusters\. These tools and interfaces manage the work of setting up, operating, and scaling a data warehouse; provisioning capacity, monitoring, and backing up the cluster, and applying patches and upgrades to the Amazon Redshift engine\.

  + You can use the AWS Management Console to interactively create, manage, and delete clusters\. The topics in this guide include instructions for using the AWS Management Console to perform specific tasks\.

  + You can use one of several AWS management interfaces or SDKs to programmatically create, manage, and delete clusters\. For more information, see [Using the Amazon Redshift Management Interfaces](using-aws-sdk.md)\.

+ After creating an Amazon Redshift cluster, you can create, manage, and delete databases in the cluster by using client applications or tools that execute SQL statements through the PostgreSQL ODBC or JDBC drivers\.

  + For information about installing client SQL tools and connecting to a cluster, see [Connecting to a Cluster](connecting-to-cluster.md)\.

  + For information about designing databases and the SQL statements supported by Amazon Redshift, go to the [Amazon Redshift Database Developer Guide](http://docs.aws.amazon.com/redshift/latest/dg/welcome.html)\.

The interfaces used to work with Amazon Redshift clusters and databases comply with the mechanisms that control access, such as security groups and IAM policies\. For more information, see [Security](iam-redshift-user-mgmt.md)\.