# Amazon Redshift Database Encryption<a name="working-with-db-encryption"></a>

In Amazon Redshift, you can enable database encryption for your clusters to help protect data at rest\. When you enable encryption for a cluster, the data blocks and system metadata are encrypted for the cluster and its snapshots\.

Encryption is an optional, immutable setting of a cluster\. If you want encryption, you enable it during the cluster launch process\.To go from an unencrypted cluster to an encrypted cluster or the other way around, unload your data from the existing cluster and reload it in a new cluster with the chosen encryption setting\. For more information, see [Migrating an Unencrypted Cluster to an Encrypted Cluster](migrating-to-an-encrypted-cluster.md)

Though encryption is an optional setting in Amazon Redshift, we recommend enabling it for clusters that contain sensitive data\. Additionally, you might be required to use encryption depending on the guidelines or regulations that govern your data\. For example, the Payment Card Industry Data Security Standard \(PCI DSS\), the Sarbanes\-Oxley Act \(SOX\), the Health Insurance Portability and Accountability Act \(HIPAA\), and other such regulations provide guidelines for handling specific types of data\.

Amazon Redshift uses a hierarchy of encryption keys to encrypt the database\. You can use either AWS Key Management Service \(AWS KMS\) or a hardware security module \(HSM\) to manage the top\-level encryption keys in this hierarchy\. The process that Amazon Redshift uses for encryption differs depending on how you manage keys\.

Additionally, Amazon Redshift automatically integrates with AWS KMS but not with an HSM\. When you use an HSM, you must use client and server certificates to configure a trusted connection between Amazon Redshift and your HSM\.

## Database Encryption for Amazon Redshift Using AWS KMS<a name="working-with-aws-kms"></a>

When you choose AWS KMS for key management with Amazon Redshift, there is a four\-tier hierarchy of encryption keys\. These keys, in hierarchical order, are the master key, a cluster encryption key \(CEK\), a database encryption key \(DEK\), and data encryption keys\.

When you launch your cluster, Amazon Redshift returns a list of the customer master keys \(CMKs\) that your AWS account has created or has permission to use in AWS KMS\. You select a CMK to use as your master key in the encryption hierarchy\.

By default, Amazon Redshift selects your default key as the master key\. Your default key is an AWS\-managed key that is created for your AWS account to use in Amazon Redshift\. AWS KMS creates this key the first time you launch an encrypted cluster in a region and choose the default key\.

