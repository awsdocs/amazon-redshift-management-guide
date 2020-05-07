# Purchasing a reserved node offering with the Amazon Redshift console<a name="purchase-reserved-node-offering-console"></a>

You use the **Reserved Nodes** page in the Amazon Redshift console to purchase reserved node offerings, and to view current and past reservations\. 

After you purchase an offering, the **Reserved Node** list displays your reservations and the details of each one, such as the node type, number of nodes, and status of the reservation\. For more information about the reservation details, see [How reserved nodes work](purchase-reserved-node-instance.md#how-reserved-nodes-work)\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="reserved-nodes-purchase"></a>

**To purchase a reserved node**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CLUSTERS**, then choose **Reserved nodes** to display the list of reserved nodes\. 

1. Choose **Purchase reserved nodes** to display the page to choose the properties of the node that you want to purchase\. 

1. Enter the properties of the node, then choose **Purchase reserved nodes**\. 

## Original console<a name="reserved-nodes-purchase-originalconsole"></a><a name="purchase-reserved-nodes-task"></a>

**To purchase a reserved node offering**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Reserved Nodes**\.

1. Choose **Purchase Reserved Nodes**\.

1. In the **Purchase Reserved Nodes** wizard, select **Node Type**, **Term**, and **Offering Type**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/reserved-nodes-purchase-10.png)

1. For **Number of Nodes**, type the number of nodes to reserve\.

1. Choose **Continue**\.

1. Review the offering details, and then choose **Purchase**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/reserved-nodes-purchase-20.png)

1. On the **Reserved Nodes** page, the reservation displays in the reservations list with a status of **payment\-pending**\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

## New console<a name="reserved-node-upgrade"></a>

To upgrade a reserved node, use the **Original Console** or the AWS CLI\.

## Original console<a name="reserved-node-upgrade-originalconsole"></a>

### Upgrade a reserved node from DC1 to DC2<a name="upgrade-reserved-node"></a>

You can upgrade your DC1 reserved nodes to DC2 nodes for the remainder of your current term at no cost\. DC2 is designed for demanding data warehousing workloads that require low latency and high throughput\. 

**Prerequisites**

Migrate the cluster that includes the node you plan to upgrade before upgrading the reserved nodes\. To migrate your DC1 cluster to DC2, use the resize or restore operation\. If your cluster is a **DC1\.large cluster**, you can restore to a new DC2\.large cluster using an existing snapshot\. If your cluster is a **DC1\.8xlage cluster**, you can resize it to be a DC2\.8xlarge cluster\. Make sure that the DC1 cluster is shut down before you upgrade the reserved nodes\. The DC2 cluster accrues on\-demand pricing until you upgrade the DC1 reserved nodes\. 

For more information about restoring from a snapshot, see [Amazon Redshift snapshots](working-with-snapshots.md)\. For more information about resizing a cluster, see [Resizing clusters in Amazon Redshift](managing-cluster-operations.md#rs-resize-tutorial)\. <a name="upgrade-reserved-nodes-task"></a>

**To upgrade a reserved node reservation**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation pane, choose **Reserved Nodes**\.

1. Choose the DC1 reserved node that you want to upgrade\.

1. Review the details on the **Upgrade reserved node** dialog box and choose **Upgrade**\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/upgrade-reservednode.png)

## Upgrading reserved nodes with the AWS CLI<a name="reserved-node-upgrade-cli"></a><a name="upgrade-reserved-nodes-cli"></a>

**To upgrade a reserved node reservation with the AWS CLI**

1. Obtain a list of ReservedNodeOfferingID's for offerings that meet your requirements for payment type, term, and charges\. The following example illustrates this step\. 

   ```
   aws redshift get-reserved-node-exchange-offerings --reserved-node-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
   {
       "ReservedNodeOfferings": [
           {
               "Duration": 31536000,
               "ReservedNodeOfferingId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
               "UsagePrice": 0.0,
               "NodeType": "dc2.large",
               "RecurringCharges": [
                   {
                       "RecurringChargeFrequency": "Hourly",
                       "RecurringChargeAmount": 0.2
                   }
               ],
               "CurrencyCode": "USD",
               "OfferingType": "No Upfront",
               "ReservedNodeOfferingType": "Regular",
               "FixedPrice": 0.0
           }
       ]
   }
   ```

1. Call `accept-reserved-node-exchange` and provide the ID for the DC1 reserved node that you want to exchange along with the ReservedNodeOfferingID you obtained in the previous step\.

   The following example illustrates this step\.

   ```
   aws redshift accept-reserved-node-exchange --reserved-node-id xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx --target-reserved-node-offering-id yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyyy
   {
       "ExchangedReservedNode": {
           "UsagePrice": 0.0,
           "OfferingType": "No Upfront",
           "State": "exchanging",
           "FixedPrice": 0.0,
           "CurrencyCode": "USD",
           "ReservedNodeId": "zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
           "NodeType": "dc2.large",
           "NodeCount": 1,
           "RecurringCharges": [
               {
                   "RecurringChargeFrequency": "Hourly",
                   "RecurringChargeAmount": 0.2
               }
           ],
           "ReservedNodeOfferingType": "Regular",
           "StartTime": "2018-06-27T18:02:58Z",
           "ReservedNodeOfferingId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyyy",
           "Duration": 31536000
       }
   }
   ```

You can confirm that the exchange is complete by calling [describe\-reserved\-nodes](https://docs.aws.amazon.com/cli/latest/reference/redshift/describe-reserved-nodes.html) and checking the value for `Node type`\. 