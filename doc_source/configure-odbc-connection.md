# Configuring an ODBC connection<a name="configure-odbc-connection"></a>

You can use an ODBC connection to connect to your Amazon Redshift cluster from many third\-party SQL client tools and applications\. To do this, set up the connection on your client computer or Amazon EC2 instance\. If your client tool supports JDBC, you might choose to use that type of connection rather than ODBC due to the ease of configuration that JDBC provides\. However, if your client tool doesn't support JDBC, follow the steps in this section to configure an ODBC connection\. 

Amazon Redshift provides ODBC drivers for Linux, Windows, and macOS X operating systems\. Before you install an ODBC driver, determine whether your SQL client tool is 32\-bit or 64\-bit\. Install the ODBC driver that matches the requirements of your SQL client tool\. Otherwise, the connection doesn't work\. If you use more than one SQL client tool on the same computer or instance, make sure that you download the appropriate drivers\. You might need to install both the 32\-bit and the 64\-bit drivers if the tools differ in their system architecture\. 

For the latest information about ODBC driver functionality and prerequisites, see [Amazon Redshift ODBC driver release notes](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Release+Notes.pdf)\. 

For installation and configuration information for Amazon Redshift ODBC drivers, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

If you want to use an ODBC connection, take the following steps\.

**Topics**
+ [Obtain the ODBC URL for your cluster](#obtain-odbc-url)
+ [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](#install-odbc-driver-windows)
+ [Install the Amazon Redshift ODBC driver on Linux](#install-odbc-driver-linux)
+ [Install the Amazon Redshift ODBC driver on macOS X](#install-odbc-driver-mac)
+ [Configure the ODBC driver on Linux and macOS X operating systems](#odbc-driver-configure-linux-mac)
+ [Configure ODBC driver options](#configure-odbc-options)
+ [Use previous ODBC driver versions in certain cases](#odbc-previous-versions)

## Obtain the ODBC URL for your cluster<a name="obtain-odbc-url"></a>

Amazon Redshift displays the ODBC URL for your cluster in the Amazon Redshift console\. This URL contains the information to set up the connection between your client computer and the database\.

 An ODBC URL has the following format: `Driver={driver};Server=endpoint;Database=database_name;UID=user_name;PWD=password;Port=port_number` 

The fields of the format shown preceding have the following values\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-odbc-connection.html)

The following is an example ODBC URL: `Driver={Amazon Redshift (x64)}; Server=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com; Database=dev; UID=masteruser; PWD=insert_your_master_user_password_here; Port=5439` 

For information about how to get your ODBC connection, see [Finding your cluster connection string](configuring-connections.md#connecting-connection-string)\. 

## Install and configure the Amazon Redshift ODBC driver on Microsoft Windows<a name="install-odbc-driver-windows"></a>

### System requirements<a name="odbc-driver-sysreq-windows"></a>

You install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet a list of minimum system requirements\. For information about minimum system requirements, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

### Installing the Amazon Redshift driver on Windows operating systems<a name="odbc-driver-windows-how-to-install"></a>

Use the following procedure to download the Amazon Redshift ODBC drivers for Windows operating systems\. Only use a driver other than these if you're running a third\-party application that is certified for use with Amazon Redshift and that requires a specific driver\. 

**To install the ODBC driver**

1. Download one of the following, depending on the system architecture of your SQL client tool or application:
   + [32\-bit ODBC driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC32-1.4.16.1000.msi) 

     The name for this driver is Amazon Redshift \(x86\)\.
   + [64\-bit ODBC driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC64-1.4.16.1000.msi) 

     The name for this driver is Amazon Redshift \(x64\)\.
**Note**  
Download the MSI package that corresponds to the system architecture of your SQL client tool or application\. For example, if your SQL client tool is 64\-bit, install the 64\-bit driver\.

    Then download and review the [Amazon Redshift ODBC and JDBC driver license agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+and+JDBC+Driver+License+Agreement.pdf)\.  

1.  Double\-click the \.msi file, and then follow the steps in the wizard to install the driver\. 

### Creating a system DSN entry for an ODBC connection on Microsoft Windows<a name="create-dsn-odbc-windows"></a>

After you download and install the ODBC driver, add a data source name \(DSN\) entry to the client computer or Amazon EC2 instance\. SQL client tools use this data source to connect to the Amazon Redshift database\. 

We recommend that you create a system DSN instead of a user DSN\. Some applications load the data using a different user account\. These applications might not be able to detect user DSNs that are created under another user account\.

**Note**  
For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, additional steps are required\. For more information, see [Configure a JDBC or ODBC connection to use IAM credentials](generating-iam-credentials-configure-jdbc-odbc.md)\.

For information about how to create a system DSN entry, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

**To create a system DSN entry for an ODBC connection on Windows**

1. In the **Start** menu, open **ODBC Data Sources**\.

   Make sure that you choose the ODBC Data Source Administrator that has the same bitness as the client application that you are using to connect to Amazon Redshift\.

1. In the **ODBC Data Source Administrator**, choose the **Driver** tab and locate the driver folder\.
**Note**  
If you installed the 32\-bit driver, the folder is named **Amazon Redshift ODBC Driver \(32\-bit\)**\. If you installed the 64\-bit driver, the folder is named **Amazon Redshift ODBC Driver \(64\-bit\)**\. If you installed both drivers, you have a folder for each driver\. 

1.  Choose the **System DSN** tab to configure the driver for all users on the computer, or the **User DSN** tab to configure the driver for your user account only\. 

1.  Choose **Add**\. The **Create New Data Source** window opens\. 

1.  Choose the **Amazon Redshift** ODBC driver, and then choose **Finish**\. The ** Amazon Redshift ODBC Driver DSN Setup** window opens\.

1. Under **Connection Settings**, enter the following information:
<a name="rs-mgmt-dsn"></a>
**Data source name**  
Enter a name for the data source\. You can use any name that you want to identify the data source later when you create the connection to the cluster\. For example, if you followed the *Amazon Redshift Getting Started*, you might type `exampleclusterdsn` to make it easy to remember the cluster that you associate with this DSN\.
<a name="rs-mgmt-server"></a>
**Server**  
Specify the endpoint for your Amazon Redshift cluster\. You can find this information in the Amazon Redshift console on the cluster's details page\. For more information, see [Configuring connections in Amazon Redshift](configuring-connections.md)\.
<a name="rs-mgmt-port"></a>
**Port**  
Enter the port number that the database uses\. By default, Amazon Redshift uses 5439, but use the port that the cluster was configured to use when it was launched\.
<a name="rs-mgmt-database"></a>
**Database**  
Enter the name of the Amazon Redshift database\. If you launched your cluster without specifying a database name, enter `dev`\. Otherwise, use the name that you chose during the launch process\. If you followed the *Amazon Redshift Getting Started*, enter `dev`\.

1. Under **Authentication**, specify the configuration options to configure standard or IAM authentication\. For information about different authentication options, see "Configuring Authentication on Windows" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

1. Under **SSL Settings**, specify a value for the following:
<a name="rs-mgmt-ssl-authentication"></a>
**SSL authentication**  
Choose a mode for handling Secure Sockets Layer \(SSL\)\. In a test environment, you might use `prefer`\. However, for production environments and when secure data exchange is required, use `verify-ca` or `verify-full`\. For more information about using SSL on Windows, see "Configuring SSL Verification on Windows" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

1. Under **Additional Options**, specify options on how to return query results to your SQL client tool or application\. For more information, see "Configuring Additional Options on Windows" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

1. In **Logging Options**, specify values for the logging option\. For more information, see "Configuring Logging Options on Windows" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

   Then choose **OK**\.

1. Under **Data Type Options**, specify values for data types\. For more information, see "Configuring Data Type Options on Windows" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

   Then choose **OK**\.

1. Choose **Test**\. If the client computer can connect to the Amazon Redshift database, you see the following message: **Connection successful**\. 

    If the client computer fails to connect to the database, you can troubleshoot possible issues\. For more information, see [Troubleshooting connection issues in Amazon Redshift](troubleshooting-connections.md)\. 

1. Configure TCP keepalives on Windows to prevent connections from timing out\. For information about how to configure TCP keepalives on Windows, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\.

1. To help troubleshooting, configure logging\. For information about how to configure logging on Windows, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

## Install the Amazon Redshift ODBC driver on Linux<a name="install-odbc-driver-linux"></a>

### System requirements<a name="odbc-driver-sysreq-linux"></a>

You install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet a list of minimum system requirements\. For information about minimum system requirements, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

### Installing the Amazon Redshift driver on Linux operating systems<a name="odbc-driver-linux-how-to-install"></a>

Use the steps in this section to download and install the Amazon Redshift ODBC drivers on a supported Linux distribution\. The installation process installs the driver files in the following directories: 
+ `/opt/amazon/redshiftodbc/lib/32` \(for a 32\-bit driver\)
+ `/opt/amazon/redshiftodbc/lib/64` \(for a 64\-bit driver\)
+ `/opt/amazon/redshiftodbc/ErrorMessages`
+ `/opt/amazon/redshiftodbc/Setup`<a name="rs-mgmt-install-odbc-drivers-linux"></a>

**To install the Amazon Redshift ODBC driver**

1. Download one of the following, depending on the system architecture of your SQL client tool or application: 
   + [32\-bit RPM driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-32-bit-1.4.16.1000-1.i686.rpm) 
   + [64\-bit RPM driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-64-bit-1.4.16.1000-1.x86_64.rpm) 
   + [32\-bit Debian driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-32-bit-1.4.16.1000-1.i686.deb) 
   + [64\-bit Debian driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-64-bit-1.4.16.1000-1.x86_64.deb) 

   The name for each of these drivers is Amazon Redshift ODBC driver\.
**Note**  
Download the package that corresponds to the system architecture of your SQL client tool or application\. For example, if your client tool is 64\-bit, install a 64\-bit driver\.

    Then download and review the [Amazon Redshift ODBC and JDBC driver license agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+and+JDBC+Driver+License+Agreement.pdf)\.  

1. Go to the location where you downloaded the package, and then run one of the following commands\. Use the command that corresponds to your Linux distribution\. 
   + On RHEL  and CentOS  operating systems, run the following command\.

     ```
     yum --nogpgcheck localinstall RPMFileName
     ```

     Replace *`RPMFileName`* with the RPM package file name\. For example, the following command demonstrates installing the 32\-bit driver\.

     ```
     yum --nogpgcheck localinstall AmazonRedshiftODBC-32bit-1.x.x.xxxx-x.x86_64.deb
     ```
   + On SLES, run the following command\.

     ```
     zypper install RPMFileName
     ```

     Replace *`RPMFileName`* with the RPM package file name\. For example, the following command demonstrates installing the 64\-bit driver\.

     ```
     zypper install AmazonRedshiftODBC-1.x.x.xxxx-x.x86_64.rpm
     ```
   + On Debian, run the following command\.

     ```
     sudo apt install ./DEBFileName.deb
     ```

     Replace `DEBFileName.deb` with the Debian package file name\. For example, the following command demonstrates installing the 64\-bit driver\.

     ```
     sudo apt install ./AmazonRedshiftODBC-1.x.x.xxxx-x.x86_64.deb
     ```

**Important**  
When you have finished installing the drivers, configure them for use on your system\. For more information on driver configuration, see [Configure the ODBC driver on Linux and macOS X operating systems](#odbc-driver-configure-linux-mac)\.

## Install the Amazon Redshift ODBC driver on macOS X<a name="install-odbc-driver-mac"></a>

### System requirements<a name="odbc-driver-sysreq-mac"></a>

You install the driver on client computers accessing an Amazon Redshift data warehouse\. Each computer where you install the driver must meet a list of minimum system requirements\. For information about minimum system requirements, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

### Installing the Amazon Redshift ODBC driver on macOS X<a name="odbc-driver-mac-how-to-install"></a>

Use the steps in this section to download and install the Amazon Redshift ODBC driver on a supported version of macOS X\. The installation process installs the driver files in the following directories: 
+ `/opt/amazon/redshift/lib/universal`
+ `/opt/amazon/redshift/ErrorMessages`
+ `/opt/amazon/redshift/Setup`<a name="rs-mgmt-install-odbc-drivers-mac"></a>

**To install the Amazon Redshift ODBC driver on macOS X**

1. Download the [macOS X driver version 1\.4\.16](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-1.4.16.1000.dmg)\. The name for this driver is Amazon Redshift ODBC driver\. 
**Important**  
After certificate rotation on September 4, 2020, ODBC Driver version 1\.4\.8\.1000 or earlier on macOS will not be able to establish connections to Amazon Redshift clusters\. For more information, see [Driver update required for Amazon Redshift ODBC Driver earlier than 1\.4\.10 on Apple macOS](https://forums.aws.amazon.com/ann.jspa?annID=7735)\. 

   Then download and review the [Amazon Redshift ODBC and JDBC driver license agreement](https://s3.amazonaws.com/redshift-downloads/drivers/Amazon+Redshift+ODBC+and+JDBC+Driver+License+Agreement.pdf)\.  

1. Double\-click **AmazonRedshiftODBC\.dmg** to mount the disk image\.

1. Double\-click **AmazonRedshiftODBC\.pkg** to run the installer\.

1. Follow the steps in the installer to complete the driver installation process\. To perform the installation, agree to the terms of the license agreement\.

**Important**  
When you have finished installing the driver, configure it for use on your system\. For more information on driver configuration, see [Configure the ODBC driver on Linux and macOS X operating systems](#odbc-driver-configure-linux-mac)\.

## Configure the ODBC driver on Linux and macOS X operating systems<a name="odbc-driver-configure-linux-mac"></a>

On Linux and macOS X operating systems, you use an ODBC driver manager to configure the ODBC connection settings\. ODBC driver managers use configuration files to define and configure ODBC data sources and drivers\. The ODBC driver manager that you use depends on the operating system that you use\. For more information about the supported ODBC driver managers to configure the Amazon Redshift ODBC drivers, see [System requirements](#odbc-driver-sysreq-linux) for Linux operating systems and [System requirements](#odbc-driver-sysreq-mac) for macOS X operating systems\.

Three files are required for configuring the Amazon Redshift ODBC driver: `amazon.redshiftodbc.ini`, `odbc.ini`, and `odbcinst.ini`\.

If you installed to the default location, the `amazon.redshiftodbc.ini` configuration file is located in one of the following directories:
+ `/opt/amazon/redshiftodbc/lib/32` \(for the 32\-bit driver on Linux operating systems\)
+ `/opt/amazon/redshiftodbc/lib/64` \(for the 64\-bit driver on Linux operating systems\)
+ `/opt/amazon/redshift/lib` \(for the driver on macOS X\)

Additionally, under `/opt/amazon/redshiftodbc/Setup` on Linux or /opt/amazon/redshift/Setup on macOS X, there are sample `odbc.ini` and `odbcinst.ini` files\. You can use these files as examples for configuring the Amazon Redshift ODBC driver and the data source name \(DSN\)\.

We don't recommend using the Amazon Redshift ODBC driver installation directory for the configuration files\. The sample files in the `Setup` directory are for example purposes only\. If you reinstall the Amazon Redshift ODBC driver at a later time, or upgrade to a newer version, the installation directory is overwritten\. You then lose any changes that you might have made to those files\.

To avoid this, copy the `amazon.redshiftodbc.ini` file to a directory other than the installation directory\. If you copy this file to the user's home directory, add a period \(\.\) to the beginning of the file name to make it a hidden file\.

For the `odbc.ini` and `odbcinst.ini` files, either use the configuration files in the user's home directory or create new versions in another directory\. By default, your Linux or macOS X operating system should have an `odbc.ini` file and an `odbcinst.ini` file in the user's home directory \(`/home/$USER` or `~/`\.\)\. These default files are hidden files, which is indicated by the dot \(\.\) in front of each file name\. These files display only when you use the `-a` flag to list the directory contents\.

Whichever option you choose for the `odbc.ini` and `odbcinst.ini` files, modify the files to add driver and DSN configuration information\. If you create new files, you also need to set environment variables to specify where these configuration files are located\. 

By default, ODBC driver managers are configured to use hidden versions of the `odbc.ini` and `odbcinst.ini` configuration files \(named \.`odbc.ini` and \.`odbcinst.ini`\) located in the home directory\. They also are configured to use the `amazon.redshiftodbc.ini` file in the `/lib` subfolder of the driver installation directory\. If you store these configuration files elsewhere, set the environment variables described following so that the driver manager can locate the files\. For more information, see "Specifying the Locations of the Driver Configuration Files" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

### Creating a data source name on Linux and macOS X operating systems<a name="configure-odbc-ini-file"></a>

 When connecting to your data store using a data source name \(DSN\), configure the `odbc.ini` file to define DSNs\. Set the properties in the `odbc.ini` file to create a DSN that specifies the connection information for your data store\.

For information about how to configure the `odbcini` file, see "Creating a Data Source Name on a Non\-Windows Machine" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

 Use the following format on Linux operating systems\.

```
[ODBC Data Sources]
driver_name=dsn_name

[dsn_name]
Driver=path/driver_file

Host=cluster_endpoint
Port=port_number
Database=database_name
locale=locale
```

The following example shows the configuration for odbc\.ini on Linux operating systems\.

```
[ODBC Data Sources]
Amazon_Redshift_x32=Amazon Redshift (x86)
Amazon_Redshift_x64=Amazon Redshift (x64)

[Amazon Redshift (x86)]
Driver=/opt/amazon/redshiftodbc/lib/32/libamazonredshiftodbc32.so
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US

[Amazon Redshift (x64)]
Driver=/opt/amazon/redshiftodbc/lib/64/libamazonredshiftodbc64.so
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US
```

Use the following format on macOS X operating systems\.

```
[ODBC Data Sources]
driver_name=dsn_name

[dsn_name]
Driver=path/lib/amazonredshiftodbc.dylib

Host=cluster_endpoint
Port=port_number
Database=database_name
locale=locale
```

 The following example shows the configuration for `odbc.ini` on macOS X operating systems\.

```
[ODBC Data Sources]
Amazon_Redshift_dylib=Amazon Redshift DSN for macOS X

[Amazon Redshift DSN for macOS X]
Driver=/opt/amazon/redshift/lib/amazonredshiftodbc.dylib
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US
```

### Configuring a connection without a DSN on Linux and macOS X operating systems<a name="configure-odbcinst-ini-file"></a>

To connect to your data store through a connection that doesn't have a DSN, define the driver in the `odbcinst.ini` file\. Then provide a DSN\-less connection string in your application\.

For information about how to configure the `odbcinst.ini` file in this case, see "Configuring a DSN\-less Connection on a Non\-Windows Machine" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

Use the following format on Linux operating systems\.

```
[ODBC Drivers]
driver_name=Installed
...
                            
[driver_name]
Description=driver_description
Driver=path/driver_file
    
...
```

The following example shows the `odbcinst.ini` configuration for both the 32\-bit and 64\-bit drivers installed in the default directories on Linux operating systems\.

```
[ODBC Drivers]
Amazon Redshift (x86)=Installed
Amazon Redshift (x64)=Installed

[Amazon Redshift (x86)]
Description=Amazon Redshift ODBC Driver (32-bit)
Driver=/opt/amazon/redshiftodbc/lib/32/libamazonredshiftodbc32.so

[Amazon Redshift (x64)]
Description=Amazon Redshift ODBC Driver (64-bit)
Driver=/opt/amazon/redshiftodbc/lib/64/libamazonredshiftodbc64.so
```

Use the following format on macOS X operating systems\.

```
[ODBC Drivers]
driver_name=Installed
...
                            
[driver_name]
Description=driver_description
Driver=path/lib/amazonredshiftodbc.dylib
    
...
```

The following example shows the `odbcinst.ini` configuration for the driver installed in the default directory on macOS X operating systems\.

```
[ODBC Drivers]
Amazon RedshiftODBC DSN=Installed

[Amazon RedshiftODBC DSN]
Description=Amazon Redshift ODBC Driver for macOS X
Driver=/opt/amazon/redshift/lib/amazonredshiftodbc.dylib
```

### Configuring environment variables<a name="rs-mgmt-config-global-env-variables"></a>

Use the correct ODBC driver manager to load the correct driver\. To do this, set the library path environment variable\. For more information, see "Specifying ODBC Driver Managers on Non\-Windows Machines" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

By default, ODBC driver managers are configured to use hidden versions of the `odbc.ini` and `odbcinst.ini` configuration files \(named \.`odbc.ini` and \.`odbcinst.ini`\) located in the home directory\. They also are configured to use the `amazon.redshiftodbc.ini` file in the `/lib` subfolder of the driver installation directory\. If you store these configuration files elsewhere, the environment variables so that the driver manager can locate the files\. For more information, see "Specifying the Locations of the Driver Configuration Files" in [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

### Configuring connection features<a name="connection-config-features"></a>

You can configure the following connection features for your ODBC setting:
+ Configure the ODBC driver to provide credentials and authenticate the connection to the Amazon Redshift database\.
+ Configure the ODBC driver to connect to a socket enabled with Secure Sockets Layer \(SSL\), if you are connecting to an Amazon Redshift server that has SSL enabled\.
+ Configure the ODBC driver to connect to Amazon Redshift through a proxy server\.
+ Configure the ODBC driver to use a query processing mode to prevent queries from consuming too much memory\.
+ Configure the ODBC driver to pass IAM authentication processes through a proxy server\.
+ Configure the ODBC driver to use TCP keepalives to prevent connections from timing out\.

For information about these connection features, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

## Configure ODBC driver options<a name="configure-odbc-options"></a>

You can use configuration options to control the behavior of the Amazon Redshift ODBC driver\.

In Microsoft Windows, you typically set driver options when you configure a data source name \(DSN\)\. You can also set driver options in the connection string when you connect programmatically, or by adding or changing registry keys in `HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBC.INI\your_DSN`\. For more information about configuring a DSN, see [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](#install-odbc-driver-windows)\. For an example of setting driver options in a connection string, see [Connect to your cluster programmatically](connecting-in-code.md)\.

In Linux and macOS X, you set driver configuration options in your `odbc.ini` and `amazon.redshiftodbc.ini` files, as described in [Configure the ODBC driver on Linux and macOS X operating systems](#odbc-driver-configure-linux-mac)\. Configuration options set in an `amazon.redshiftodbc.ini` file apply to all connections\. In contrast, configuration options set in an `odbc.ini` file are specific to a connection\. Configuration options set in `odbc.ini` take precedence over configuration options set in `amazon.redshiftodbc.ini`\.

For information about how to set up ODBC driver configuration options, see [Amazon Redshift ODBC driver installation and configuration guide](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/Amazon+Redshift+ODBC+Driver+Install+Guide.pdf)\. 

## Use previous ODBC driver versions in certain cases<a name="odbc-previous-versions"></a>

Download a previous version of the Amazon Redshift ODBC driver only if your tool requires a specific version of the driver\. 

For authentication using AWS Identity and Access Management \(IAM\) credentials or identity provider \(IdP\) credentials, use Amazon Redshift ODBC driver version 1\.3\.6\.1000 or later\.

**Important**  
Amazon Redshift has changed the way that SSL certificates are managed\. If you must use a driver version earlier than 1\.3\.7\.1000, you might need to update your current trust root CA certificates to continue to connect to your clusters using SSL\. For more information, see [Transitioning to ACM certificates for SSL connections](connecting-transitioning-to-acm-certs.md)\.

### Use previous ODBC driver versions for Windows<a name="odbc-previous-versions-windows"></a>

The following are the 32\-bit drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC32\-1\.4\.16\.1000\.msi ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC32-1.4.16.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC32\-1\.4\.14\.1000\.msi ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC32-1.4.14.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC32\-1\.4\.13\.1000\.msi ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC32-1.4.13.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC32\-1\.4\.11\.1000\.msi ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC32-1.4.11.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC32\-1\.4\.10\.1000\.msi ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC32-1.4.10.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC32\-1\.4\.8\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC32-1.4.8.1000.msi)\. 

The following are the 64\-bit drivers: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC64\-1\.4\.16\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC64-1.4.16.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC64\-1\.4\.14\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC64-1.4.14.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC64\-1\.4\.13\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC64-1.4.13.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC64\-1\.4\.11\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC64-1.4.11.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC64\-1\.4\.10\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC64-1.4.10.1000.msi)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC64\-1\.4\.8\.1000\.msi](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC64-1.4.8.1000.msi)\. 

### Use previous ODBC driver versions for Linux<a name="odbc-previous-versions-linux"></a>

The following are the versions of the 32\-bit driver: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.16\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-32-bit-1.4.16.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.16\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-32-bit-1.4.16.1000-1.i686.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.14\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC-32-bit-1.4.14.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.14\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC-32-bit-1.4.14.1000-1.i686.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.13\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC-32-bit-1.4.13.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.13\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC-32-bit-1.4.13.1000-1.i686.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.11\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC-32-bit-1.4.11.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.11\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC-32-bit-1.4.11.1000-1.i686.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.10\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC-32-bit-1.4.10.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.10\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC-32-bit-1.4.10.1000-1.i686.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.8\.1000\-1\.i686\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC-32-bit-1.4.8.1000-1.i686.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC\-32\-bit\-1\.4\.8\.1000\-1\.i686\.deb](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC-32-bit-1.4.8.1000-1.i686.deb)\. 

The following are the versions of the 64\-bit driver: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.16\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-64-bit-1.4.16.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.16\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-64-bit-1.4.16.1000-1.x86_64.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.14\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC-64-bit-1.4.14.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.14\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC-64-bit-1.4.14.1000-1.x86_64.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.13\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC-64-bit-1.4.13.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.13\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC-64-bit-1.4.13.1000-1.x86_64.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.11\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC-64-bit-1.4.11.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.11\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC-64-bit-1.4.11.1000-1.x86_64.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.10\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC-64-bit-1.4.10.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.10\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC-64-bit-1.4.10.1000-1.x86_64.deb)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.8\.1000\-1\.x86\_64\.rpm](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC-64-bit-1.4.8.1000-1.x86_64.rpm)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.8\.1000/AmazonRedshiftODBC\-64\-bit\-1\.4\.8\.1000\-1\.x86\_64\.deb ](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.8.1000/AmazonRedshiftODBC-64-bit-1.4.8.1000-1.x86_64.deb)\. 

### Use previous ODBC driver versions for macOS X<a name="odbc-previous-versions-mac"></a>

The following are the versions of the Amazon Redshift ODBC driver for macOS X: 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.16\.1000/AmazonRedshiftODBC\-1\.4\.16\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.16.1000/AmazonRedshiftODBC-1.4.16.1000.dmg)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.14\.1000/AmazonRedshiftODBC\-1\.4\.14\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.14.1000/AmazonRedshiftODBC-1.4.14.1000.dmg)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.13\.1000/AmazonRedshiftODBC\-1\.4\.13\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.13.1000/AmazonRedshiftODBC-1.4.13.1000.dmg)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.11\.1000/AmazonRedshiftODBC\-1\.4\.11\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.11.1000/AmazonRedshiftODBC-1.4.11.1000.dmg)\. 
+ [https://s3\.amazonaws\.com/redshift\-downloads/drivers/odbc/1\.4\.10\.1000/AmazonRedshiftODBC\-1\.4\.10\.1000\.dmg](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/1.4.10.1000/AmazonRedshiftODBC-1.4.10.1000.dmg)\. 

**Important**  
After certificate rotation on September 4, 2020, ODBC Driver version 1\.4\.8\.1000 or earlier on macOS will not be able to establish connections to Amazon Redshift clusters\. For more information, see [Driver update required for Amazon Redshift ODBC Driver earlier than 1\.4\.10 on Apple macOS](https://forums.aws.amazon.com/ann.jspa?annID=7735)\. 