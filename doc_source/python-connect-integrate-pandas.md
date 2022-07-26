# Integrating the Python connector with pandas<a name="python-connect-integrate-pandas"></a>

Following is an example of integrating the Python connector with pandas\.

```
>>>  import pandas

cursor.execute("select * from book")
result: pandas.DataFrame = cursor.fetch_dataframe()
print(result)
```