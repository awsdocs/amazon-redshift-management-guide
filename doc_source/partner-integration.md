# Integrating Amazon Redshift with an AWS Partner<a name="partner-integration"></a>

By working with Amazon Redshift, you can integrate with AWS Partners from the **Cluster details** page on the Amazon Redshift console\. From the **Cluster details** page, you can speed up your data onboarding into your Amazon Redshift data warehouse with AWS Partner applications\. You can also join and analyze data from different sources together with existing data in your cluster\. Before completing integration with Informatica, you must add the partner's IP addresses to the allowlist of inbound traffic\. The following AWS Partners can integrate with Amazon Redshift: 
+ [Datacoral](https://www.datacoral.com/aws-partnership/)
+ [Etleap](https://etleap.com/partners/aws-amazon-web-services/)
+ [Fivetran](https://fivetran.com/partners/aws)
+ [SnapLogic](https://www.snaplogic.com/partners/amazon-web-services)
+ [Stitch](https://www.stitchdata.com/data-warehouses/amazon-redshift/)
+ [Upsolver](https://www.upsolver.com/integrations/redshift)
+ [Matillion \(preview\)](https://www.matillion.com/technology/cloud-data-warehouse/amazon-redshift/)
+ [Sisense \(preview\)](https://www.sisense.com/)

AWS Partners can integrate with Amazon Redshift using the AWS CLI or Amazon Redshift API operations\. For more information, see the *AWS CLI Command Reference* or the *Amazon Redshift API Reference*\. 

## Integrating with an AWS Partner using the Amazon Redshift console<a name="partner-integration-console"></a>

Use the following procedure to integrate a cluster with an AWS Partner\. 

**To integrate an Amazon Redshift cluster with an AWS Partner**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**\. 

1. Choose the cluster that you want to integrate\. 

1. Choose **Add partner integration**\. The **Choose partner** page opens with details about the available AWS Partners\. 

1. Choose an AWS Partner, then choose **Next**\. 

   More details about the chosen AWS Partner appear, along with details about the cluster that you are integrating\. The **Cluster details** section includes information that you provide on the AWS Partner website such as the **Cluster identifier**, **Endpoint**, **Database name**, and **User name** \(which is a database user name\)\. This information is sent to the partner that you chose\. 

1. Choose **Add partner** to open the AWS Partner's website\.

1. Configure the integration with your Amazon Redshift cluster on the partner's website\. On the partner's website, you can select and configure the data sources that are loaded to your Amazon Redshift cluster\. You can also define additional extract, load, and transform \(ELT\) transformations to process your business data, join it with other datasets, and build consolidated views for analysis and reporting\.  

You can view and manage AWS Partner integrations from the cluster details **Properties** tab\. The **Integrations** section lists the **Partner** name that you can use to link to the AWS Partner website, the **Status** of the integration, the **Database** that receives the data, and the **Last successful connection** that might have updated the cluster\. 

The possible status values are as follows: 
+ Active – The AWS Partner can connect to the cluster and complete configured tasks\.
+ Inactive – The AWS Partner integration doesn't exist\.
+ Runtime failure – The AWS Partner can connect to the cluster but can't complete configured tasks\.
+ Connection failure – The AWS Partner can't connect to the cluster\. 

After you delete an AWS Partner integration from Amazon Redshift, data continues to flow into your cluster\. Complete the delete on the partner's website\.

## Loading data with AWS partners<a name="partner-integration-data-load"></a>

Aside from integrating a partner with an Amazon Redshift cluster, you can also move data from more than 30 sources into your cluster using our partner's data loading tools\. Before you do so, you must add the partner's IP addresses \(found below\) to the allowlist of inbound rules\. For more information about adding rules to an Amazon EC2 security group, see [ Authorizing Inbound Traffic for Your Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\. Note that while the Informatica Data Loader tool is free, data ingress charges might apply depending on the data sources and targets you choose\.

You can load data from the following partners:
+ [Informatica](https://www.informatica.com/solutions/explore-ecosystems/aws.html) – [IP addresses](https://knowledge.informatica.com/s/article/611041?language=en_US)

**To integrate an Amazon Redshift cluster with Informatica**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose AWS partner integration, then choose the partner you want to integrate your cluster with\.

1. Choose **Complete <partner\-name> integration**\. You will be redirected to the partner's integration site\.

1. Enter the necessary details on the partner's site and complete the integration\.