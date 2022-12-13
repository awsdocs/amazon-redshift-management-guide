# Other configuration options<a name="spark-redshift-connector-other-config"></a>

## Change the maximum size of string columns<a name="spark-redshift-connector-other-config-max-size"></a>

Redshift creates string columns as text columns when creating tables, which are stored as VARCHAR\(256\)\. If you want columns that support larger sizes, you can use maxlength to specify the maximum length of string columns\. The following is an example of how to specify `maxlength`\. 

```
columnLengthMap.foreach { case (colName, length) =>
  val metadata = new MetadataBuilder().putLong("maxlength", length).build()
  df = df.withColumn(colName, df(colName).as(colName, metadata))
}
```

## Setting a column type<a name="spark-redshift-connector-other-config-column-type"></a>

To set a column type, use the `redshift_type` field\.

```
columnTypeMap.foreach { case (colName, colType) =>
  val metadata = new MetadataBuilder().putString("redshift_type", colType).build()
  df = df.withColumn(colName, df(colName).as(colName, metadata))
}
```

## Setting a compression encoding on a column<a name="spark-redshift-connector-other-config-compression-encoding"></a>

 To use a specific compression encoding on a column, use the encoding field\. For a full list of support compression encodings, see [Compression encodings](https://docs.aws.amazon.com/redshift/latest/dg/c_Compression_encodings.html)\. 

## Setting a description for a column<a name="spark-redshift-connector-other-config-description"></a>

To set a description, use the `description` field\.

## Authentication between Redshift and Amazon S3<a name="spark-redshift-connector-other-config-unload-as-text"></a>

 By default, the result is unloaded to Amazon S3 in the parquet format\. To unload the result as pipe\-delimited text file, specify the following option\. 

```
.option("unload_s3_format", "TEXT")
```

## Connector parameters<a name="spark-redshift-connector-other-config-spark-parameters"></a>

The parameter map or `OPTIONS` in Spark SQL supports the following settings\.

[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/redshift/latest/mgmt/spark-redshift-connector-other-config.html)

**Note**  
 Acknowledgement: This documentation contains sample code and language developed by the [Apache Software Foundation](http://www.apache.org/) licensed under the [Apache 2\.0 license](https://www.apache.org/licenses/LICENSE-2.0)\. 