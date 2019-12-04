# Configure the ODBC Driver on Linux and Mac OS X Operating Systems<a name="odbc-driver-configure-linux-mac"></a>

On Linux and Mac OS X operating systems, you use an ODBC driver manager to configure the ODBC connection settings\. ODBC driver managers use configuration files to define and configure ODBC data sources and drivers\. The ODBC driver manager that you use depends on the operating system that you use\. For more information about the supported ODBC driver managers to configure the Amazon Redshift ODBC drivers, see [System Requirements](install-odbc-driver-linux.md#odbc-driver-sysreq-linux) for Linux operating systems and [System Requirements](install-odbc-driver-mac.md#odbc-driver-sysreq-mac) for Mac OS X operating systems\.

Three files are required for configuring the Amazon Redshift ODBC driver: `amazon.redshiftodbc.ini`, `odbc.ini`, and `odbcinst.ini`\.

If you installed to the default location, the `amazon.redshiftodbc.ini` configuration file is located in one of the following directories:
+ /opt/amazon/redshiftodbc/lib/32 \(for the 32\-bit driver on Linux operating systems\)
+ /opt/amazon/redshiftodbc/lib/64 \(for the 64\-bit driver on Linux operating systems\)
+ /opt/amazon/redshift/lib \(for the driver on Mac OS X\)

Additionally, under /opt/amazon/redshiftodbc/Setup on Linux or /opt/amazon/redshift/Setup on Mac OS X, there are sample `odbc.ini` and `odbcinst.ini` files for you to use as examples for configuring the Amazon Redshift ODBC driver and the data source name \(DSN\)\.

We don't recommend using the Amazon Redshift ODBC driver installation directory for the configuration files\. The sample files in the Setup directory are for example purposes only\. If you reinstall the Amazon Redshift ODBC driver at a later time, or upgrade to a newer version, the installation directory is overwritten and you'll lose any changes you might have made to those files\.

To avoid this, you should copy the amazon\.redshiftodbc\.ini file to a directory other than the installation directory\. If you copy this file to the user's home directory, add a period \(\.\) to the beginning of the file name to make it a hidden file\.

For the odbc\.ini and odbcinst\.ini files, you should either use the configuration files in the user's home directory or create new versions in another directory\. By default, your Linux or Mac OS X operating system should have an \.odbc\.ini file and an \.odbcinst\.ini file in the user's home directory \(/home/$USER or \~/\.\)\. These default files are hidden files, which are indicated by the dot \(\.\) in front of the file name, and they will only display when you use the \-a flag to list the directory contents\.

Whichever option you choose for the odbc\.ini and odbcinst\.ini files, you will need to modify them to add driver and DSN configuration information\. If you chose to create new files, you also need to set environment variables to specify where these configuration files are located\. 

## Configuring the odbc\.ini File<a name="configure-odbc-ini-file"></a>

 You use the odbc\.ini file to define data source names \(DSNs\)\. 

 Use the following format on Linux operating systems:

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

The following example shows the configuration for odbc\.ini on Linux operating systems:

```
[ODBC Data Sources]
Amazon_Redshift_x32=Amazon Redshift (x86)
Amazon_Redshift_x64=Amazon Redshift (x64)

[Amazon Redshift (x86)]
Driver=/opt/amazon/redshiftodbc/lib/32/lib/amazonredshiftodbc32.so
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US

[Amazon Redshift (x64)]
Driver=/opt/amazon/redshiftodbc/lib/64/lib/amazonredshiftodbc64.so
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US
```

Use the following format on Mac OS X operating systems: 

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

 The following example shows the configuration for odbc\.ini on Mac OS X operating systems: 

```
[ODBC Data Sources]
Amazon_Redshift_dylib=Amazon Redshift DSN for Mac OS X

[Amazon Redshift DSN for Mac OS X]
Driver=/opt/amazon/redshift/lib/amazonredshiftodbc.dylib
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932
Database=dev
locale=en-US
```

## Configuring the odbcinst\.ini File<a name="configure-odbcinst-ini-file"></a>

You use the odbcinst\.ini file to define ODBC drivers\.

Use the following format on Linux operating systems:

```
[ODBC Drivers]
driver_name=Installed
...
                            
[driver_name]
Description=driver_description
Driver=path/driver_file
    
...
```

The following example shows the odbcinst\.ini configuration for both the 32\-bit and 64\-bit drivers installed in the default directories on Linux operating systems:

```
[ODBC Drivers]
Amazon Redshift (x86)=Installed
Amazon Redshift (x64)=Installed

[Amazon Redshift (x86)]
Description=Amazon Redshift ODBC Driver (32-bit)
Driver=/opt/amazon/redshiftodbc/lib/32/lib/amazonredshiftodbc32.so

[Amazon Redshift (x64)]
Description=Amazon Redshift ODBC Driver (64-bit)
Driver=/opt/amazon/redshiftodbc/lib/64/lib/amazonredshiftodbc64.so
```

Use the following format on Mac OS X operating systems:

```
[ODBC Drivers]
driver_name=Installed
...
                            
[driver_name]
Description=driver_description
Driver=path/lib/amazonredshiftodbc.dylib
    
...
```

The following example shows the odbcinst\.ini configuration for the driver installed in the default directory on Mac OS X operating systems: 

```
[ODBC Drivers]
Amazon RedshiftODBC DSN=Installed

[Amazon RedshiftODBC DSN]
Description=Amazon Redshift ODBC Driver for Mac OS X
Driver=/opt/amazon/redshift/lib/amazonredshiftodbc.dylib
```

## Configuring Environment Variables for Driver Configuration Files<a name="rs-mgmt-config-global-env-variables"></a>

In order for the Amazon Redshift ODBC driver to function properly, you need to set a number of environmental variables, as described following\.

Set an environment variable to specify the path to the driver manager libraries:
+ On Linux, set `LD_LIBRARY_PATH` to point to the directory containing the driver manager libraries\. For more information on supported driver managers, see [Install the Amazon Redshift ODBC Driver on Linux Operating Systems](install-odbc-driver-linux.md)\.
+ On Mac OS X, set `DYLD_LIBRARY_PATH` to point to the directory containing the driver manager libraries\. For more information on supported driver managers, see [Install the Amazon Redshift ODBC Driver on Mac OS X](install-odbc-driver-mac.md)\.

Optionally, set `AMAZONREDSHIFTODBCINI` to point to your amazon\.redshiftodbc\.ini file\. `AMAZONREDSHIFTODBCINI` must specify the full path, including the file name\. You must either set this variable, or place this file in a location where the system will find it in a search\. The following search order is used to locate the amazon\.redshiftodbc\.ini file: 

1. If the `AMAZONREDSHIFTODBCINI` environment variable is defined, then the driver searches for the file specified by the environment variable\.

1. If the `AMAZONREDSHIFTODBCINI` environment variable is not defined, then the driver searches in its own directory—that is, the directory that contains the driver binary\.

1. If the amazon\.redshiftodbc\.ini file cannot be found, the driver tries to automatically determine the driver manager settings and connect\. However, error messages won't display correctly in this case\.

If you decide to use a directory other than the user's home directory for the odbc\.ini and odbcinst\.ini files, you also need to set environment variables to specify where the configuration files appear: 
+ Set `ODBCINI` to point to your odbc\.ini file\.
+ Set `ODBCSYSINI` to point to the directory containing the odbcinst\.ini file\.

On Linux, you can set the environment variables as shown in the following example if these are true: 
+ Your driver manager libraries are located in the /usr/local/lib directory\.
+ Your odbc\.ini and amazon\.redshiftodbc\.ini files are located in the /etc directory\.
+ Your odbcinst\.ini file is located in the /usr/local/odbc directory\.

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
export ODBCINI=/etc/odbc.ini
export AMAZONREDSHIFTODBCINI=/etc/amazon.redshiftodbc.ini 
export ODBCSYSINI=/usr/local/odbc
```

On macOS X, you can set the environment variables as shown in the following example if these are true:
+ Your driver manager libraries are located in the /usr/local/lib directory\.
+ Your odbc\.ini and amazon\.redshiftodbc\.ini files are located in the /etc directory\.
+ Your odbcinst\.ini file is located in the /usr/local/odbc directory\.

```
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/usr/local/lib
export ODBCINI=/etc/odbc.ini
export AMAZONREDSHIFTODBCINI=/etc/amazon.redshiftodbc.ini 
export ODBCSYSINI=/usr/local/odbc
```