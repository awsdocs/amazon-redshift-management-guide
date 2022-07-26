# Managing snapshots using the AWS SDK for Java<a name="managing-snapshots-java"></a>

The following example demonstrates these common operations involving a snapshot:
+ Creating a manual cluster snapshot of a cluster\.
+ Displaying information about all the snapshots of a cluster\.
+ Deleting manual snapshots of a cluster\.

In this example, a snapshot of the cluster is initiated\. When the snapshot is successfully created, all manual snapshots for the cluster that were created before the new snapshot are deleted\. When creation of the manual snapshot is initiated, the snapshot is not immediately available\. Therefore, this example uses a loop to poll for the status of the snapshot by calling the `describeClusterSnapshot` method\. It normally takes a few moments for a snapshot to become available after initiation\. For more information about snapshots, see [Amazon Redshift snapshots](working-with-snapshots.md)\.

For step\-by\-step instructions to run the following example, see [Running Java examples for Amazon Redshift using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and provide a cluster identifier\. 

**Example**  

```
/**
 * Copyright 2010-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
 *
 * This file is licensed under the Apache License, Version 2.0 (the "License").
 * You may not use this file except in compliance with the License. A copy of
 * the License is located at
 *
 * http://aws.amazon.com/apache2.0/
 *
 * This file is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
 * CONDITIONS OF ANY KIND, either express or implied. See the License for the
 * specific language governing permissions and limitations under the License.
*/

// snippet-sourcedescription:[CreateAndDescribeSnapshot demonstrates how to create an Amazon Redshift cluster snapshot and describe existing snapshots.]
// snippet-service:[redshift]
// snippet-keyword:[Java]
// snippet-keyword:[Amazon Redshift]
// snippet-keyword:[Code Sample]
// snippet-keyword:[CreateClusterSnapshot]
// snippet-keyword:[DeleteClusterSnapshot]
// snippet-keyword:[DescribeClusterSnapshots]
// snippet-sourcetype:[full-example]
// snippet-sourcedate:[2019-01-30]
// snippet-sourceauthor:[AWS]
// snippet-start:[redshift.java.CreateAndDescribeSnapshot.complete]

package com.amazonaws.services.redshift;

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;

import com.amazonaws.services.redshift.model.*;

public class CreateAndDescribeSnapshot {

    public static AmazonRedshift client;
    public static String clusterIdentifier = "***provide a cluster identifier***";
    public static long sleepTime = 20;

    public static void main(String[] args) throws IOException {

         // Default client using the {@link com.amazonaws.auth.DefaultAWSCredentialsProviderChain}
        client = AmazonRedshiftClientBuilder.defaultClient();

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
// snippet-end:[redshift.java.CreateAndDescribeSnapshot.complete]
```