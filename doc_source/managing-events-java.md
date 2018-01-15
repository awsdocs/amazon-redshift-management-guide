# Viewing Events Using the AWS SDK for Java<a name="managing-events-java"></a>

The following example lists the events for a specified cluster and specified event source type\. The example shows how to use pagination\. 

For step\-by\-step instructions to run the following example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and specify a cluster identifier and event source type\.

**Example**  

```
import java.io.IOException;
import java.util.Date;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.*;

public class ListEvents {

    public static AmazonRedshiftClient client;
    public static String clusterIdentifier = "***provide cluster identifier***";
    public static String eventSourceType = "***provide source type***"; // e.g. cluster-snapshot
    
    public static void main(String[] args) throws IOException {
        
        AWSCredentials credentials = new PropertiesCredentials(
                ListEvents.class
                .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
        
        try {            
             listEvents(); 
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static void listEvents() {
        long oneWeeksAgoMilli = (new Date()).getTime() - (7L*24L*60L*60L*1000L);
        Date oneWeekAgo = new Date();
        oneWeekAgo.setTime(oneWeeksAgoMilli);
        String marker = null;
        
        do {
            DescribeEventsRequest request = new DescribeEventsRequest()
            .withSourceIdentifier(clusterIdentifier)
            .withSourceType(eventSourceType)
            .withStartTime(oneWeekAgo)
            .withMaxRecords(20);
            DescribeEventsResult result = client.describeEvents(request);
            marker = result.getMarker();
            for (Event event : result.getEvents()) {
                printEvent(event);
            }        
        } while (marker != null);
        
        
    }
    static void printEvent(Event event)
    {
        if (event == null)
        {
            System.out.println("\nEvent object is null.");
            return;
        }

        System.out.println("\nEvent metadata:\n");
        System.out.format("SourceID: %s\n", event.getSourceIdentifier());
        System.out.format("Type: %s\n", event.getSourceType());
        System.out.format("Message: %s\n", event.getMessage());
        System.out.format("Date: %s\n", event.getDate());
    }
}
```