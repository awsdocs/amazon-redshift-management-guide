# Creating initialization \(\.ini\) files for JDBC driver version 2\.0<a name="jdbc20-ini-file"></a>

By using initialization \(\.ini\) files for Amazon Redshift JDBC driver version 2\.0, you can specify system level configuration parameters\. For example, federated IdP authentication parameters can vary for each application\. The \.ini file provides a common location for SQL clients to get the required configuration parameters\. 

You can create an JDBC driver version 2\.0 initialization \(\.ini\) file that contains configuration options for SQL clients\. The default name of the file is `rsjdbc.ini`\. The JDBC driver version 2\.0 checks for the \.ini file in the following locations, listed in order of precedence:
+ `IniFile` parameter in the connection URL or in the connection property dialog box of the SQL client\. Be sure that the `IniFile` parameter contains the full path to the \.ini file, including the file name\. For information about the `IniFile` parameter, see [IniFile](jdbc20-configuration-options.md#jdbc20-inifile-option)\. If the `IniFile` parameter incorrectly specifies the location of the \.ini file, an error displays\.
+ Environment variables such as AMAZON\_REDSHIFT\_JDBC\_INI\_FILE with the full path, including the file name\. You can use `rsjdbc.ini` or specify a file name\. If the AMAZON\_REDSHIFT\_JDBC\_INI\_FILE environment variable incorrectly specifies the location of the \.ini file, an error displays\.
+ Directory where the driver JAR file is located\.
+ User home directory\.
+ Temp directory of the system\.

You can organize the \.ini file into sections, for example \[DRIVER\]\. Each section contains key\-value pairs that specify various connection parameters\. You can use the `IniSection` parameter to specify a section in the \.ini file\. For information about the `IniSection` parameter, see [IniSection](jdbc20-configuration-options.md#jdbc20-inisection-option)\. 

Following is an example of the \.ini file format, with sections for \[DRIVER\], \[DEV\], \[QA\], and \[PROD\]\. The \[DRIVER\] section can apply to any connection\.

```
[DRIVER]
key1=val1
key2=val2

[DEV]
key1=val1
key2=val2

[QA]
key1=val1
key2=val2

[PROD]
key1=val1
key2=val2
```

The JDBC driver version 2\.0 loads configuration parameters from the following locations, listed in order of precedence:
+ Default configuration parameters in the application code\.
+ \[DRIVER\] section properties from the \.ini file, if included\.
+ Custom section configuration parameters, if the `IniSection` option is provided in the connection URL or in the connection property dialog box of the SQL client\.
+ Properties from the connection property object specified in the `getConnection` call\.
+ Configuration parameters speified in the connection URL\.