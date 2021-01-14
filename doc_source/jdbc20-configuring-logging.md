# Configure logging<a name="jdbc20-configuring-logging"></a>

You can enable logging in the driver to assist in diagnosing issues\.

You can log driver information by using the following methods:
+ To save logged information in \.log files, see [Using Log Files](#jdbc20-using-log-files)\.
+ To send logged information to the LogStream or LogWriter specified in the DriverManager, see [Using LogStream or LogWriter](#jdbc20-logstream-option)\. 

You provide the configuration information to the driver in the connection URL\. For more information about the syntax of the connection URL, see [Building the connection URL](jdbc20-obtain-url.md#jdbc20-build-connection-url)\.

## Using Log Files<a name="jdbc20-using-log-files"></a>

You should only enable logging long enough to capture an issue\. Logging decreases performance and can consume a large quantity of disk space\. 

Set the LogLevel key in your connection URL to enable logging and specify the amount of detail included in log files\. The following table lists the logging levels provided by the Amazon Redshift JDBC driver version 2\.0, in order from least verbose to most verbose\. 


| LogLevel value | Description | 
| --- | --- | 
|  1  |  Log severe error events that will lead the driver to abort\.  | 
|  2  |  Log error events that might allow the driver to continue running\.  | 
|  3  |  Log events that might result in an error if action is not taken\.  | 
|  4  |  Log general information that describes the progress of the driver\.  | 
|  5  |  Log detailed information that is useful for debugging the driver\.  | 
|  6  |  Log all driver activity\.  | 

**To enable logging that uses log files**

1. Set the LogLevel property to the desired level of information to include in log files\.

1. Set the LogPath property to the full path to the folder where you want to save log files\. 

   For example, the following connection URL enables logging level 3 and saves the log files in the C:\\temp folder: `jdbc:redshift://redshift.company.us-west- 1.redshift.amazonaws.com:9000/Default;DSILogLevel=3; LogPath=C:\temp`

1. To make sure that the new settings take effect, restart your JDBC application and reconnect to the server\.

   The Amazon Redshift JDBC driver produces the following log files in the location specified in the LogPath property:
   +  redshift\_jdbc\.log file that logs driver activity that is not specific to a connection\.
   + redshift\_jdbc\_connection\_\[Number\]\.log file for each connection made to the database, where \[Number\] is a number that identifies each log file\. This file logs driver activity that is specific to the connection\.

If the LogPath value is invalid, then the driver sends the logged information to the standard output stream \(`System.out`\)

## Using LogStream or LogWriter<a name="jdbc20-logstream-option"></a>

Only enable logging long enough to capture an issue\. Logging decreases performance and can consume a large quantity of disk space\. 

Set the LogLevel key in your connection URL to enable logging and specify the amount of detail sent to the LogStream or LogWriter specified in the DriverManager\. 

**To enable logging that uses the LogStream or LogWriter:**

1. To configure the driver to log general information that describes the progress of the driver, set the LogLevel property to 1 or INFO\.

1. To make sure that the new settings take effect, restart your JDBC application and reconnect to the server\.

**To disable logging that uses the LogStream or LogWriter:**

1. Remove the LogLevel property from the connection URL\.

1. To make sure that the new settings take effect, restart your JDBC application and reconnect to the server\.