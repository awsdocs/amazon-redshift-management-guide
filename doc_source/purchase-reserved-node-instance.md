# Purchasing Amazon Redshift reserved nodes<a name="purchase-reserved-node-instance"></a>

## Overview<a name="purchase-reserved-node-offering-overview"></a>

In AWS, the charges that you accrue for using Amazon Redshift are based on compute nodes\. Each compute node is billed at an hourly rate\. The hourly rate varies depending on factors such as region, node type, and whether the node receives on\-demand node pricing or reserved node pricing\.

*On\-demand node pricing* is the most expensive, but most flexible option in Amazon Redshift\. With on\-demand rates, you are charged only for compute nodes that you have in a running cluster\. If you shut down or delete a cluster, you are no longer charged for compute nodes that were in that cluster\. You are billed only for the compute nodes that you use, and no more\. The hourly rate that you are charged for each compute node varies depending on factors such as region and node type\.

*Reserved node pricing* is less expensive than on\-demand pricing because compute nodes are billed at discounted hourly rates\. However, to receive these discounted rates, you must purchase reserved node offerings\. When you purchase an offering, you make a reservation\. The reservation sets a discounted rate for each node that you reserve for the duration of the reservation\. The discounted rate in an offering varies depending on factors such as the region, node type, duration, and payment option\.

You may designate a node as a reserved node by calling the `PurchaseReservedNodeOffering` API operation or choosing **Purchase reserved nodes** on the Amazon Redshift console\. When you purchase a reserved node, you must specify an AWS Region, node type, term, quantity of nodes, and offering type for the applicable reserved node type\. The reserved node may only be used in the designated AWS Region\. 

