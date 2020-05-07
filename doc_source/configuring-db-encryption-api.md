# Configuring database encryption using the Amazon Redshift API and AWS CLI<a name="configuring-db-encryption-api"></a>

Use the Amazon Redshift API and AWS Command Line Interface \(AWS CLI\) to configure encryption key options for Amazon Redshift databases\. For more information about database encryption, see [Amazon Redshift database encryption](working-with-db-encryption.md)\.

## Configuring Amazon Redshift to use AWS KMS encryption keys using the Amazon Redshift API and AWS CLI<a name="manage-aws-kms-api-cli"></a>

You can use the following Amazon Redshift API actions to configure Amazon Redshift to use AWS KMS encryption keys\.
+  [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster.html) 
+  [CreateSnapshotCopyGrant](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateSnapshotCopyGrant.html) 
+  [DescribeSnapshotCopyGrants](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeSnapshotCopyGrants.html) 
+  [DeleteSnapshotCopyGrant](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteSnapshotCopyGrant.html) 
+  [DisableSnapshotCopy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DisableSnapshotCopy.html) 
+  [EnableSnapshotCopy](https://docs.aws.amazon.com/redshift/latest/APIReference/API_EnableSnapshotCopy.html) 

You can use the following Amazon Redshift CLI operations to configure Amazon Redshift to use AWS KMS encryption keys\.
+  [create\-cluster](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster.html) 
+  [create\-snapshot\-copy\-grant](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-snapshot-copy-grant.html) 
+  [describe\-snapshot\-copy\-grants](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-snapshot-copy-grants.html) 
+  [delete\-snapshot\-copy\-grant](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-snapshot-copy-grant.html) 
+  [disable\-snapshot\-copy](https://docs.aws.amazon.com/cli/latest/reference/redshift/disable-snapshot-copy.html) 
+  [enable\-snapshot\-copy](https://docs.aws.amazon.com/cli/latest/reference/redshift/enable-snapshot-copy.html) 

## Configuring Amazon Redshift to use an HSM using the Amazon Redshift API and AWS CLI<a name="manage-HSM-api-cli"></a>

You can use the following Amazon Redshift API actions to manage hardware security modules\.
+  [CreateHsmClientCertificate](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateHsmClientCertificate.html) 
+  [CreateHsmConfiguration](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateHsmConfiguration.html) 
+  [DeleteHsmClientCertificate](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteHsmClientCertificate.html) 
+  [DeleteHsmConfiguration](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteHsmConfiguration.html) 
+  [DescribeHsmClientCertificates](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeHsmClientCertificates.html) 
+  [DescribeHsmConfigurations](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeHsmConfigurations.html) 

You can use the following AWS CLI operations to manage hardware security modules\.
+  [create\-hsm\-client\-certificate](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-hsm-client-certificate.html) 
+  [create\-hsm\-configuration](https://docs.aws.amazon.com/cli/latest/reference/redshift/create-hsm-configuration.html) 
+  [delete\-hsm\-client\-certificate](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-hsm-client-certificate.html) 
+  [delete\-hsm\-configuration](https://docs.aws.amazon.com/cli/latest/reference/redshift/delete-hsm-configuration.html) 
+  [describe\-hsm\-client\-certificates](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-hsm-client-certificates.html) 
+  [describe\-hsm\-configurations](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-hsm-configurations.html) 

## Rotating encryption keys using the Amazon Redshift API and AWS CLI<a name="manage-key-rotation-api-cli"></a>

You can use the following Amazon Redshift API actions to rotate encryption keys\.
+  [RotateEncryptionKey](https://docs.aws.amazon.com/redshift/latest/APIReference/API_RotateEncryptionKey.html) 

You can use the following AWS CLI operations to rotate encryption keys\.
+  [rotate\-encryption\-key](https://docs.aws.amazon.com/cli/latest/reference/redshift/rotate-encryption-key.html) 