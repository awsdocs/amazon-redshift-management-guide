# Managing Cluster Security Groups Using the AWS SDK for Java<a name="managing-security-groups-java"></a>

The following example demonstrates common operations on cluster security groups, including:

+ Creating a new cluster security group\.

+ Adding ingress rules to a cluster security group\.

+ Associating a cluster security group with a cluster by modifying the cluster configuration\.

By default, when a new cluster security group is created, it has no ingress rules\. This example modifies a new cluster security group by adding two ingress rules\. One ingress rule is added by specifying a CIDR/IP range; the other is added by specifying an owner ID and Amazon EC2 security group combination\.

For step\-by\-step instructions to run the following example, see [Running Java Examples for Amazon Redshift Using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\. You need to update the code and provide a cluster identifier and AWS account number\.

**Example**  

```
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.PropertiesCredentials;
import com.amazonaws.services.redshift.AmazonRedshiftClient;
import com.amazonaws.services.redshift.model.*;

public class CreateAndModifyClusterSecurityGroup {

    public static AmazonRedshiftClient client;
    public static String clusterSecurityGroupName = "securitygroup1";
    public static String clusterIdentifier = "***provide cluster identifier***";
    public static String ownerID = "***provide account id****";  
        
    public static void main(String[] args) throws IOException {
            
        AWSCredentials credentials = new PropertiesCredentials(
                CreateAndModifyClusterSecurityGroup.class
                        .getResourceAsStream("AwsCredentials.properties"));
    
        client = new AmazonRedshiftClient(credentials);
        
        try {            
             createClusterSecurityGroup();   
             describeClusterSecurityGroups();
             addIngressRules();
             associateSecurityGroupWithCluster();
        } catch (Exception e) {
            System.err.println("Operation failed: " + e.getMessage());
        }
    }

    private static void createClusterSecurityGroup() {
        CreateClusterSecurityGroupRequest request = new CreateClusterSecurityGroupRequest()
        .withDescription("my cluster security group")
        .withClusterSecurityGroupName(clusterSecurityGroupName);
        
        client.createClusterSecurityGroup(request);
        System.out.format("Created cluster security group: '%s'\n", clusterSecurityGroupName);
    }
    
    private static void addIngressRules() {

        AuthorizeClusterSecurityGroupIngressRequest request = new AuthorizeClusterSecurityGroupIngressRequest()
            .withClusterSecurityGroupName(clusterSecurityGroupName)
            .withCIDRIP("192.168.40.5/32");
            
        ClusterSecurityGroup result = client.authorizeClusterSecurityGroupIngress(request);
        
        request = new AuthorizeClusterSecurityGroupIngressRequest()
            .withClusterSecurityGroupName(clusterSecurityGroupName)
            .withEC2SecurityGroupName("default")
            .withEC2SecurityGroupOwnerId(ownerID);        
        result = client.authorizeClusterSecurityGroupIngress(request);
        System.out.format("\nAdded ingress rules to security group '%s'\n", clusterSecurityGroupName);
        printResultSecurityGroup(result);
    }
    
    private static void associateSecurityGroupWithCluster() {

        // Get existing security groups used by the cluster.
        DescribeClustersRequest request = new DescribeClustersRequest()
        .withClusterIdentifier(clusterIdentifier);
  
        DescribeClustersResult result = client.describeClusters(request);
        List<ClusterSecurityGroupMembership> membershipList = 
            result.getClusters().get(0).getClusterSecurityGroups();

        List<String> secGroupNames = new ArrayList<String>();
        for (ClusterSecurityGroupMembership mem : membershipList) {
            secGroupNames.add(mem.getClusterSecurityGroupName());
        }
        // Add new security group to the list.
        secGroupNames.add(clusterSecurityGroupName);
        
        // Apply the change to the cluster.
        ModifyClusterRequest request2 = new ModifyClusterRequest()
        .withClusterIdentifier(clusterIdentifier)
        .withClusterSecurityGroups(secGroupNames);        
       
        Cluster result2 = client.modifyCluster(request2);
        System.out.format("\nAssociated security group '%s' to cluster '%s'.", clusterSecurityGroupName, clusterIdentifier);
   }

    private static void describeClusterSecurityGroups() {
        DescribeClusterSecurityGroupsRequest request = new DescribeClusterSecurityGroupsRequest();
       
        DescribeClusterSecurityGroupsResult result = client.describeClusterSecurityGroups(request);
        printResultSecurityGroups(result.getClusterSecurityGroups());
    }

    private static void printResultSecurityGroups(List<ClusterSecurityGroup> groups)
    {
        if (groups == null)
        {
            System.out.println("\nDescribe cluster security groups result is null.");
            return;
        }

        System.out.println("\nPrinting security group results:");
        for (ClusterSecurityGroup group : groups)
        {
            printResultSecurityGroup(group);
        }        
    }
    private static void printResultSecurityGroup(ClusterSecurityGroup group) {
        System.out.format("\nName: '%s', Description: '%s'\n", group.getClusterSecurityGroupName(), group.getDescription());
        for (EC2SecurityGroup g : group.getEC2SecurityGroups()) {
            System.out.format("EC2group: '%s', '%s', '%s'\n", g.getEC2SecurityGroupName(), g.getEC2SecurityGroupOwnerId(), g.getStatus());
        }
        for (IPRange range : group.getIPRanges()) {
            System.out.format("IPRanges: '%s', '%s'\n", range.getCIDRIP(), range.getStatus());

        }        
    }
}
```