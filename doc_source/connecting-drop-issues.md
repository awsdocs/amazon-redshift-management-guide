# Queries appear to hang and sometimes fail to reach the cluster<a name="connecting-drop-issues"></a>

**Example issue**

You experience an issue with queries completing, where the queries appear to be running but hang in the SQL client tool\. Sometimes the queries fail to appear in the cluster, such as in system tables or the Amazon Redshift console\. 

**Possible solution**

 This issue can happen due to packet drop\. In this case, there is a difference in the maximum transmission unit \(MTU\) size in the network path between two Internet Protocol \(IP\) hosts\. The MTU size determines the maximum size, in bytes, of a packet that can be transferred in one Ethernet frame over a network connection\. In AWS, some Amazon EC2 instance types support an MTU of 1500 \(Ethernet v2 frames\) and other instance types support an MTU of 9001 \(TCP/IP jumbo frames\)\. 

 To avoid issues that can occur with differences in MTU size, we recommend doing one of the following: 
+ If your cluster uses the EC2\-VPC platform, configure the Amazon VPC security group with an inbound custom Internet Control Message Protocol \(ICMP\) rule that returns `Destination Unreachable`\. The rule thus instructs the originating host to use the lowest MTU size along the network path\. For details on this approach, see [Configuring security groups to allow ICMP "destination unreachable"](#configure-custom-icmp)\. 
+ If your cluster uses the EC2\-Classic platform, or you can't allow the ICMP inbound rule, disable TCP/IP jumbo frames so that Ethernet v2 frames are used\. For details on this approach, see [Configuring the MTU of an instance](#set-mtu)\.

## Configuring security groups to allow ICMP "destination unreachable"<a name="configure-custom-icmp"></a>

 When there is a difference in the MTU size in the network between two hosts, first make sure that your network settings don't block path MTU discovery \(PMTUD\)\. PMTUD enables the receiving host to respond to the originating host with the following ICMP message: `Destination Unreachable: fragmentation needed and DF set (ICMP Type 3, Code 4)`\. This message instructs the originating host to use the lowest MTU size along the network path to resend the request\. Without this negotiation, packet drop can occur because the request is too large for the receiving host to accept\. For more information about this ICMP message, go to [RFC792](http://tools.ietf.org/html/rfc792) on the *Internet Engineering Task Force \(IETF\)* website\. 

 If you don't explicitly configure this ICMP inbound rule for your Amazon VPC security group, PMTUD is blocked\. In AWS, security groups are virtual firewalls that specify rules for inbound and outbound traffic to an instance\. For information about Amazon Redshift cluster security group, see [Amazon Redshift cluster security groups](working-with-security-groups.md)\. For clusters using the EC2\-VPC platform, Amazon Redshift uses VPC security groups to allow or deny traffic to the cluster\. By default, the security groups are locked down and deny all inbound traffic\. For information about how to set inbound and outbound rules for EC2\-Classic or EC2\-VPC instances, see [Differences between instances in EC2\-Classic and a VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-classic-platform.html#ec2_classic_platform) in the *Amazon EC2 User Guide for Linux Instances*\.

 For more information about how to add rules to VPC security groups, see [Managing VPC security groups for a cluster](managing-vpc-security-groups.md)\. For more information about specific PMTUD settings required in this rule, see [Path MTU discovery](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/network_mtu.html#path_mtu_discovery) in the *Amazon EC2 User Guide for Linux Instances\.* 

## Configuring the MTU of an instance<a name="set-mtu"></a>

In some cases, your cluster might use the EC2\-Classic platform or you can't allow the custom ICMP rule for inbound traffic\. In these cases, we recommend that you adjust the MTU to 1500 on the network interface \(NIC\) of the EC2 instances you connect to your Amazon Redshift cluster from\. This adjustment disables TCP/IP jumbo frames to ensure that connections consistently use the same packet size\. However, this option reduces your maximum network throughput for the instance entirely, not just for connections to Amazon Redshift\. For more information, see the following procedures\. <a name="set-mtu-win-os"></a>

**To set MTU on a Microsoft Windows operating system**

If your client runs in a Microsoft Windows operating system, you can review and set the MTU value for the Ethernet adapter by using the `netsh` command\. 

1. Run the following command to determine the current MTU value: 

   ```
   netsh interface ipv4 show subinterfaces
   ```

1.  Review the `MTU` value for the `Ethernet` adapter in the output\. 

1. If the value is not `1500`, run the following command to set it: 

   ```
   netsh interface ipv4 set subinterface "Ethernet" mtu=1500 store=persistent
   ```

   After you set this value, restart your computer for the changes to take effect\.<a name="set-mtu-linux-os"></a>

**To set MTU on a Linux operating system**

 If your client runs in a Linux operating system, you can review and set the MTU value by using the `ip` command\. 

1. Run the following command to determine the current MTU value: 

   ```
   $ ip link show eth0
   ```

1. Review the value following `mtu` in the output\. 

1. If the value is not `1500`, run the following command to set it: 

   ```
   $ sudo ip link set dev eth0 mtu 1500
   ```<a name="set-mtu-mac-os"></a>

**To set MTU on a Mac operating system**
+ To set the MTU on a Mac operating system, follow the instructions in [macOS X 10\.4 or later: How to change the MTU for troubleshooting purposes](https://support.apple.com/kb/ht2532)\.