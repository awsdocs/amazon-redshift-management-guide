# Importing NumPy and connecting to Amazon Redshift<a name="python-basic-test-example"></a>

To import the Amazon Redshift Python connector and Numerical Python \(NumPy\), run the following commands\.

```
import redshift_connector
import numpy
```

To connect to an Amazon Redshift cluster using AWS credentials, run the following command\.

```
conn = redshift_connector.connect(
    host='examplecluster.abc123xyz789.us-west-1.redshift.amazonaws.com',
    port=5439,
    database='dev',
    user='awsuser',
    password='my_password'
 )
```