# Data encryption<a name="security-encryption"></a>

Data protection refers to protecting data while in\-transit \(as it travels to and from Amazon Redshift at rest \(while it is stored on disks in Amazon Redshift data centers\)\. You can protect data in transit by using SSL or by using client\-side encryption\. You have the following options of protecting data at rest in Amazon Redshift\.
+ **Use server\-side encryption** – You request Amazon Redshift to encrypt your data before saving it on disks in its data centers and decrypt it when you download the objects\. 
+ **Use client\-side encryption** – You can encrypt data client\-side and upload the encrypted data to Amazon Redshift\. In this case, you manage the encryption process, the encryption keys, and related tools\.