# Install the Amazon Redshift ODBC Driver on Mac OS X<a name="install-odbc-driver-mac"></a>

## System Requirements<a name="odbc-driver-sysreq-mac"></a>

You install the driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet the following minimum system requirements: 
+ Mac OS X version 10\.6\.8 or later
+ 215 MB of available disk space
+ iODBC Driver Manager version 3\.52\.7 or later\. For more information about the iODBC driver manager and links to download it, go to the [Independent Open Database Connectivity website](http://www.iodbc.org/dataspace/doc/iodbc/wiki/iodbcWiki/Downloads)\.
+ An Amazon Redshift master user or user account to connect to the database

## Installing the Amazon Redshift Driver on Mac OS X<a name="odbc-driver-mac-how-to-install"></a>

Use the steps in this section to download and install the Amazon Redshift ODBC driver on a supported version of Mac OS X\. The installation process will install the driver files in the following directories: 
+ /opt/amazon/redshift/lib/universal
+ /opt/amazon/redshift/ErrorMessages
+ /opt/amazon/redshift/Setup<a name="rs-mgmt-install-odbc-drivers-mac"></a>

**To install the Amazon Redshift ODBC driver on Mac OS X**

1. Download [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC\-1\.4\.8\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC-1.4.8.1000.dmg)\. The name for this driver is Amazon Redshift ODBC Driver\. 

   Then download and review the [Amazon Redshift ODBC Driver License Agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+Driver+License+Agreement.pdf)\. If you need to distribute these drivers to your customers or other third parties, please email *redshift\-pm@amazon\.com* to arrange an appropriate license\. 

1. Double\-click **AmazonRedshiftODBC\.dmg** to mount the disk image\.

1. Double\-click **AmazonRedshiftODBC\.pkg** to run the installer\.

1. Follow the steps in the installer to complete the driver installation process\. You'll need to agree to the terms of the license agreement to perform the installation\.

**Important**  
When you have finished installing the driver, configure it for use on your system\. For more information on driver configuration, see [Configure the ODBC Driver on Linux and Mac OS X Operating Systems](odbc-driver-configure-linux-mac.md)\.