If you don’t want to use the default key, you must have \(or create\) a customer\-managed CMK separately in AWS KMS before you launch your cluster in Amazon Redshift\. Customer\-managed CMKs give you more flexibility, including the ability to create, rotate, disable, define access control for, and audit the encryption keys used to help protect your data\. For more information about creating CMKs, go to [Creating Keys](http://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\.

If you want to use a AWS KMS key from another AWS account, you must have permission to use the key and specify its ARN in Amazon Redshift\. For more information about access to keys in AWS KMS, go to [Controlling Access to Your Keys](http://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

After you choose a master key, Amazon Redshift requests that AWS KMS generate a data key and encrypt it using the selected master key\. This data key is used as the CEK in Amazon Redshift\. AWS KMS exports the encrypted CEK to Amazon Redshift, where it is stored internally on disk in a separate network from the cluster along with the grant to the CMK and the encryption context for the CEK\. Only the encrypted CEK is exported to Amazon Redshift; the CMK remains in AWS KMS\. Amazon Redshift also passes the encrypted CEK over a secure channel to the cluster and loads it into memory\. Then, Amazon Redshift calls AWS KMS to decrypt the CEK and loads the decrypted CEK into memory\. For more information about grants, encryption context, and other AWS KMS\-related concepts, go to [Concepts](http://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\.

Next, Amazon Redshift randomly generates a key to use as the DEK and loads it into memory in the cluster\. The decrypted CEK is used to encrypt the DEK, which is then passed over a secure channel from the cluster to be stored internally by Amazon Redshift on disk in a separate network from the cluster\. Like the CEK, both the encrypted and decrypted versions of the DEK are loaded into memory in the cluster\. The decrypted version of the DEK is then used to encrypt the individual encryption keys that are randomly generated for each data block in the database\.

When the cluster reboots, Amazon Redshift starts with the internally stored, encrypted versions of the CEK and DEK, reloads them into memory, and then calls AWS KMS to decrypt the CEK with the CMK again so it can be loaded into memory\. The decrypted CEK is then used to decrypt the DEK again, and the decrypted DEK is loaded into memory and used to encrypt and decrypt the data block keys as needed\.

For more information about creating Amazon Redshift clusters that are encrypted with AWS KMS keys, see [Creating a Cluster](managing-clusters-console.md#create-cluster) and [Manage Clusters Using the Amazon Redshift CLI and API](manage-clusters-api-cli.md)\.

### Copying AWS KMS\-Encrypted Snapshots to Another Region<a name="configure-snapshot-copy-grant"></a>

AWS KMS keys are specific to a region\. If you enable copying of Amazon Redshift snapshots to another region, and the source cluster and its snapshots are encrypted using a master key from AWS KMS, you need to configure a grant for Amazon Redshift to use a master key in the destination region\. This grant enables Amazon Redshift to encrypt snapshots in the destination region\. For more information cross\-region snapshot copy, see [Copying Snapshots to Another Region](working-with-snapshots.md#cross-region-snapshot-copy)\.

**Note**  
If you enable copying of snapshots from an encrypted cluster and use AWS KMS for your master key, you cannot rename your cluster because the cluster name is part of the encryption context\. If you must rename your cluster, you can disable copying of snapshots in the source region, rename the cluster, and then configure and enable copying of snapshots again\.

The process to configure the grant for copying snapshots is as follows\. 

1. In the destination region, create a snapshot copy grant by doing the following:

   +  If you do not already have an AWS KMS key to use, create one\. For more information about creating AWS KMS keys, go to [Creating Keys](http://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html) in the *AWS Key Management Service Developer Guide*\. 

   + Specify a name for the snapshot copy grant\. This name must be unique in that region for your AWS account\.

   + Specify the AWS KMS key ID for which you are creating the grant\. If you do not specify a key ID, the grant applies to your default key\.

1. In the source region, enable copying of snapshots and specify the name of the snapshot copy grant that you created in the destination region\.

This preceding process is only necessary if you enable copying of snapshots using the AWS CLI, the Amazon Redshift API, or SDKs\. If you use the console, Amazon Redshift provides the proper workflow to configure the grant when you enable cross\-region snapshot copy\. For more information about configuring cross\-region snapshot copy for AWS KMS\-encrypted clusters by using the console, see [Configure Cross\-Region Snapshot Copy for an AWS KMS\-Encrypted Cluster](managing-snapshots-console.md#xregioncopy-kms-encrypted-snapshot)\.

Before the snapshot is copied to the destination region, Amazon Redshift decrypts the snapshot using the master key in the source region and re\-encrypts it temporarily using a randomly generated RSA key that Amazon Redshift manages internally\. Amazon Redshift then copies the snapshot over a secure channel to the destination region, decrypts the snapshot using the internally managed RSA key, and then re\-encrypts the snapshot using the master key in the destination region\.

For more information about configuring snapshot copy grants for AWS KMS\-encrypted clusters, see [Configuring Amazon Redshift to Use AWS KMS Encryption Keys Using the Amazon Redshift API and AWS CLI](configuring-db-encryption-api.md#manage-aws-kms-api-cli)\.

## Encryption for Amazon Redshift Using Hardware Security Modules<a name="working-with-HSM"></a>

If you don’t use AWS KMS for key management, you can use a hardware security module \(HSM\) for key management with Amazon Redshift\. 

**Important**  
HSM encryption is not supported for DC2 node types\.

HSMs are devices that provide direct control of key generation and management\. They provide greater security by separating key management from the application and database layers\. Amazon Redshift supports AWS CloudHSM Classic for key management\. The encryption process is different when you use HSM to manage your encryption keys instead of AWS KMS\.

**Important**  
Amazon Redshift supports only AWS CloudHSM Classic\. We don't support the newer AWS CloudHSM service\. AWS CloudHSM Classic isn't available in all regions\. For more information about available regions, see [Region Table](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)\. 

When you configure your cluster to use an HSM, Amazon Redshift sends a request to the HSM to generate and store a key to be used as the CEK\. However, unlike AWS KMS, the HSM doesn’t export the CEK to Amazon Redshift\. Instead, Amazon Redshift randomly generates the DEK in the cluster and passes it to the HSM to be encrypted by the CEK\. The HSM returns the encrypted DEK to Amazon Redshift, where it is further encrypted using a randomly\-generated, internal master key and stored internally on disk in a separate network from the cluster\. Amazon Redshift also loads the decrypted version of the DEK in memory in the cluster so that the DEK can be used to encrypt and decrypt the individual keys for the data blocks\.

If the cluster is rebooted, Amazon Redshift decrypts the internally\-stored, double\-encrypted DEK using the internal master key to return the internally stored DEK to the CEK\-encrypted state\. The CEK\-encrypted DEK is then passed to the HSM to be decrypted and passed back to Amazon Redshift, where it can be loaded in memory again for use with the individual data block keys\.

### Configuring a Trusted Connection Between Amazon Redshift and an HSM<a name="configure-trusted-connection"></a>

When you opt to use an HSM for management of your cluster key, you need to configure a trusted network link between Amazon Redshift and your HSM\. Doing this requires configuration of client and server certificates\. The trusted connection is used to pass the encryption keys between the HSM and Amazon Redshift during encryption and decryption operations\.

Amazon Redshift creates a public client certificate from a randomly generated private and public key pair\. These are encrypted and stored internally\. You download and register the public client certificate in your HSM, and assign it to the applicable HSM partition\.

You provide Amazon Redshift with the HSM IP address, HSM partition name, HSM partition password, and a public HSM server certificate, which is encrypted by using an internal master key\. Amazon Redshift completes the configuration process and verifies that it can connect to the HSM\. If it cannot, the cluster is put into the INCOMPATIBLE\_HSM state and the cluster is not created\. In this case, you must delete the incomplete cluster and try again\.

**Important**  
When you modify your cluster to use a different HSM partition, Amazon Redshift verifies that it can connect to the new partition, but it does not verify that a valid encryption key exists\. Before you use the new partition, you must replicate your keys to the new partition\. If the cluster is restarted and Amazon Redshift cannot find a valid key, the restart fails\. For more information, see [Replicating Keys Across HSMs](http://docs.aws.amazon.com/cloudhsm/latest/userguide//cli-clone-hapg.html)\. 

For more information about configuring Amazon Redshift to use an HSM, see [Configuring Amazon Redshift to Use an HSM Using the Amazon Redshift console](configuring-db-encryption-console.md#manage-HSM-console) and [Configuring Amazon Redshift to use an HSM Using the Amazon Redshift API and AWS CLI](configuring-db-encryption-api.md#manage-HSM-api-cli)\.

After initial configuration, if Amazon Redshift fails to connect to the HSM, an event is logged\. For more information about these events, see [Amazon Redshift Event Notifications](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-event-notifications.html)\.

## About Rotating Encryption Keys in Amazon Redshift<a name="working-with-key-rotation"></a>

In Amazon Redshift, you can rotate encryption keys for encrypted clusters\. When you start the key rotation process, Amazon Redshift rotates the CEK for the specified cluster and for any automated or manual snapshots of the cluster\. Amazon Redshift also rotates the DEK for the specified cluster, but cannot rotate the DEK for the snapshots while they are stored internally in Amazon Simple Storage Service \(Amazon S3\) and encrypted using the existing DEK\. 

While the rotation is in progress, the cluster is put into a ROTATING\_KEYS state until completion, at which time the cluster returns to the AVAILABLE state\. Amazon Redshift handles decryption and re\-encryption during the key rotation process\.

**Note**  
You cannot rotate keys for snapshots without a source cluster\. Before you delete a cluster, consider whether its snapshots rely on key rotation\.

Because the cluster is momentarily unavailable during the key rotation process, you should rotate keys only as often as your data needs require or when you suspect the keys might have been compromised\. As a best practice, you should review the type of data that you store and plan how often to rotate the keys that encrypt that data\. The frequency for rotating keys varies depending on your corporate policies for data security, and any industry standards regarding sensitive data and regulatory compliance\. Ensure that your plan balances security needs with availability considerations for your cluster\.

For more information about rotating keys, see [Rotating Encryption Keys Using the Amazon Redshift console](configuring-db-encryption-console.md#manage-key-rotation-console) and [Rotating Encryption Keys Using the Amazon Redshift API and AWS CLI](configuring-db-encryption-api.md#manage-key-rotation-api-cli)\.