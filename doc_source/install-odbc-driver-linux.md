# Install the Amazon Redshift ODBC Driver on Linux Operating Systems<a name="install-odbc-driver-linux"></a>

## System Requirements<a name="odbc-driver-sysreq-linux"></a>

You install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet the following minimum system requirements: 

+ One of the following Linux distributions \(32\- and 64\-bit editions\):

  + Red Hat Enterprise Linux \(RHEL\) 5\.0/6\.0/7\.0

  + CentOS 5\.0/6\.0/7\.0

  + Debian 7

  + SUSE Linux Enterprise Server \(SLES\) 11

+ 75 MB of available disk space

+ One of the following ODBC driver managers: 

  + iODBC Driver Manager 3\.52\.7 or later\. For more information about the iODBC driver manager and links to download it, go to the [Independent Open Database Connectivity website](https://www.iodbc.org/dataspace/iodbc/wiki/iODBC/)\.

  + unixODBC 2\.3\.0 or later\. For more information about the unixODBC driver manager and links to download it, go to the [unixODBC website](https://www.unixodbc.org/)\. 

+ An Amazon Redshift master user or user account to connect to the database

## Installing the Amazon Redshift Driver on Linux Operating Systems<a name="odbc-driver-linux-how-to-install"></a>

Use the steps in this section to download and install the Amazon Redshift ODBC drivers on a supported Linux distribution\. The installation process will install the driver files in the following directories: 

+ /opt/amazon/redshiftodbc/lib/32 \(for a 32\-bit driver\)

+ /opt/amazon/redshiftodbc/lib/64 \(for a 64\-bit driver\)

+ /opt/amazon/redshiftodbc/ErrorMessages

+ /opt/amazon/redshiftodbc/Setup<a name="rs-mgmt-install-odbc-drivers-linux"></a>

**To install the Amazon Redshift ODBC driver**

1. Download one of the following, depending on the system architecture of your SQL client tool or application: 

   + 32\-bit \.rpm: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC\-32bit\-1\.3\.7\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC-32bit-1.3.7.1000-1.i686.rpm)

   + 64\-bit \.rpm: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC\-64bit\-1\.3\.7\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC-64bit-1.3.7.1000-1.x86_64.rpm) 

   + Debian 32\-bit \.rpm: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC\-32bit\-1\.3\.7\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC-32bit-1.3.7.1000-1.i686.deb)

   + Debian 64\-bit \.rpm: [https://s3\.amazonaws\.com/redshift\-downloads/drivers/AmazonRedshiftODBC\-64bit\-1\.3\.7\.1000\-1\.x86\_64\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/AmazonRedshiftODBC-64bit-1.3.7.1000-1.x86_64.deb) 

   The name for both of these drivers is Amazon Redshift ODBC Driver\.
**Note**  
Download the package that corresponds to the system architecture of your SQL client tool or application\. For example, if your client tool is 64\-bit, install a 64\-bit driver\.

    Then download and review the [Amazon Redshift ODBC Driver License Agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+Driver+License+Agreement.pdf)\. If you need to distribute these drivers to your customers or other third parties, please email *redshift\-pm@amazon\.com* to arrange an appropriate license\. 

1. Navigate to the location where you downloaded the package, and then run one of the following commands\. Use the command that corresponds to your Linux distribution\. 

   + On RHEL 5\.0/6\.0 and CentOS 5\.0/6\.0 operating systems, run this command:

     ```
     yum --nogpgcheck localinstall RPMFileName
     ```

     Replace *RPMFileName* with the RPM package file name\. For example, the following command demonstrates installing the 32\-bit driver:

     ```
     yum --nogpgcheck localinstall AmazonRedshiftODBC-32bit-1.x.x.xxxx-x.x86_64.deb
     ```

   + On SLES 11, run this command:

     ```
     zypper install RPMFileName
     ```

     Replace *RPMFileName* with the RPM package file name\. For example, the following command demonstrates installing the 64\-bit driver:

     ```
     zypper install AmazonRedshiftODBC-1.x.x.xxxx-x.x86_64.rpm
     ```

   + On Debian 7, run this command:

     ```
     sudo apt install ./DEBFileName.deb
     ```

     Replace *DEBFileName\.deb* with the Debian package file name\. For example, the following command demonstrates installing the 64\-bit driver:

     ```
     sudo apt install ./AmazonRedshiftODBC-1.x.x.xxxx-x.x86_64.deb
     ```

**Important**  
When you have finished installing the drivers, configure them for use on your system\. For more information on driver configuration, see [Configure the ODBC Driver on Linux and Mac OS X Operating Systems](odbc-driver-configure-linux-mac.md)\.