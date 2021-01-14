# Integrating Amazon Redshift with an AWS Partner \(preview\)<a name="partner-integration"></a>


|  | 
| --- |
| This is prerelease documentation for the partner integration feature for Amazon Redshift, which is in preview release\. The documentation and the feature are both subject to change\. We recommend that you use this feature only with test clusters, and not in production environments\. For preview terms and conditions, see Beta Service Participation in [AWS Service Terms](https://aws.amazon.com/service-terms/)\.   | 

By working with Amazon Redshift, you can integrate with AWS Partners from the **Cluster details** page on the Amazon Redshift console\. From the **Cluster details** page, you can speed up your data onboarding into your Amazon Redshift data warehouse with AWS Partner applications\. You can also join and analyze data from different sources together with existing data in your cluster\. The following AWS Partners can integrate with Amazon Redshift: 
+ [Etleap](https://etleap.com/)
+ [Fivetran](https://fivetran.com/)
+ [Segment](https://segment.com/)

Use the following procedure to integrate a cluster with an AWS Partner\. 

**To integrate an Amazon Redshift cluster with an AWS Partner**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **CLUSTERS**\. 

1. Choose the cluster that you want to integrate\. 

1. Choose **Add partner integration**\. The **Choose partner** page opens with details about the available AWS Partners\. 

1. Choose an AWS Partner, then choose **Next**\. 

   More details about the chosen AWS Partner appear, along with details about the cluster that you are integrating\. The **Cluster details** section includes information that you provide on the AWS Partner website such as the **Cluster identifier**, **Endpoint**, **Database name**, and **User name** \(which is a database user name\)\. This information is sent to the partner that you chose\. 

1. Choose **Add partner** to open the AWS Partner's website\.

1. Configure the integration with your Amazon Redshift cluster on the partner's website\. On the partner's website, you can select and configure the data sources that are loaded to your Amazon Redshift cluster\. You can also define additional extract, load, and transform \(ELT\) transformations to process your business data, join it with other datasets, and build consolidated views for analysis and reporting\.  