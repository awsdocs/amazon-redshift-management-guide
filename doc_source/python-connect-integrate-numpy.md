# Integrating the Python connector with NumPy<a name="python-connect-integrate-numpy"></a>

Following is an example of integrating the Python connector with NumPy\.

```
>>>  import numpy
cursor.execute("select * from book")

result: numpy.ndarray = cursor.fetch_numpy_array()
print(result)
```

Following is the result\.

```
[['One Hundred Years of Solitude' 'Gabriel García Márquez']
['A Brief History of Time' 'Stephen Hawking']]
```