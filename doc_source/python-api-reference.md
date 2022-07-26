# API reference for the Amazon Redshift Python connector<a name="python-api-reference"></a>

Following, you can find a description of the Amazon Redshift Python connector API operations\.

## redshift\_connector<a name="python-api-redshift_connector"></a>

Following, you can find a description of the `redshift_connector` API operation\.

`connect(user, database, password[, port, …])`  
Establishes a connection to an Amazon Redshift cluster\. This function validates user input, optionally authenticates using an identity provider plugin, and then constructs a connection object\.

`apilevel`  
The DBAPI level supported, currently "2\.0"\.

`paramstyle``str(object=’’) -> str str(bytes_or_buffer[, encoding[, errors]]) -> str`  
The database API parameter style to use globally\.

## Connection<a name="python-api-connection"></a>

Following, you can find a description of the connection API operations for the Amazon Redshift Python connector\.

`__init__(user, password, database[, host, …])`  
Initializes a raw connection object\.

`cursor`  
Creates a cursor object bound to this connection\.

`commit`  
Commits the current database transaction\.

`rollback`  
Rolls back the current database transaction\.

`close`  
Closes the database connection\.

`execute(cursor, operation, vals)`  
Runs the specified SQL command\. You can provide the parameters as a sequence or as a mapping, depending upon the value of `redshift_connector.paramstyle`\.

`run(sql[, stream])`  
Runs the specified SQL command\. Optionally, you can provide a stream for use with the COPY command\.

`xid(format_id, global_transaction_id, …)`  
Create a transaction ID\. Only the `global_transaction_id` parameter is used in postgres\. format\_id and branch\_qualifier are not used in postgres\. The `global_transaction_id` can be any string identifier supported by postgres that returns a tuple \(`format_id`, `global_transaction_id`, `branch_qualifier`\)\.

`tpc_begin(xid)`  
Begins a TPC transaction with a transaction ID `xid` consisting of a a format ID, global transaction ID, and branch qualifier\. 

`tpc_prepare`  
Performs the first phase of a transaction started with \.tpc\_begin\.

`tpc_commit([xid])`  
When called with no arguments, \.tpc\_commit commits a TPC transaction previously prepared with \.tpc\_prepare\(\)\.

`tpc_rollback([xid])`  
When called with no arguments, \.tpc\_rollback rolls back a TPC transaction\.

`tpc_recover`  
Returns a list of pending transaction IDs suitable for use with \.tpc\_commit\(xid\) or \.tpc\_rollback\(xid\)\.

## Cursor<a name="python-api-cursor"></a>

Following, you can find a description of the cursor API operation\.

`__init__(connection[, paramstyle])`  
Initializes a raw cursor object\.

`insert_bulk_data(filename, table_name, parameter_indices, column_names, delimiter, batch_size)`  
Runs a bulk INSERT statement\.

`execute(operation[, args, stream, …])`  
Runs a database operation\.

`executemany(operation, param_sets)`  
Prepares a database operation, and then runs it for all parameter sequences or mappings provided\.

`fetchone`  
Fetches the next row of a query result set\.

`fetchmany([num])`  
Fetches the next set of rows of a query result\.

`fetchall`  
Fetches all remaining rows of a query result\.

`close`  
Closes the cursor now\. 

`__iter__`  
A cursor object can be iterated to retrieve the rows from a query\.

`fetch_dataframe([num])`  
Returns a dataframe of the last query results\.

`write_dataframe(df, table)`  
Writes the same structure dataframe into an Amazon Redshift database\.

`fetch_numpy_array([num])`  
Returns a NumPy array of the last query results\.

`get_catalogs`  
Amazon Redshift doesn't support multiple catalogs from a single connection\. Amazon Redshift only returns the current catalog\.

`get_tables([catalog, schema_pattern, …])`  
Returns the unique public tables which are user\-defined within the system\.

`get_columns([catalog, schema_pattern, …])`  
Returns a list of all columns in a specific table in an Amazon Redshift database\.

## AdfsCredentialsProvider plugin<a name="python-adfs-credentials-plugin"></a>

Following is the syntax for the AdfsCredentialsProvider plugin API operation for the Amazon Redshift Python connector\. 

```
redshift_connector.plugin.AdfsCredentialsProvider()
```

## AzureCredentialsProvider plugin<a name="python-azure-credentials-plugin"></a>

Following is the syntax for the AzureCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.AzureCredentialsProvider()
```

## BrowserAzureCredentialsProvider plugin<a name="python-browser-azure-credentials-plugin"></a>

Following is the syntax for the BrowserAzureCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.BrowserAzureCredentialsProvider()
```

## BrowserSamlCredentialsProvider plugin<a name="python-browser-saml-credentials-plugin"></a>

Following is the syntax for the BrowserSamlCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.BrowserSamlCredentialsProvider()
```

## OktaCredentialsProvider plugin<a name="python-okta-credentials-plugin"></a>

Following is the syntax for the OktaCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.OktaCredentialsProvider()
```

## PingCredentialsProvider plugin<a name="python-ping-credentials-plugin"></a>

Following is the syntax for the PingCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.PingCredentialsProvider()
```

## SamlCredentialsProvider plugin<a name="python-saml-credentials-plugin"></a>

Following is the syntax for the SamlCredentialsProvider plugin API operation for the Amazon Redshift Python connector\.

```
redshift_connector.plugin.SamlCredentialsProvider()
```