# Purchasing a reserved node offering with the Amazon Redshift console<a name="purchase-reserved-node-offering-console"></a>

You use the **Reserved Nodes** page in the Amazon Redshift console to purchase reserved node offerings, and to view current and past reservations\. 

After you purchase an offering, the **Reserved Node** list displays your reservations and the details of each one, such as the node type, number of nodes, and status of the reservation\. For more information about the reservation details, see [How reserved nodes work](purchase-reserved-node-instance.md#how-reserved-nodes-work)\. 

**To purchase a reserved node**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **Clusters**, then choose **Reserved nodes** to display the list of reserved nodes\. 

1. Choose **Purchase reserved nodes** to display the page to choose the properties of the node that you want to purchase\.

1. Enter the properties of the node, then choose **Purchase reserved nodes**\. 

To upgrade a reserved node, use  the AWS CLI\.

You can't convert all node types to reserved nodes, and it's also possible that an existing reserved node isn't available for renewal\. This might be because the node type is discontinued\. Contact customer support to renew a discontinued node type\.

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