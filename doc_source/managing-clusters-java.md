# Managing Clusters Using the AWS SDK for Java<a name="managing-clusters-java"></a>

The following Java code example demonstrates common cluster management operations including:

+ Creating a cluster\.

+ Listing metadata about a cluster\.

+ Modifying configuration options\.

After you initiate the request for the cluster to be created, you must wait until the cluster is in the `available` state before you can modify it\. This example uses a loop to periodically check the status of the cluster using the `describeClusters` method\. When the cluster is available, the preferred maintenance window for the cluster is changed\.

For step\-by\-step instructions to run the following example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and specify a cluster identifier\.

**Example**  

```
import java.io.IOException;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.*;

public class CreateAndModifyCluster {

    public static AmazonRedshiftClient client;
    public static String clusterIdentifier = "***provide a cluster identifier***";
    public static long sleepTime = 20; 
    
    public static void main(String[] args) throws IOException {

        AWSCredentials credentials = new PropertiesCredentials(
                CreateAndModifyCluster.class
                        .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
        
        try {            
             createCluster();   
             waitForClusterReady();
             describeClusters();
             modifyCluster();
             describeClusters();
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static void createCluster() {
       CreateClusterRequest request = new CreateClusterRequest()
          .withClusterIdentifier(clusterIdentifier)
          .withMasterUsername("masteruser")
          .withMasterUserPassword("12345678Aa")
          .withNodeType("ds1.xlarge")
          .withNumberOfNodes(2);          
          
       Cluster createResponse = client.createCluster(request);
       System.out.println("Created cluster " + createResponse.getClusterIdentifier());
    }
    
    private static void describeClusters() {
       DescribeClustersRequest request = new DescribeClustersRequest()
          .withClusterIdentifier(clusterIdentifier);
          
       DescribeClustersResult result = client.describeClusters(request);
       printResult(result);
    }

    private static void modifyCluster() {
       ModifyClusterRequest request = new ModifyClusterRequest()
          .withClusterIdentifier(clusterIdentifier)
          .withPreferredMaintenanceWindow("wed:07:30-wed:08:00");
          
       client.modifyCluster(request);
       System.out.println("Modified cluster " + clusterIdentifier);
       
    }
    
    private static void printResult(DescribeClustersResult result)
    {
        if (result == null)
        {
            System.out.println("Describe clusters result is null.");
            return;
        }

        System.out.println("Cluster property:");
        System.out.format("Preferred Maintenance Window: %s\n", result.getClusters().get(0).getPreferredMaintenanceWindow());
    }
    
    private static void waitForClusterReady() throws InterruptedException {
        Boolean clusterReady = false;
        System.out.println("Wating for cluster to become available.");
        while (!clusterReady) {
            DescribeClustersResult result = client.describeClusters(new DescribeClustersRequest()
                .withClusterIdentifier(clusterIdentifier));
            String status = (result.getClusters()).get(0).getClusterStatus();
            if (status.equalsIgnoreCase("available")) {
                clusterReady = true;
            }
            else {
                System.out.print(".");
                Thread.sleep(sleepTime*1000);
            }
        }
    }
}
```