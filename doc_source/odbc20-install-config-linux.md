# Installing and configuring the Amazon Redshift ODBC driver on Linux<a name="odbc20-install-config-linux"></a>

## System Requirements<a name="odbc20-system-requirements-linux"></a>

You must install the Amazon Redshift ODBC driver on client computers accessing an Amazon Redshift data warehouse\. For each computer where you install the driver, there are the following minimum requirements: 
+ Root access on the machine\.
+ One of the following distributions:
  + Red Hat® Enterprise Linux® \(RHEL\) 7 or 8
  + CentOS 7 or 8\.
+ 150 MB of available disk space\.
+ unixODBC 2\.2\.14 or later\.
+ glibc 2\.26 or later\.

## Installing the Amazon Redshift ODBC driver<a name="odbc20-install-linux"></a>

To download and install the Amazon Redshift ODBC driver version 2\.x for Linux:

1.  Download the following driver: [64\-bit RPM driver version 2\.0\.0\.5](https://s3.amazonaws.com/redshift-downloads/drivers/odbc/2.0.0.5/AmazonRedshiftODBC-64-bit-2.0.0.5.x86_64.rpm) 
**Note**  
32\-bit ODBC drivers are discontinued\. Further updates will not be released, except for urgent security patches\.

1.  Go to the location where you downloaded the package, and then run one of the following commands\. Use the command that corresponds to your Linux distribution\. 

   On RHEL and CentOS operating systems, run the following command:

   ```
   yum --nogpgcheck localinstall RPMFileName
   ```

   Replace `RPMFileName` with the RPM package file name\. For example, the following command demonstrates installing the 64\-bit driver:

   ```
   yum --nogpgcheck localinstall AmazonRedshiftODBC-64-bit-2.x.xx.xxxx.x86_64.rpm
   ```

## Using an ODBC driver manager to configure the ODBC driver on Linux<a name="odbc20-config-linux"></a>

On Linux, you use an ODBC driver manager to configure the ODBC connection settings\. ODBC driver managers use configuration files to define and configure ODBC data sources and drivers\. The ODBC driver manager that you use depends on the operating system that you use\.

### Configuring the ODBC driver using unixODBC driver manager<a name="odbc20-config-unixodbc-linux"></a>

The following files are required to configure the Amazon Redshift ODBC driver: 
+ ` amazon.redshiftodbc.ini `
+ ` odbc.ini `
+ ` odbcinst.ini `

 If you installed to the default location, the `amazon.redshiftodbc.ini` configuration file is located in `/opt/amazon/redshiftodbcx64`\.

 Additionally, under `/opt/amazon/redshiftodbcx64`, you can find sample `odbc.ini` and `odbcinst.ini` files\. You can use these files as examples for configuring the Amazon Redshift ODBC driver and the data source name \(DSN\)\.

 We don't recommend using the Amazon Redshift ODBC driver installation directory for the configuration files\. The sample files in the installed directory are for example purposes only\. If you reinstall the Amazon Redshift ODBC driver at a later time, or upgrade to a newer version, the installation directory is overwritten\. You will lose any changes that you might have made to files in the installation directory\.

 To avoid this, copy the `amazon.redshiftodbc.ini` file to a directory other than the installation directory\. If you copy this file to the user's home directory, add a period \(\.\) to the beginning of the file name to make it a hidden file\.

 For the `odbc.ini` and `odbcinst.ini` files, either use the configuration files in the user's home directory or create new versions in another directory\. By default, your Linux operating system should have an `odbc.ini` file and an `odbcinst.ini` file in the user's home directory \(`/home/$USER` or `~/.`\)\. These default files are hidden files, which is indicated by the dot \(\.\) in front of each file name\. These files display only when you use the `-a` flag to list the directory contents\.

 Whichever option you choose for the `odbc.ini` and `odbcinst.ini` files, modify the files to add driver and DSN configuration information\. If you create new files, you also need to set environment variables to specify where these configuration files are located\.

 By default, ODBC driver managers are configured to use hidden versions of the `odbc.ini` and `odbcinst.ini` configuration files \(named `.odbc.ini` and `.odbcinst.ini`\) located in the home directory\. They also are configured to use the `amazon.redshiftodbc.ini` file in the driver installation directory\. If you store these configuration files elsewhere, set the environment variables described following so that the driver manager can locate the files\.

 If you are using unixODBC, do the following: 
+  Set `ODBCINI` to the full path and file name of the `odbc.ini` file\. 
+  Set `ODBCSYSINI` to the full path of the directory that contains the `odbcinst.ini` file\. 
+  Set `AMAZONREDSHIFTODBCINI` to the full path and file name of the `amazon.redshiftodbc.ini` file\. 

The following is an example of setting the values above:

```
export ODBCINI=/usr/local/odbc/odbc.ini 
export ODBCSYSINI=/usr/local/odbc 
export AMAZONREDSHIFTODBCINI=/etc/amazon.redshiftodbc.ini
```

### Configuring a connection using a data source name \(DSN\) on Linux<a name="odbc20-dsn-linux"></a>

When connecting to your data store using a data source name \(DSN\), configure the `odbc.ini` file to define data source names \(DSNs\)\. Set the properties in the `odbc.ini` file to create a DSN that specifies the connection information for your data store\.

On Linux operating systems, use the following format:

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

The following example shows the configuration for `odbc.ini` with the 64\-bit ODBC driver on Linux operating systems\.

```
[ODBC Data Sources]
Amazon_Redshift_x64=Amazon Redshift ODBC Driver (x64)

[Amazon_Redshift_x64]
Driver=/opt/amazon/redshiftodbcx64/librsodbc64.so
Host=examplecluster.abc123xyz789.us-west-2.redshift.amazonaws.com
Port=5932Database=dev
locale=en-US
```

### Configuring a connection without a DSN on Linux<a name="odbc20-no-dsn-linux"></a>

 To connect to your data store through a connection that doesn't have a DSN, define the driver in the `odbcinst.ini` file\. Then provide a DSN\-less connection string in your application\.

On Linux operating systems, use the following format:

```
[ODBC Drivers]
driver_name=Installed
...
                            
[driver_name]
Description=driver_description
Driver=path/driver_file
    
...
```

The following example shows the configuration for `odbcinst.ini` with the 64\-bit ODBC driver on Linux operating systems\.

```
[ODBC Drivers]
Amazon Redshift ODBC Driver (x64)=Installed

[Amazon Redshift ODBC Driver (x64)]
Description=Amazon Redshift ODBC Driver (64-bit)
Driver=/opt/amazon/redshiftodbcx64/librsodbc64.so
```