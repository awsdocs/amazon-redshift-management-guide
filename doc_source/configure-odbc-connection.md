# Configure an ODBC Connection<a name="configure-odbc-connection"></a>

You can use an ODBC connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools and applications\. To do this, you need to set up the connection on your client computer or Amazon EC2 instance\. If your client tool supports JDBC, you might choose to use that type of connection rather than ODBC due to the ease of configuration that JDBC provides\. However, if your client tool doesn't support JDBC, follow the steps in this section to configure an ODBC connection\. 

Amazon Redshift provides ODBC drivers for Linux, Windows, and Mac OS X operating systems\. Before you install an ODBC driver, you need to determine whether your SQL client tool is 32\-bit or 64\-bit\. You should install the ODBC driver that matches the requirements of your SQL client tool; otherwise, the connection will not work\. If you use more than one SQL client tool on the same computer or instance, make sure that you download the appropriate drivers\. You might need to install both the 32\-bit and the 64\-bit drivers if the tools differ in their system architecture\. 

Check the [Amazon Redshift ODBC Driver Release Notes](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.7.1000/Amazon+Redshift+ODBC+Driver+Release+Notes.pdf) for the lasted information about driver functionality and prerequisites\. 

**Topics**
+ [Obtain the ODBC URL for Your Cluster](#obtain-odbc-url)
+ [Install and Configure the Amazon Redshift ODBC Driver on Microsoft Windows Operating Systems](install-odbc-driver-windows.md)
+ [Install the Amazon Redshift ODBC Driver on Linux Operating Systems](install-odbc-driver-linux.md)
+ [Install the Amazon Redshift ODBC Driver on Mac OS X](install-odbc-driver-mac.md)
+ [Configure the ODBC Driver on Linux and Mac OS X Operating Systems](odbc-driver-configure-linux-mac.md)
+ [ODBC Driver Configuration Options](configure-odbc-options.md)
+ [Previous ODBC Driver Versions](odbc-previous-versions.md)

## Obtain the ODBC URL for Your Cluster<a name="obtain-odbc-url"></a>

Amazon Redshift displays the ODBC URL for your cluster in the Amazon Redshift console\. This URL contains the information that you need to set up the connection between your client computer and the database\.

 An ODBC URL has the following format: 

 Driver=\{*driver*\};Server=*endpoint*;Database=*database\_name*;UID=*user\_name*;PWD=*password*;Port=*port\_number* 

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-connection.html)

The following is an example ODBC URL: `Driver={Amazon Redshift (x64)}; Server=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com; Database=dev; UID=masteruser; PWD=insert_your_master_user_password_here; Port=5439` 

**To obtain your ODBC URL**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  At top right, select the region in which you created your cluster\. 

    If you followed the *Amazon Redshift Getting Started*, select **US West \(Oregon\)**\. 

1.  In the left navigation pane, click **Clusters**, and then click your cluster\. 

    If you followed the *Amazon Redshift Getting Started*, click `examplecluster`\. 

1.  On the **Configuration** tab, under **Cluster Database Properties**, copy the ODBC URL of the cluster\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/rs-mgmt-clusters-cluster-database-properties-odbc.png)