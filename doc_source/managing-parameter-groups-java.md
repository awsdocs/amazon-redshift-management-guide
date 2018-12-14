# Managing Parameter Groups Using the AWS SDK for Java<a name="managing-parameter-groups-java"></a>

This example demonstrates the following tasks related to parameter groups:
+ Creating a parameter group
+ Modifying a parameter group
+ Associating a parameter group with a cluster
+ Getting information about parameter groups

This example creates a new parameter group, `parametergroup1`, and makes the following updates:
+ Changes the parameter `extra_float_digits` to 2 from the default value of 0\. 
+ Replaces the existing workload management configuration \(`wlm_json_configuration` parameter\) with the following JSON which defines a queue in addition to the default queue\. 

  ```
  [
     {
        "user_group":[
           "example_user_group1"
        ],
        "query_group":[
           "example_query_group1"
        ],
        "query_concurrency":7
     },
     {
        "query_concurrency":5
     }
  ]
  ```

The preceding JSON is an array of two objects, one for each queue\. The first object defines a queue with specific user group and query group\. It also sets the concurrency level to 7\.

```
{
   "user_group":[
      "example_user_group1"
   ],
   "query_group":[
      "example_query_group1"
   ],
   "query_concurrency":7
}
```

Because this example replaces the WLM configuration, this JSON configuration also defines the default queue with no specific user group or query group\. It sets the concurrency to the default value, 5\. 

```
{
   "query_concurrency":5
}
```

For more information about Workload Management \(WML\) configuration, go to [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html)\. 

For step\-by\-step instructions to run the following example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and provide a cluster identifier\.

**Example**  

```
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.*;

public class CreateAndModifyClusterParameterGroup {

    public static AmazonRedshiftClient client;
    public static String clusterParameterGroupName = "parametergroup1";
    public static String clusterIdentifier = "***provide cluster identifier***";
    public static String parameterGroupFamily = "redshift-1.0";
        
    public static void main(String[] args) throws IOException {
    
        AWSCredentials credentials = new PropertiesCredentials(
                CreateAndModifyClusterParameterGroup.class
                        .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
        
        try {                       
             createClusterParameterGroup();   
             modifyClusterParameterGroup();
             associateParameterGroupWithCluster();
             describeClusterParameterGroups();
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static void createClusterParameterGroup() {
       CreateClusterParameterGroupRequest request = new CreateClusterParameterGroupRequest()
           .withDescription("my cluster parameter group")
           .withParameterGroupName(clusterParameterGroupName)
           .withParameterGroupFamily(parameterGroupFamily);      
       client.createClusterParameterGroup(request);
       System.out.println("Created cluster parameter group.");
    }
    
    private static void describeClusterParameterGroups() {          
       DescribeClusterParameterGroupsResult result = client.describeClusterParameterGroups();
       printResultClusterParameterGroups(result);
    }

    private static void modifyClusterParameterGroup() {
       List<Parameter> parameters = new ArrayList<Parameter>();
       parameters.add(new Parameter()
           .withParameterName("extra_float_digits")
           .withParameterValue("2"));
       // Replace WLM configuration. The new configuration defines a queue (in addition to the default).
       parameters.add(new Parameter()
       .withParameterName("wlm_json_configuration")  
       .withParameterValue("[{\"user_group\":[\"example_user_group1\"],\"query_group\":[\"example_query_group1\"],\"query_concurrency\":7},{\"query_concurrency\":5}]"));
       
       ModifyClusterParameterGroupRequest request = new ModifyClusterParameterGroupRequest()
           .withParameterGroupName(clusterParameterGroupName)
           .withParameters(parameters);
       client.modifyClusterParameterGroup(request);
       
    }

    private static void associateParameterGroupWithCluster() {

        ModifyClusterRequest request = new ModifyClusterRequest()
        .withClusterIdentifier(clusterIdentifier)
        .withClusterParameterGroupName(clusterParameterGroupName);
       
        Cluster result = client.modifyCluster(request);
        
        System.out.format("Parameter Group %s is used for Cluster %s\n", 
                clusterParameterGroupName, result.getClusterParameterGroups().get(0).getParameterGroupName());
    }
    private static void printResultClusterParameterGroups(DescribeClusterParameterGroupsResult result)
    {
        if (result == null)
        {
            System.out.println("\nDescribe cluster parameter groups result is null.");
            return;
        }

        System.out.println("\nPrinting parameter group results:\n");
        for (ClusterParameterGroup group : result.getParameterGroups()) {
            System.out.format("\nDescription: %s\n", group.getDescription());
            System.out.format("Group Family Name: %s\n", group.getParameterGroupFamily());
            System.out.format("Group Name: %s\n", group.getParameterGroupName());           
            describeClusterParameters(group.getParameterGroupName());
        }
    }
    
    private static void describeClusterParameters(String parameterGroupName) {
        DescribeClusterParametersRequest request = new DescribeClusterParametersRequest()
            .withParameterGroupName(parameterGroupName);
        
        DescribeClusterParametersResult result = client.describeClusterParameters(request);  
        printResultClusterParameters(result, parameterGroupName);
    }

    private static void printResultClusterParameters(DescribeClusterParametersResult result, String parameterGroupName)
    {
        if (result == null)
        {
            System.out.println("\nCluster parameters is null.");
            return;
        }

        System.out.format("\nPrinting cluster parameters for \"%s\"\n", parameterGroupName);
        for (Parameter parameter : result.getParameters()) {
            System.out.println("  Name: " + parameter.getParameterName() + ", Value: " + parameter.getParameterValue());
            System.out.println("  DataType: " + parameter.getDataType() + ", MinEngineVersion: " + parameter.getMinimumEngineVersion());
            System.out.println("  AllowedValues: " + parameter.getAllowedValues() + ", Source: " + parameter.getSource());
            System.out.println("  IsModifiable: " + parameter.getIsModifiable() + ", Description: " + parameter.getDescription());
        }
    }
}
```