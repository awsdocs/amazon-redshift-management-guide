# Examples of using the Amazon Redshift Python connector<a name="python-connect-examples"></a>

Following are examples of how to use the Amazon Redshift Python connector\. To run them, you must first install the Python connector\. For more information on installing the Amazon Redshift Python connector, see [Installing the Amazon Redshift Python connector](python-driver-install.md)\.

**Topics**
+ [Connecting to and querying an Amazon Redshift cluster using AWS credentials](#python-connect-cluster)
+ [Enabling autocommit](#python-connect-enable-autocommit)
+ [Using COPY to copy data from an Amazon S3 bucket and UNLOAD to write data to it](#python-connect-copy-unload-s3)

## Connecting to and querying an Amazon Redshift cluster using AWS credentials<a name="python-connect-cluster"></a>

The following example guides you through connecting to an Amazon Redshift cluster using your AWS credentials, then querying a table and retrieving the query results\.

```
#Connect to the cluster
>>> import redshift_connector
>>> conn = redshift_connector.connect(
     host='examplecluster.abc123xyz789.us-west-1.redshift.amazonaws.com',
     database='dev',
     user='awsuser',
     password='my_password'
  )
  
# Create a Cursor object
>>> cursor = conn.cursor()

# Query a table using the Cursor
>>> cursor.execute("select * from book")
                
#Retrieve the query result set
>>> result: tuple = cursor.fetchall()
>>> print(result)
 >> (['One Hundred Years of Solitude', 'Gabriel García Márquez'], ['A Brief History of Time', 'Stephen Hawking'])
```

## Enabling autocommit<a name="python-connect-enable-autocommit"></a>

The autocommit property is off by default, following the Python Database API Specification\. You can use the following commands to turn on the connection's autocommit property after performing a rollback command to make sure that a transaction is not in progress\.

```
#Connect to the cluster
>>> import redshift_connector
>>> conn = redshift_connector.connect(...)

# Run a rollback command
>>>  conn.rollback()

# Turn on autocommit
>>>  conn.autocommit = True
>>>  conn.run("VACUUM")

# Turn off autocommit
>>>  conn.autocommit = False
```

## Using COPY to copy data from an Amazon S3 bucket and UNLOAD to write data to it<a name="python-connect-copy-unload-s3"></a>

The following example shows how to copy data from an Amazon S3 bucket into a table and then unload from that table back into the bucket\.

A text file named `category_csv.txt` containing the following data is uploaded to an Amazon S3 bucket:\.

```
12,Shows,Musicals,Musical theatre
13,Shows,Plays,"All ""non-musical"" theatre"
14,Shows,Opera,"All opera, light, and ""rock"" opera"
15,Concerts,Classical,"All symphony, concerto, and choir concerts"
```

Following is an example of the Python code, which first connects to the Amazon Redshift database\. It then creates a table called `category` and copies the CSV data from the S3 bucket into the table\.

```
#Connect to the cluster and create a Cursor
>>> import redshift_connector
>>> with redshift_connector.connect(...) as conn:
>>> with conn.cursor() as cursor:

#Create an empty table
>>>     cursor.execute("create table category (catid int, cargroup varchar, catname varchar, catdesc varchar)")

#Use COPY to copy the contents of the S3 bucket into the empty table 
>>>     cursor.execute("copy category from 's3://testing/category_csv.txt' iam_role 'arn:aws:iam::123:role/RedshiftCopyUnload' csv;")

#Retrieve the contents of the table
>>>     cursor.execute("select * from category")
>>>     print(cursor.fetchall())

#Use UNLOAD to copy the contents of the table into the S3 bucket
>>>     cursor.execute("unload ('select * from category') to 's3://testing/unloaded_category_csv.txt'  iam_role 'arn:aws:iam::123:role/RedshiftCopyUnload' csv;")

#Retrieve the contents of the bucket
>>>     print(cursor.fetchall())
 >> ([12, 'Shows', 'Musicals', 'Musical theatre'], [13, 'Shows', 'Plays', 'All "non-musical" theatre'], [14, 'Shows', 'Opera', 'All opera, light, and "rock" opera'], [15, 'Concerts', 'Classical', 'All symphony, concerto, and choir concerts'])
```

The data is unloaded into the file `unloaded_category_csv.text0000_part00` in the S3 bucket, with the following content:

```
12,Shows,Musicals,Musical theatre
13,Shows,Plays,"All ""non-musical"" theatre"
14,Shows,Opera,"All opera, light, and ""rock"" opera"
15,Concerts,Classical,"All symphony, concerto, and choir concerts"
```