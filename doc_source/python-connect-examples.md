# Examples of using the Amazon Redshift Python connector<a name="python-connect-examples"></a>

Following are examples of how to use the Amazon Redshift Python connector\.

**Topics**
+ [Connecting to an Amazon Redshift cluster using AWS credentials](#python-connect-cluster)
+ [Querying a table](#python-connect-query)
+ [Retrieving the query result set](#python-connect-retrieve-result)
+ [Enabling autocommit](#python-connect-enable-autocommit)
+ [Using COPY to copy data from and UNLOAD to write data to an Amazon S3 bucket](#python-connect-copy-unload-s3)

## Connecting to an Amazon Redshift cluster using AWS credentials<a name="python-connect-cluster"></a>

To connect to an Amazon Redshift cluster using your AWS credentials, run the following command\.

```
>>> conn = redshift_connector.connect(
     host='examplecluster.abc123xyz789.us-west-1.redshift.amazonaws.com',
     database='dev',
     user='awsuser',
     password='my_password'
  )
```

## Querying a table<a name="python-connect-query"></a>

To select all rows from the table `book`, run the following command\.

```
>>> cursor.execute("select * from book")
```

## Retrieving the query result set<a name="python-connect-retrieve-result"></a>

To retrieve the query result set, run the following command\.

```
>>>  result: tuple = cursor.fetchall()
 print(result)
 >> (['One Hundred Years of Solitude', 'Gabriel García Márquez'], ['A Brief History of Time', 'Stephen Hawking'])
```

## Enabling autocommit<a name="python-connect-enable-autocommit"></a>

The autocommit property is off by default, following the Python Database API Specification\. You can use the following commands to turn on the autocommit property of the connection\. First, you perform a rollback command to make sure that a transaction is not in progress\.

```
>>>  con.rollback()
con.autocommit = True
con.run("VACUUM")
con.autocommit = False
```

## Using COPY to copy data from and UNLOAD to write data to an Amazon S3 bucket<a name="python-connect-copy-unload-s3"></a>

The following example shows how to copy data from an Amazon S3 bucket into a table and then unload from the table into the S3 bucket\.

A text file named `category_csv.txt` containing the following data is uploaded to an S3 bucket\.

```
>>> 12,Shows,Musicals,Musical theatre
13,Shows,Plays,"All ""non-musical"" theatre"
14,Shows,Opera,"All opera, light, and ""rock"" opera"
15,Concerts,Classical,"All symphony, concerto, and choir concerts"
```

Following is an example of the Python code, which first connects to the Amazon Redshift database\. It then creates a table called `category` and copies the CSV data from the S3 bucket into the table\.

```
>>> with redshift_connector.connect(...) as conn:
    with conn.cursor() as cursor:
        cursor.execute("create table category (catid int, cargroup varchar, catname varchar, catdesc varchar)")
        cursor.execute("copy category from 's3://testing/category_csv.txt' iam_role 'arn:aws:iam::123:role/RedshiftCopyUnload' csv;")
        cursor.execute("select * from category")
        print(cursor.fetchall())
        cursor.execute("unload ('select * from category') to 's3://testing/unloaded_category_csv.txt'  iam_role 'arn:aws:iam::123:role/RedshiftCopyUnload' csv;")
        print('done')
```

```
>>> ([12, 'Shows', 'Musicals', 'Musical theatre'], [13, 'Shows', 'Plays', 'All "non-musical" theatre'], [14, 'Shows', 'Opera', 'All opera, light, and "rock" opera'], [15, 'Concerts', 'Classical', 'All symphony, concerto, and choir concerts'])
done
```

The data is unloaded into the file `unloaded_category_csv.text0000_part00` in the S3 bucket\.

```
>>> 12,Shows,Musicals,Musical theatre
13,Shows,Plays,"All ""non-musical"" theatre"
14,Shows,Opera,"All opera, light, and ""rock"" opera"
15,Concerts,Classical,"All symphony, concerto, and choir concerts"
```