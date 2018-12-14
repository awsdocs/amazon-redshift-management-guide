# Troubleshooting Connection Issues in Amazon Redshift<a name="troubleshooting-connections"></a>

 If you have issues with connecting to your cluster from a SQL client tool, there are several things that you can check to narrow down the problem\. If you are using SSL or server certificates, first remove this complexity while you troubleshoot the connection issue\. Then add this back when you have found a solution\. For more information, see [Configure Security Options for Connections](connecting-ssl-support.md)\. 

**Important**  
Amazon Redshift has changed the way that we manage SSL certificates\. If you have trouble connecting using SSL, you might need to update your current trust root CA certificates\. For more information, see [Transitioning to ACM Certificates for SSL Connections](connecting-transitioning-to-acm-certs.md)\.

 The following section has some example error messages and possible solutions for connection issues\. Because different SQL client tools provide different error messages, this is not a complete list, but should be a good starting point for troubleshooting issues\. 

**Topics**
+ [Connecting from Outside of Amazon EC2 —Firewall Timeout Issue](connecting-firewall-guidance.md)
+ [The Connection Is Refused or Fails](connecting-refusal-failure-issues.md)
+ [The Client and Driver Are Incompatible](connecting-architecture-mismatch.md)
+ [Queries Appear to Hang and Sometimes Fail to Reach the Cluster](connecting-drop-issues.md)