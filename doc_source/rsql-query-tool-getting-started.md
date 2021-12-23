# Getting started with Amazon Redshift RSQL<a name="rsql-query-tool-getting-started"></a>

Install Amazon Redshift RSQL on a computer with a Linux, macOS, or Microsoft Windows operating system\.

## Prerequisites<a name="rsql-query-tool-prerequisites"></a>

*Linux:*

1. Install the driver manager with the following command:

   ```
   sudo yum install unixODBC
   ```

1. Install the ODBC driver: [Installing the Amazon Redshift driver on Linux operating systems](configure-odbc-connection.md#odbc-driver-linux-how-to-install)\.

1. Copy the ini file to your home directory:

   ```
   cp /opt/amazon/redshiftodbc/Setup/odbc.ini ~/.odbc.ini
   ```

1. Set the environment variables to point to the location of the file:

   ```
   export ODBCINI=~/.odbc.ini
   export ODBCSYSINI=/opt/amazon/redshiftodbc/Setup
   export AMAZONREDSHIFTODBCINI=/opt/amazon/redshiftodbc/lib/64/amazon.redshiftodbc.ini
   ```

    For more information about configuring environment variables, see [Configuring environment variables](configure-odbc-connection.md#rs-mgmt-config-global-env-variables)\. 

*Mac OSX:*

1. Install the driver manager with the following command:

   ```
   brew install unixodbc openssl --build-from-source
   ```

1. Install the ODBC driver: [Install the Amazon Redshift ODBC driver on macOS X](configure-odbc-connection.md#install-odbc-driver-mac)\.

1. Copy the ini file to your home directory:

   ```
   cp /opt/amazon/redshift/Setup/odbc.ini ~/.odbc.ini
   ```

1. Set the environment variables to point to the location of the file:

   ```
   export ODBCINI=~/.odbc.ini
   export ODBCSYSINI=/opt/amazon/redshift/Setup
   export AMAZONREDSHIFTODBCINI=/opt/amazon/redshift/lib/amazon.redshiftodbc.ini
   ```

    For more information about configuring environment variables, see [Configuring environment variables](configure-odbc-connection.md#rs-mgmt-config-global-env-variables)\. 

1. Set `DYLD_LIBRARY_PATH` to location of your libodbc\.dylib if its not in `/usr/local/lib`\.

   ```
   export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/lib
   ```

*Windows:*

Follow the instructions at [Install and configure the Amazon Redshift ODBC driver on Microsoft Windows](configure-odbc-connection.md#install-odbc-driver-windows) to install the driver\. Windows doesn't require a driver manager\. OpenSSL is required for Amazon Redshift RSQL on Windows\.

## Download RSQL<a name="rsql-query-tool-download"></a>
+ Linux rpm file: [64\-bit RSQL Version 1\.0\.1 rpm file](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1-1.x86_64.rpm)\.
+ Mac OS dmg file: [64\-bit RSQL Version 1\.0\.1 dmg file](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.dmg)\.
+ Windows msi file: [64\-bit RSQL Version 1\.0\.1 msi file](https://s3.amazonaws.com/redshift-downloads/amazon-redshift-rsql/1.0.1/AmazonRedshiftRsql-1.0.1.msi)\.

## Install RSQL<a name="rsql-query-tool-installation"></a>

Perform the steps for your operating system\.
+ *Linux:*

  ```
  sudo rpm -i AmazonRedshiftRsql-<version>-1.x86_64.rpm
  ```
+ *Mac OS:*

  1. Double\-click the dmg file to mount the disk image\.

  1. Double\-click the pkg file to run the installer\.

  1. Follow the steps in the installer to complete the installation\. Agree to the terms of the license agreement\.
+ *Windows:*

  1. Double\-click the file to run the installer\.

  1. Follow the prompts to complete the installation\.