This topic discusses what reserved node offerings are and how you can purchase them to reduce the cost of running your Amazon Redshift clusters\. This topic discusses rates in general terms as on\-demand or discounted so you can understand pricing concepts and how pricing affects billing\. For more information about specific rates, go to [Amazon Redshift Pricing](https://aws.amazon.com/redshift/pricing/)\.

### About reserved node offerings<a name="reserverd-node-offering-concept"></a>

If you intend to keep your Amazon Redshift cluster running continuously for a prolonged period, you should consider purchasing reserved node offerings\. These offerings provide significant savings over on\-demand pricing, but they require you to reserve compute nodes and commit to paying for those nodes for either a one\-year or three\-year duration\. 

Reserved nodes are a billing concept that is used strictly to determine the rate at which you are charged for nodes\. Reserving a node does not actually create any nodes for you\. You are charged for reserved nodes regardless of usage, which means that you must pay for each node that you reserve for the duration of the reservation, whether or not you have any nodes in a running cluster to which the discounted rate applies\.

In the evaluation phase of your project or when you're developing a proof of concept, on\-demand pricing gives you the flexibility to pay as you go, to pay only for what you use, and to stop paying at any time by shutting down or deleting clusters\. After you have established the needs of your production environment and begin the implementation phase, you should consider reserving compute nodes by purchasing one or more offerings\.

An offering can apply to one or more compute nodes\. You specify the number of compute nodes to reserve when you purchase the offering\. You might choose to purchase one offering for multiple compute nodes, or you might choose to purchase multiple offerings and specify a certain number of compute nodes in each offering\.

For example, any of the following are valid ways to purchase an offering for three compute nodes: 
+ Purchase one offering and specify three compute nodes\.
+ Purchase two offerings, and specify one compute node for the first offering and two compute nodes for the second offering\.
+ Purchase three offerings, and specify one compute node for each of the offerings\.

### Comparing pricing among reserved node offerings<a name="comparing-reserved-node-pricing"></a>

Amazon Redshift provides several payment options for offerings\. The payment option that you choose affects the payment schedule and the discounted rate that you are charged for the reservation\. The more that you pay upfront for the reservation, the better the overall savings are\.

The following payment options are available for offerings\. The offerings are listed in order from least to most savings over on\-demand rates\. 

**Note**  
You are charged the applicable hourly rate for every hour in the specified duration of the reservation, regardless of whether you use the reserved node or not\. The payment option just determines the frequency of payments and the discount to be applied\. For more information, see [About reserved node offerings](#reserverd-node-offering-concept)\.


**Comparing reserved node offerings**  

| Payment option | Payment schedule | Comparative savings | Duration | Upfront charges | Recurring monthly charges | 
| --- | --- | --- | --- | --- | --- | 
| No Upfront | Monthly installments for the duration of the reservation\. No upfront payment\. | About a 20 percent discount over on\-demand rates\. | One\-year term | None | Yes | 
| Partial Upfront | Partial upfront payment, and monthly installments for the duration of the reservation\. | Up to 41 percent to 73 percent discount depending on duration\. | One\-year or three\-year term | Yes | Yes | 
| All Upfront | Full upfront payment for the reservation\. No monthly charges\. | Up to 42 percent to 76 percent discount depending on duration\. | One\-year or three\-year term | Yes | None | 

**Note**  
If you previously purchased **Heavy Utilization** offerings for Amazon Redshift, the comparable offering is the **Partial Upfront** offering\.

### How reserved nodes work<a name="how-reserved-nodes-work"></a>

With reserved node offerings, you pay according to the payment terms as described in the preceding section\. You pay this way whether you already have a running cluster or you launch a cluster after you have a reservation\.

When you purchase an offering, your reservation has a status of **payment\-pending** until the reservation is processed\. If the reservation fails to be processed, the status displays as **payment\-failed** and you can try the process again\. Once your reservation is successfully processed, its status changes to **active**\. The applicable discounted rate in your reservation is not applied to your bill until the status changes to **active**\. After the reservation duration elapses, the status changes to **retired** but you can continue to access information about the reservation for historical purposes\. When a reservation is **retired**, your clusters continue to run but you might be billed at the on\-demand rate unless you have another reservation that applies discounted pricing to the nodes\.

Reserved nodes are specific to the region in which you purchase the offering\. If you purchase an offering by using the Amazon Redshift console, select the AWS region in which you want to purchase an offering, and then complete the reservation process\. If you purchase an offering programmatically, the region is determined by the Amazon Redshift endpoint that you connect to\. For more information about Amazon Redshift regions, go to [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#redshift_region) in the *Amazon Web Services General Reference*\.

To ensure that the discounted rate is applied to all of the nodes when you launch a cluster, make sure that the region, the node type, and the number of nodes that you select match one or more active reservations\. Otherwise, you'll be charged at the on\-demand rate for nodes that don’t match an active reservation\.

In a running cluster, if you exceed the number of nodes that you have reserved, you begin to accrue charges for those additional nodes at the on\-demand rate\. This accrual means that it is possible for you to be charged varying rates for nodes in the same cluster depending on how many nodes you’ve reserved\. You can purchase another offering to cover those additional nodes, and then the discounted rate is applied to those nodes for the remainder of the duration once the reservation status becomes **active**\.

If you resize your cluster into a different node type and you haven’t reserved nodes of that type, you’ll be charged at the on\-demand rate\. You can purchase another offering with the new node type if you want to receive discounted rates for your resized cluster\. However, you also continue to pay for the original reservation until its duration elapses\.  If you need to alter your reservations before the term expires, please create a support case using the [AWS Console](https://console.aws.amazon.com/support/home)\. 

### Reserved nodes and consolidated billing<a name="reserved-nodes-consolidated-billing"></a>

The pricing benefits of Reserved Nodes are shared when the purchasing account is part of a set of accounts billed under one consolidated billing payer account\. The hourly usage across all sub\-accounts is aggregated in the payer account every month\. This is typically useful for companies in which there are different functional teams or groups; then, the normal Reserved Nodes logic is applied to calculate the bill\. For more information, see [Consolidated Billing](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html) in the AWS Billing and Cost Management User Guide\.

### Reserved node examples<a name="reserved-node-examples"></a>

The scenarios in this section demonstrate how nodes accrue charges based on on\-demand and discounted rates using the following reservation details: 
+ Region: US West \(Oregon\)
+ Node Type: ds2\.xlarge
+ Payment Option: No Upfront
+ Duration: one year
+ Number of Reserved Nodes: 16

#### Example 1<a name="reserved-node-example-1"></a>

You have one ds2\.xlarge cluster in the US West \(Oregon\) region with 20 nodes\.

In this scenario, 16 of the nodes receive the discounted rate from the reservation, but the additional 4 nodes in the cluster are billed at the on\-demand rate\.

#### Example 2<a name="reserved-node-example-2"></a>

You have one ds2\.xlarge cluster in the US West \(Oregon\) region with 12 nodes\.

In this scenario, all 12 nodes in the cluster receive the discounted rate from the reservation\. However, you also pay for the remaining reserved nodes in the reservation even though you don't currently have a running cluster to which they apply\.

#### Example 3<a name="reserved-node-example-3"></a>

You have one ds2\.xlarge cluster in the US West \(Oregon\) region with 12 nodes\. You run the cluster for several months with this configuration, and then you need to add nodes to the cluster\. You resize the cluster, choosing the same node type and specifying a total of 16 nodes\.

In this scenario, you are billed the discounted rate for 16 nodes\. Your charges remain the same for the full year duration because the number of nodes that you have in the cluster is equal to the number of nodes that you have reserved\.

#### Example 4<a name="reserved-node-example-4"></a>

You have one ds2\.xlarge cluster in the US West \(Oregon\) region with 16 nodes\. You run the cluster for several months with this configuration, and then you need to add nodes\. You resize the cluster, choosing the same node type and specifying a total of 20 nodes\.

In this scenario, you are billed the discounted rate for all the nodes prior to the resize\. After the resize, you are billed the discounted rate for 16 of the nodes for the rest of the year, and you are billed at the on\-demand rate for the additional 4 nodes that you added to the cluster\.

#### Example 5<a name="reserved-node-example-5"></a>

You have two ds2\.xlarge clusters in the US West \(Oregon\) region\. One of the clusters has 6 nodes, and the other has 10 nodes\.

In this scenario, you are billed at the discounted rate for all of the nodes because the total number of nodes in both clusters is equal to the number of nodes that you have reserved\.

#### Example 6<a name="reserved-node-example-6"></a>

You have two ds2\.xlarge clusters in the US West \(Oregon\) region\. One of the clusters has 4 nodes, and the other has 6 nodes\.

In this scenario, you are billed the discounted rate for the 10 nodes that you have in running clusters, and you also pay the discounted rate for the additional 6 nodes that you have reserved even though you don't currently have any running clusters to which they apply\.