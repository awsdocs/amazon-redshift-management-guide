# Encryption at Rest<a name="security-server-side-encryption"></a>

Server\-side encryption is about data encryption at rest—that is, Amazon Redshift encrypts your data as it writes it in its data centers and decrypts it for you when you access it\. As long as you authenticate your request and you have access permissions, there is no difference in the way you access encrypted or unencrypted objects\. For example, if you share your objects using a presigned URL, that URL works the same way for both encrypted and unencrypted objects\.

**Note**  
You can't apply different types of server\-side encryption to the same data simultaneously\.

You have three mutually exclusive options depending on how you choose to manage the encryption keys:
+ **Use server\-side encryption with Amazon S3\-Managed Keys \(SSE\-S3\)** – Each object is encrypted with a unique key\. As an additional safeguard, it encrypts the key itself with a master key that it regularly rotates\. Amazon Redshift server\-side encryption uses one of the strongest block ciphers available, 256\-bit Advanced Encryption Standard \(AES\-256\), to encrypt your data\. For more information, see [Amazon Redshift Database Encryption](working-with-db-encryption.md)\. 
+ **Use server\-side encryption with AWS KMS\-Managed keys \(SSE\-KMS\)** – Similar to SSE\-S3, but with some additional benefits along with some additional charges for using this service\. There are separate permissions for the use of an envelope key \(that is, a key that protects your data's encryption key\) that provides added protection against unauthorized access of your objects in Amazon S3\. SSE\-KMS also provides you with an audit trail of when your key was used and by whom\. Additionally, you have the option to create and manage encryption keys yourself, or use a default key that is unique to you, the service you're using, and the Region you're working in\. For more information, see [Database Encryption for Amazon Redshift Using AWS KMS](working-with-db-encryption.md#working-with-aws-kms)\.
+ **Use server\-side encryption with customer\-provided keys \(SSE\-C\)** – You manage the encryption keys and Amazon Redshift manages the encryption, as it writes to disks, and decryption, when you access your objects\. For more information, see [Database Encryption for Amazon Redshift Using AWS KMS](working-with-db-encryption.md#working-with-aws-kms)\.

**Note**  
When you list objects in your bucket, the list API returns a list of all objects, regardless of whether they are encrypted\.