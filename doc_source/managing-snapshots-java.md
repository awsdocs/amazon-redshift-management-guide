# Managing Snapshots Using the AWS SDK for Java<a name="managing-snapshots-java"></a>

The following example demonstrates these common operations involving a snapshot:

+ Creating a manual cluster snapshot of a cluster\.

+ Displaying information about all the snapshots of a cluster\.

+ Deleting manual snapshots of a cluster\.

In this example, a snapshot of the cluster is initiated\. When the snapshot is successfully created, all manual snapshots for the cluster that were created before the new snapshot are deleted\. When creation of the manual snapshot is initiated, the snapshot is not immediately available\. Therefore, this example uses a loop to poll for the status of the snapshot by calling the `describeClusterSnapshot` method\. It normally takes a few moments for a snapshot to become available after initiation\. For more information about snapshots, see [Amazon Redshift Snapshots](working-with-snapshots.md)\.

For step\-by\-step instructions to run the following example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and provide a cluster identifier\. 

**Example**  

```
import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.CreateClusterSnapshotRequest;
import com.amazonaws.services.redshift.model.DeleteClusterSnapshotRequest;
import com.amazonaws.services.redshift.model.DescribeClusterSnapshotsRequest;
import com.amazonaws.services.redshift.model.DescribeClusterSnapshotsResult;
import com.amazonaws.services.redshift.model.Snapshot;

public class CreateAndDescribeSnapshot {

    public static AmazonRedshiftClient client;
    public static String clusterIdentifier = "***provide cluster identifier***";
    public static long sleepTime = 10; 
        
    public static void main(String[] args) throws IOException {
    
        AWSCredentials credentials = new PropertiesCredentials(
                CreateAndDescribeSnapshot.class
                        .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
        
        try {
             // Unique snapshot identifier
             String snapshotId = "my-snapshot-" + (new SimpleDateFormat("yyyy-MM-dd-HH-mm-ss")).format(new Date());

             Date createDate = createManualSnapshot(snapshotId);   
             waitForSnapshotAvailable(snapshotId);
             describeSnapshots();
             deleteManualSnapshotsBefore(createDate);
             describeSnapshots();
             
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static Date createManualSnapshot(String snapshotId) {
        
        CreateClusterSnapshotRequest request = new CreateClusterSnapshotRequest()
        .withClusterIdentifier(clusterIdentifier)
        .withSnapshotIdentifier(snapshotId);
        Snapshot snapshot = client.createClusterSnapshot(request);
        System.out.format("Created cluster snapshot: %s\n", snapshotId);
        return snapshot.getSnapshotCreateTime();
    }
    
    private static void describeSnapshots() {

        DescribeClusterSnapshotsRequest request = new DescribeClusterSnapshotsRequest()
            .withClusterIdentifier(clusterIdentifier);
        DescribeClusterSnapshotsResult result = client.describeClusterSnapshots(request);
                
        printResultSnapshots(result);
    }

    private static void deleteManualSnapshotsBefore(Date creationDate) {
        
        DescribeClusterSnapshotsRequest request = new DescribeClusterSnapshotsRequest()
            .withEndTime(creationDate)
            .withClusterIdentifier(clusterIdentifier)
            .withSnapshotType("manual");
        
        DescribeClusterSnapshotsResult result = client.describeClusterSnapshots(request);
        
        for (Snapshot s : result.getSnapshots()) {
            DeleteClusterSnapshotRequest deleteRequest = new DeleteClusterSnapshotRequest()
            .withSnapshotIdentifier(s.getSnapshotIdentifier());
            Snapshot deleteResult = client.deleteClusterSnapshot(deleteRequest);
            System.out.format("Deleted snapshot %s\n", deleteResult.getSnapshotIdentifier());            
        }
    }
    
    private static void printResultSnapshots(DescribeClusterSnapshotsResult result) {
        System.out.println("\nSnapshot listing:");
        for (Snapshot snapshot : result.getSnapshots()) {
            System.out.format("Identifier: %s\n", snapshot.getSnapshotIdentifier());
            System.out.format("Snapshot type: %s\n", snapshot.getSnapshotType());
            System.out.format("Snapshot create time: %s\n", snapshot.getSnapshotCreateTime());
            System.out.format("Snapshot status: %s\n\n", snapshot.getStatus());
        }
    }
    
    private static Boolean waitForSnapshotAvailable(String snapshotId) throws InterruptedException {
        Boolean snapshotAvailable = false;
        System.out.println("Wating for snapshot to become available.");
        while (!snapshotAvailable) {
            DescribeClusterSnapshotsResult result = client.describeClusterSnapshots(new DescribeClusterSnapshotsRequest()
                .withSnapshotIdentifier(snapshotId));
            String status = (result.getSnapshots()).get(0).getStatus();
            if (status.equalsIgnoreCase("available")) {
                snapshotAvailable = true;
            }
            else {
                System.out.print(".");
                Thread.sleep(sleepTime*1000);
            }
        }
        return snapshotAvailable;
    }

}
```