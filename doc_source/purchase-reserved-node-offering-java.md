# Purchasing a Reserved Node Offering Using the AWS SDK for Java<a name="purchase-reserved-node-offering-java"></a>

The following example demonstrates how to use the AWS SDK for Java to do the following:
+ List existing reserved nodes\.
+ Search for a new reserved node offering based on specified node criteria\.
+ Purchase a reserved node\.

This example, first selects all the reserved node offerings that match a specified node type and fixed price value\. Then, this example goes through each offering found and lets you purchase the offering\. 

**Important**  
If you run this example and accept the offer to purchase a reserved node offering, you will be charged for the offering\.

For step\-by\-step instructions to run this example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. To get information about a node type and fixed price other than those listed, update the code and provide that node type and fixed price\.

**Example**  

```
import java.io.DataInput;
import java.io.DataInputStream;
import java.io.IOException;
import java.util.ArrayList;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.DescribeReservedNodeOfferingsRequest;
import com.amazonaws.services.redshift.model.DescribeReservedNodeOfferingsResult;
import com.amazonaws.services.redshift.model.DescribeReservedNodesResult;
import com.amazonaws.services.redshift.model.PurchaseReservedNodeOfferingRequest;
import com.amazonaws.services.redshift.model.ReservedNode;
import com.amazonaws.services.redshift.model.ReservedNodeAlreadyExistsException;
import com.amazonaws.services.redshift.model.ReservedNodeOffering;
import com.amazonaws.services.redshift.model.ReservedNodeOfferingNotFoundException;
import com.amazonaws.services.redshift.model.ReservedNodeQuotaExceededException;


public class ListAndPurchaseReservedNodeOffering {

    public static AmazonRedshiftClient client;
    public static String nodeTypeToPurchase = "ds1.xlarge";
    public static Double fixedPriceLimit = 10000.00;
    public static ArrayList<ReservedNodeOffering> matchingNodes = new ArrayList<ReservedNodeOffering>();
    
    public static void main(String[] args) throws IOException {
        

        AWSCredentials credentials = new PropertiesCredentials(
                ListAndPurchaseReservedNodeOffering.class
                .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
    
        try {
             listReservedNodes();
             findReservedNodeOffer();
             purchaseReservedNodeOffer();
             
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static void listReservedNodes() {
        DescribeReservedNodesResult result = client.describeReservedNodes();
        System.out.println("Listing nodes already purchased.");
        for (ReservedNode node : result.getReservedNodes()) {
            printReservedNodeDetails(node);
        }
    }

    private static void findReservedNodeOffer() 
    {
        DescribeReservedNodeOfferingsRequest request = new DescribeReservedNodeOfferingsRequest();
        DescribeReservedNodeOfferingsResult result = client.describeReservedNodeOfferings(request);            
        Integer count = 0;
        
        System.out.println("\nFinding nodes to purchase.");
        for (ReservedNodeOffering offering : result.getReservedNodeOfferings()) 
        {
            if (offering.getNodeType().equals(nodeTypeToPurchase)){
            
                if (offering.getFixedPrice() < fixedPriceLimit) {
                    matchingNodes.add(offering);
                    printOfferingDetails(offering);
                    count +=1;
                }
            }
        }
        if (count == 0) {
            System.out.println("\nNo reserved node offering matches found.");
        } else {
            System.out.println("\nFound " + count + " matches.");
        }    
    }

    private static void purchaseReservedNodeOffer() throws IOException {
        if (matchingNodes.size() == 0) {
            return;
        } else {
            System.out.println("\nPurchasing nodes.");

            for (ReservedNodeOffering offering : matchingNodes) {
                printOfferingDetails(offering);
                System.out.println("WARNING: purchasing this offering will incur costs.");
                System.out.println("Purchase this offering [Y or N]?");
                DataInput in = new DataInputStream(System.in);
                String purchaseOpt = in.readLine();
                if (purchaseOpt.equalsIgnoreCase("y")){
                    
                    try {
                        PurchaseReservedNodeOfferingRequest request = new PurchaseReservedNodeOfferingRequest()
                            .withReservedNodeOfferingId(offering.getReservedNodeOfferingId());
                        ReservedNode reservedNode = client.purchaseReservedNodeOffering(request);
                        printReservedNodeDetails(reservedNode);
                    }
                    catch (ReservedNodeAlreadyExistsException ex1){
                    }
                    catch (ReservedNodeOfferingNotFoundException ex2){
                    }
                    catch (ReservedNodeQuotaExceededException ex3){
                    }
                    catch (Exception ex4){
                    }
                }
            }
            System.out.println("Finished.");

        }
    }
    
    private static void printOfferingDetails(
            ReservedNodeOffering offering) {
        System.out.println("\nOffering Match:");
        System.out.format("Id: %s\n", offering.getReservedNodeOfferingId());
        System.out.format("Node Type: %s\n", offering.getNodeType());
        System.out.format("Fixed Price: %s\n", offering.getFixedPrice());
        System.out.format("Offering Type: %s\n", offering.getOfferingType());
        System.out.format("Duration: %s\n", offering.getDuration());        
    }
    
    private static void printReservedNodeDetails(ReservedNode node) {
        System.out.println("\nPurchased Node Details:");
        System.out.format("Id: %s\n", node.getReservedNodeOfferingId());
        System.out.format("State: %s\n", node.getState());
        System.out.format("Node Type: %s\n", node.getNodeType());
        System.out.format("Start Time: %s\n", node.getStartTime());
        System.out.format("Fixed Price: %s\n", node.getFixedPrice());
        System.out.format("Offering Type: %s\n", node.getOfferingType());
        System.out.format("Duration: %s\n", node.getDuration());                
    }
}
```