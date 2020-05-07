# Configuring database encryption using the console<a name="configuring-db-encryption-console"></a>

You can use the Amazon Redshift console to configure Amazon Redshift to use an HSM and to rotate encryption keys\. For information about how to create clusters using AWS KMS encryption keys, see [Creating a cluster](managing-clusters-console.md#create-cluster) and [Managing clusters using the Amazon Redshift CLI and API](manage-clusters-api-cli.md)\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="cluster-encryption-manage"></a>

**To modify database encryption on a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to move snapshots for\.

1. For **Actions**, choose **Modify** to display the configuration page\. 

1. In the **Database configuration** section, choose a setting for **Encryption**, then choose **Modify cluster**\. 

## Original console<a name="cluster-encryption-manage-originalconsole"></a>

### Configuring Amazon Redshift to use an HSM using the Amazon Redshift console<a name="manage-HSM-console"></a>

You can use the following procedures to specify HSM connection and configuration information for Amazon Redshift by using the Amazon Redshift console\.<a name="create-hsm-connection-task"></a>

**To create an HSM connection**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**, and then choose the **HSM Connections** tab\.

1. Choose **Create HSM Connection\.**  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-connections-00.png)

1. On the **Create HSM Connection** page, type the following information:

   1. In the **HSM Connection Name** box, type a name to identify this connection\.

   1. In the **Description** box, type a description about the connection\.

   1. In the **HSM IP Address** box, type the IP address for your HSM\.

   1. In the **HSM Partition Name** box, type the name of the partition that Amazon Redshift should connect to\.

   1. In the **HSM Partition Password** box, type the password that is required to connect to the HSM partition\.

   1. Copy the public server certificate from your HSM and paste it in the **Paste the HSM's public server certificate here** box\.

   1. Choose **Create**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-connections-create-01.png)

1. After the connection is created, you can create an HSM client certificate\. If you want to create an HSM client certificate immediately after creating the connection, choose **Yes** and complete the steps in the next procedure\. Otherwise, choose **Not now** to return to the list of HSM connections and complete the remainder of the process at another time\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-connections-create-02.png)<a name="create-hsm-client-cert-task"></a>

**To create an HSM client certificate**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**, and then choose the **HSM Certificates** tab\.

1. Choose **Create HSM Client Certificate**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-client-cert-00.png)

1. On the **Create HSM Client Certificate** page, type a name in the **HSM Client Certificate Identifier** box to identify this client certificate\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-client-cert-create-01.png)

1. Choose **Next**\.

1. After the certificate is created, a confirmation page appears with information to register the key on your HSM\. If you do not have permission to configure the HSM, coordinate the following steps with an HSM administrator\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-client-cert-create-02.png)

   1. On your computer, open a new text file\.

   1. In the Amazon Redshift console, on the **Create HSM Client Certificate** confirmation page, copy the public key\.

   1. Paste the public key into the open file and save it with the file name displayed in step 1 from the confirmation page\. Make sure that you save the file with the `.pem` file extension, for example: `123456789mykey.pem`\.

   1. Upload the `.pem` file to your HSM\.

   1. On the HSM, open a command\-prompt window and run the commands listed in step 4 on the confirmation page to register the key\. The command uses the following format, with *ClientName*, *KeyFilename*, and *PartitionName* being values you need to replace with your own:

      `client register -client ClientName -hostname KeyFilename`

      `client assignPartition -client ClientName -partition PartitionName`

      For example:

      `client register -client MyClient -hostname 123456789mykey`

      `client assignPartition -client MyClient -partition MyPartition`

   1. After you register the key on the HSM, choose **Next**\.

1. After the HSM client certificate is created and registered, choose one of the following buttons\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-client-cert-create-03.png)

   1. **Launch a Cluster with HSM**\. This option starts the process of launching a new cluster\. During the process, you can select an HSM to store encryption keys\. For more information about the launch cluster process, see [Managing clusters using the console](managing-clusters-console.md)\.

     **Create an HSM Connection**\. This option starts the **Create HSM Connection** process\.

     **View Certificates**\. This option returns you to **HSM** in the navigation pane and displays a list of client certificates on the **Certificates** tab\.

     **Previous**\. This option returns you to the **Create HSM Client Certificates** confirmation page\.

     **Close**\. This option returns you to **HSM** in the navigation pane and displays a list of HSM connections on the **Connections** tab\.<a name="display-hsm-client-cert-task"></a>

**To display the public key for an HSM client certificate**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1.  In the navigation pane, choose **Security**, and then choose the **HSM Certificates** tab\. 

1. Choose the HSM client certificate to display the public key\. This key is the same one that you added to the HSM in the procedure preceding procedure, [To create an HSM client certificate](#create-hsm-client-cert-task)   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/hsm-client-cert-details.png)<a name="delete-hsm-connection-task"></a>

**To delete an HSM connection**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security**, and then choose the **HSM Connections** tab\.

1. Choose the HSM connection that you want to delete\.

1. In the **Delete HSM Connection** dialog box, choose **Delete** to delete the connection from Amazon Redshift, or choose **Cancel** to return to the **HSM Connections** tab without deleting the connection\.<a name="delete-hsm-client-cert-task"></a>

**To delete an HSM client certificate**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Security** and select the **HSM Certificates** tab\.

1. In the list, choose the HSM client certificate that you want to delete\.

1. In the **Delete HSM Client Certificate** dialog box, choose **Delete** to delete the certificate from Amazon Redshift, or choose **Cancel** to return to the **Certificates** tab without deleting the certificate\.

## Rotating encryption keys using the Amazon Redshift console<a name="manage-key-rotation-console"></a>

You can use the following procedure to rotate encryption keys by using the Amazon Redshift console\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="cluster-rotate-encryption"></a>

**To rotate the encryption keys for a cluster**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose the cluster that you want to update encryption keys\.

1. For **Actions**, choose **Rotate encryption** to display the **Rotate encryption keys** page\. 

1. On the **Rotate encryption keys** page, choose **Rotate encryption keys**\. 

### Original console<a name="cluster-rotate-encryption-originalconsole"></a><a name="rotate-encryption-key-task"></a>

**To rotate an encryption key**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Clusters**\.

1. In the list, choose the cluster for which you want to rotate keys\.

1. Choose **Database**, and then choose **Rotate Encryption Keys**\.

1. Choose **Yes, Rotate Keys** if you want to rotate the keys or **Cancel** if you do not\.
**Note**  
Your cluster will be momentarily unavailable until the key rotation process completes\.