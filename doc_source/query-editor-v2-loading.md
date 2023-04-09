# Loading data into a database<a name="query-editor-v2-loading"></a>

You can use query editor v2 to load data into a database in an Amazon Redshift cluster or workgroup\.

## Loading sample data<a name="query-editor-v2-loading-sample-data"></a>

The query editor v2 comes with sample data and notebooks available to be loaded into a sample database and corresponding schema\. 

To load sample data, choose the ![\[External\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/external.png) icon associated with the sample data you want to load\. The query editor v2 then loads the data into a schema in database `sample_data_dev` and creates a folder of saved notebooks in your **Notebooks** folder\. 

The following sample datasets are available\.

**tickit**  
Most of the examples in the Amazon Redshift documentation use sample data called `tickit`\. This data consists of seven tables: two fact tables and five dimensions\. When you load this data, the schema `tickit` is updated with sample data\. For more information about the `tickit` data, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

**tpch**  
This data is used for a decision support benchmark\. When you load this data, the schema `tpch` is updated with sample data\. For more information about the `tpch` data, see [TPC\-H](http://www.tpc.org/tpch/)\. 

**tpcds**  
This data is used for a decision support benchmark\. When you load this data, the schema `tpcds` is updated with sample data\. For more information about the `tpcds` data, see [TPC\-DS](http://www.tpc.org/tpcds/)\. 

## Loading data from Amazon S3<a name="query-editor-v2-loading-data"></a>

You can load data into an existing table from Amazon S3\.

**To load data into an existing table**

The COPY command is used by query editor v2 to load data from Amazon S3\. The COPY command generated and used in the query editor v2 load data wizard supports many of the parameters available to the COPY command syntax to copy from Amazon S3\. For information about the COPY command and its options used to load data from Amazon S3, see [COPY from Amazon Simple Storage Service](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-source-s3.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Confirm that the table is already created in the database where you want to load data\. The query editor v2 can only load data into an existing table\.

1. Choose ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-upload.png)**Load data**\.

1. For **Data source**, choose **Load from S3**\.

1. In **S3 URIs**, choose **Browse S3** to look for the Amazon S3 bucket that contains the data to load\. 

1. If the specified Amazon S3 bucket isn't in the same AWS Region as the target table, then choose the **S3 file location** for the AWS Region where the data is located\.

1. Choose **This file is a manifest file** if the Amazon S3 file is actually a manifest containing multiple Amazon S3 bucket URIs\.

1. Choose the **File format** for the file to be uploaded\. The supported data formats are CSV, JSON, DELIMITER, FIXEDWIDTH, SHAPEFILE, AVRO, PARQUET, and ORC\. Depending on the specified file format, you can choose the respective **File options**\. You can also select **Data is encrypted** if the data is encrypted and enter the Amazon Resource Name \(ARN\) of the KMS key used to encrypt the data\.

   For PARQUET and ORC, there isn't any file option to configure\.

1. Choose a compression method to compress your file\. The default is no compression\.

1. \(Optional\) The **Advanced settings** support various **Data conversion parameters** and **Load operations**\. Enter this information as needed for your file\.

   For more information about data conversion and data load parameters, see [Data conversion parameters](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html) and [Data load operations](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-load.html) in the *Amazon Redshift Database Developer Guide*\.

1. Confirm or choose the location of the **Target table** including database, schema, and table name where the data is loaded\.

1. \(Optional\) Choose column names to enter them in **Column mapping** to map columns in the order of the input data file\.

1. Choose an **IAM role** that has the required permissions to load data from Amazon S3\.

1. Choose **Load data** to start the data load\.

   When the load completes, the query editor displays with the generated COPY command that was used to load your data\. The **Result** of the COPY is shown\. If successful, you can now use SQL to select data from the loaded table\. When there is an error, query the system view STL\_LOAD\_ERRORS to get more details\. For information about COPY command errors, see [STL\_LOAD\_ERRORS](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_LOAD_ERRORS.html) in the *Amazon Redshift Database Developer Guide*\.

## Loading data from a local file<a name="query-editor-v2-loading-data-local"></a>

You can load data into an existing table from a local character\-separated file that is smaller than 5 MB\.

### Administrator setup to load data from a local file<a name="query-editor-v2-loading-data-local-setup"></a>

Your query editor v2 administrator must specify the common Amazon S3 bucket in the **Account settings** window\. The account users must be configured with the proper permissions\.
+ Required IAM permissions – the users of load from local file must have `s3:ListBucket`, `s3:GetBucketLocation`, `s3:putObject`, `s3:getObject`, and `s3:deleteObject` permissions\. The *optional\-prefix* can be specified to limit query editor v2 related use of this bucket to objects with this prefix\. You might use this option when using this same Amazon S3 bucket for uses other than query editor v2\. For more information about buckets and prefixes, see [Managing user access to specific folders](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html#example-bucket-policies-folders) in the *Amazon Simple Storage Service User Guide*\. To ensure that cross user data access is not allowed, we recommend that the query editor v2 administrator use an Amazon S3 bucket policy to restrict object access based on `aws:userid`\. The following example allows Amazon S3 permissions to a *<staging\-bucket\-name>* with read/write access only to Amazon S3 objects with the `aws:userid` as a prefix\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": [
                  "s3:ListBucket",
                  "s3:GetBucketLocation"
              ],
              "Resource": [
                  "arn:aws:s3:::<staging-bucket-name>"
              ]
          },
          {
              "Effect": "Allow",
              "Action": [
                  "s3:PutObject",
                  "s3:GetObject",
                  "s3:DeleteObject"
              ],
              "Resource": [
                  "arn:aws:s3:::<staging-bucket-name>[/<optional-prefix>]/${aws:userid}/*"
              ]
          }
      ]
  }
  ```
+ Data separation – we recommend that users not have access to each other's data \(even briefly\)\. Load from a local file uses the staging Amazon S3 bucket set up by the query editor v2 administrator\. Configure the bucket policy for the staging bucket to provide data separation between users\. The following example shows a bucket policy that separates data between users of the *<staging\-bucket\-name>*\.

  ```
  {
   "Version": "2012-10-17",
      "Statement": [
          {"Sid": "userIdPolicy",
              "Effect": "Deny",
              "Principal": "*",
              "Action": ["s3:PutObject",
                         "s3:GetObject",
                         "s3:DeleteObject"],
              "NotResource": [
                  "arn:aws:s3:::<staging-bucket-name>[/<optional-prefix>]/${aws:userid}/*"
              ]
           }
      ]
  }
  ```

### Loading data from a local file<a name="query-editor-v2-loading-data-local-procedure"></a>

**To load local file data into an existing table**

Your query editor v2 administrator must specify the common Amazon S3 bucket in the **Account settings** window\. The local file is automatically uploaded to a common Amazon S3 bucket used by your account, and then the COPY command is used by query editor v2 to load data\. The COPY command generated and run by the query editor v2 load local file window supports many of the parameters available to the COPY command syntax to copy from Amazon S3\. For information about the COPY command and its options used to load data from Amazon S3, see [COPY from Amazon S3](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-source-s3.html) in the *Amazon Redshift Database Developer Guide*\. 

1. Confirm that the table is already created in the database where you want to load data\. The query editor v2 can only load data into an existing table\.

1. Confirm that you are connected to the target database in the tree\-view panel of query editor v2\. You can create a connection using the context menu \(right\-click\) to the cluster or workgroup where the data will be loaded\. Without this connection, your load might fail with an `AccessDeniedException` and point to a missing permission `sqlworkbench:GetConnection`\.

1. Choose ![\[Load\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/qev2-upload.png)**Load data**\.

1. For **Data source**, choose **Load from local file**\.

1. Choose **Browse** to look for the file that contains the data to **Load CSV file**\. By default, files with extension `.csv` are shown but you can choose other file types that are character separated using **File options**\.

1. \(Optional\) Choose the **File options** for the file to be uploaded to change the **Delimiter character** used in the file such as a comma `(,)`, pipe `(|)`, or tab `(\t)`\. You can also choose the quote character\. You can select **Data is encrypted** if the data is encrypted and enter the Amazon Resource Name \(ARN\) of the KMS key used to encrypt the data\.

   You can also choose the compression encoding of the input file\.

1. \(Optional\) The **Advanced settings** support various **Data conversion parameters** and **Load operations**\. Enter this information as needed for your file\.

   For more information about data conversion and data load parameters, see [Data conversion parameters](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-conversion.html) and [Data load operations](https://docs.aws.amazon.com/redshift/latest/dg/copy-parameters-data-load.html) in the *Amazon Redshift Database Developer Guide*\.

1. Confirm or choose the location of the **Target table** including **Cluster or workgroup**, **Database**, **Schema**, and **Table** name where the data is loaded\.

1. \(Optional\) You can choose column names to enter in **Column mapping** to map columns in the order of the input data file\.

1. Choose **Load data** to start the data load\.

   When the load completes, a message is displayed whether the load was successful or not\. If successful, you can now use SQL to select data from the loaded table\. When there is an error, query the system view STL\_LOAD\_ERRORS to get more details\. For information about COPY command errors, see [STL\_LOAD\_ERRORS](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_LOAD_ERRORS.html) in the *Amazon Redshift Database Developer Guide*\.

   The COPY command template that was used to load data appears in your **Query history**\. This COPY command template shows some of the parameters used, but it can't be run directly in an editor tab\. For more information about query history, see [Viewing query and tab history](query-editor-v2-using.md#query-editor-v2-history)\.

1. You can change selections on the **Load data** window to run multiple loads\. Choose **Done** or **Cancel** when you are ready to close the window\.