# Amazon Redshift RSQL environment variables<a name="rsql-query-tool-environment-variables"></a>

 Amazon Redshift RSQL can use environment variables to select default parameter values\. 

## RSPASSWORD<a name="rsql-query-tool-rspassword"></a>

**Important**  
We don't recommend using this environment variable for security reasons, as some operating systems allow non\-root users to see process environment variables\.

 Sets the password for Amazon Redshift RSQL to use when connecting to Amazon Redshift\. This environment variable requires Amazon Redshift RSQL 1\.0\.4 and above\. 

 RSQL prioritizes RSPASSWORD if one is set\. If RSPASSWORD is not set and you're connecting using a DSN, RSQL takes the password from the DSN file's parameters\. Finally, if RSPASSWORD is not set and you're not using a DSN, RSQL provides a password prompt after attempting to connect\. 

The following is an example of setting an RSPASSWORD:

```
export RSPASSWORD=TestPassw0rd
```