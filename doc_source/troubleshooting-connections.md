# Troubleshooting connection issues in Amazon Redshift<a name="troubleshooting-connections"></a>

 If you have issues with connecting to your cluster from a SQL client tool, there are several things that you can check to narrow down the problem\. If you are using SSL or server certificates, first remove this complexity while you troubleshoot the connection issue\. Then add this back when you have found a solution\. For more information, see [Configuring security options for connections](connecting-ssl-support.md)\. 

**Important**  
Amazon Redshift has changed the way that SSL certificates are managed\. If you have trouble connecting using SSL, you might need to update your current trust root CA certificates\. For more information, see [Transitioning to ACM certificates for SSL connections](connecting-transitioning-to-acm-certs.md)\.

 The following section has some example error messages and possible solutions for connection issues\. Because different SQL client tools provide different error messages, this is not a complete list, but should be a good starting point for troubleshooting issues\. 

**Topics**
+ [Connecting from outside of Amazon EC2—firewall timeout issue](connecting-firewall-guidance.md)
+ [Connection is refused or fails](connecting-refusal-failure-issues.md)
+ [Client and driver are incompatible](connecting-architecture-mismatch.md)
+ [Queries appear to hang and sometimes fail to reach the cluster](connecting-drop-issues.md)