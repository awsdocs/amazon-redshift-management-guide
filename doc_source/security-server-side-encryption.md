# Encryption at rest<a name="security-server-side-encryption"></a>

Server\-side encryption is about data encryption at rest—that is, Amazon Redshift optionally encrypts your data as it writes it in its data centers and decrypts it for you when you access it\. As long as you authenticate your request and you have access permissions, there is no difference in the way you access encrypted or unencrypted data\. 

Amazon Redshift protects data at rest through encryption\. Optionally, you can protect all data stored on disks within a cluster and all backups in Amazon S3 with Advanced Encryption Standard AES\-256\.  

To manage the keys used for encrypting and decrypting your Amazon Redshift resources, you use [AWS Key Management Service \(AWS KMS\)](https://docs.aws.amazon.com/kms/latest/developerguide/)\. AWS KMS combines secure, highly available hardware and software to provide a key management system scaled for the cloud\. Using AWS KMS, you can create encryption keys and define the policies that control how these keys can be used\. AWS KMS supports AWS CloudTrail, so you can audit key usage to verify that keys are being used appropriately\. You can use your AWS KMS keys in combination with Amazon Redshift and supported AWS services\.\. For a list of services that support AWS KMS, see [How AWS Services Use AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services.html) in the *AWS Key Management Service Developer Guide*\.

**Topics**
+ [Amazon Redshift database encryption](working-with-db-encryption.md)