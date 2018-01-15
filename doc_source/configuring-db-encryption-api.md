# Configuring Database Encryption Using the Amazon Redshift API and AWS CLI<a name="configuring-db-encryption-api"></a>

Use the Amazon Redshift API and AWS Command Line Interface \(AWS CLI\) to configure encryption key options for Amazon Redshift databases\. For more information about database encryption, see [Amazon Redshift Database Encryption](working-with-db-encryption.md)\.

## Configuring Amazon Redshift to Use AWS KMS Encryption Keys Using the Amazon Redshift API and AWS CLI<a name="manage-aws-kms-api-cli"></a>

You can use the following Amazon Redshift API actions to configure Amazon Redshift to use AWS KMS encryption keys\.

+  [CreateCluster](http://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster.html) 

+  [CreateSnapshotCopyGrant](http://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateSnapshotCopyGrant.html) 

+  [DescribeSnapshotCopyGrants](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeSnapshotCopyGrants.html) 

+  [DeleteSnapshotCopyGrant](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteSnapshotCopyGrant.html) 

+  [DisableSnapshotCopy](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DisableSnapshotCopy.html) 

+  [EnableSnapshotCopy](http://docs.aws.amazon.com/redshift/latest/APIReference/API_EnableSnapshotCopy.html) 

You can use the following Amazon Redshift CLI operations to configure Amazon Redshift to use AWS KMS encryption keys\.

+  [create\-cluster](http://docs.aws.amazon.com/cli/latest/reference/redshift/create-cluster.html) 

+  [create\-snapshot\-copy\-grant](http://docs.aws.amazon.com/cli/latest/reference/redshift/create-snapshot-copy-grant.html) 

+  [describe\-snapshot\-copy\-grants](http://docs.aws.amazon.com/cli/latest/reference/redshift/describe-snapshot-copy-grants.html) 

+  [delete\-snapshot\-copy\-grant](http://docs.aws.amazon.com/cli/latest/reference/redshift/delete-snapshot-copy-grant.html) 

+  [disable\-snapshot\-copy](http://docs.aws.amazon.com/cli/latest/reference/redshift/disable-snapshot-copy.html) 

+  [enable\-snapshot\-copy](http://docs.aws.amazon.com/cli/latest/reference/redshift/enable-snapshot-copy.html) 

## Configuring Amazon Redshift to use an HSM Using the Amazon Redshift API and AWS CLI<a name="manage-HSM-api-cli"></a>

You can use the following Amazon Redshift API actions to manage hardware security modules\.

+  [CreateHsmClientCertificate](http://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateHsmClientCertificate.html) 

+  [CreateHsmConfiguration](http://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateHsmConfiguration.html) 

+  [DeleteHsmClientCertificate](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteHsmClientCertificate.html) 

+  [DeleteHsmConfiguration](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteHsmConfiguration.html) 

+  [DescribeHsmClientCertificates](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeHsmClientCertificates.html) 

+  [DescribeHsmConfigurations](http://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeHsmConfigurations.html) 

You can use the following AWS CLI operations to manage hardware security modules\.

+  [create\-hsm\-client\-certificate](http://docs.aws.amazon.com/cli/latest/reference/redshift/create-hsm-client-certificate.html) 

+  [create\-hsm\-configuration](http://docs.aws.amazon.com/cli/latest/reference/redshift/create-hsm-configuration.html) 

+  [delete\-hsm\-client\-certificate](http://docs.aws.amazon.com/cli/latest/reference/redshift/delete-hsm-client-certificate.html) 

+  [delete\-hsm\-configuration](http://docs.aws.amazon.com/cli/latest/reference/redshift/delete-hsm-configuration.html) 

+  [describe\-hsm\-client\-certificates](http://docs.aws.amazon.com/cli/latest/reference/redshift/describe-hsm-client-certificates.html) 

+  [describe\-hsm\-configurations](http://docs.aws.amazon.com/cli/latest/reference/redshift/describe-hsm-configurations.html) 

## Rotating Encryption Keys Using the Amazon Redshift API and AWS CLI<a name="manage-key-rotation-api-cli"></a>

You can use the following Amazon Redshift API actions to rotate encryption keys\.

+  [RotateEncryptionKey](http://docs.aws.amazon.com/redshift/latest/APIReference/API_RotateEncryptionKey.html) 

You can use the following AWS CLI operations to rotate encryption keys\.

+  [rotate\-encryption\-key](http://docs.aws.amazon.com/cli/latest/reference/redshift/rotate-encryption-key.html) 