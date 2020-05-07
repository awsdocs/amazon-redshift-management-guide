# Connecting from outside of Amazon EC2—firewall timeout issue<a name="connecting-firewall-guidance"></a>

## Example issue<a name="connecting-firewall-guidance.Issue"></a>

 Your client connection to the database appears to hang or timeout when running long queries, such as a COPY command\. In this case, you might observe that the Amazon Redshift console displays that the query has completed, but the client tool itself still appears to be running the query\. The results of the query might be missing or incomplete depending on when the connection stopped\. 

## Possible solutions<a name="connecting-firewall-guidance.Solutions"></a>

This issue happens when you connect to Amazon Redshift from a machine other than an Amazon EC2 instance\. In this case, idle connections are terminated by an intermediate network component, such as a firewall, after a period of inactivity\. This behavior is typical when you log on from a virtual private network \(VPN\) or your local network\. 

To avoid these timeouts, we recommend the following changes:
+ Increase client system values that deal with TCP/IP timeouts\. Make these changes on the computer you are using to connect to your cluster\. The timeout period should be adjusted for your client and network\. For more information, see [Change TCP/IP timeout settings](#connecting-firewall-guidance.change-tcpip-settings)\.
+ Optionally, set keepalive behavior at the DSN level\. For more information, see [Change DSN timeout settings](#connecting-firewall-guidance.change-dsn-settings)\.

## Change TCP/IP timeout settings<a name="connecting-firewall-guidance.change-tcpip-settings"></a>

To change TCP/IP timeout settings, configure the timeout settings according to the operating system that you use to connect to your cluster\. 
+ Linux — If your client is running on Linux, run the following command as the root user to change the timeout settings for the current session: 

  ```
  /sbin/sysctl -w net.ipv4.tcp_keepalive_time=200 net.ipv4.tcp_keepalive_intvl=200 net.ipv4.tcp_keepalive_probes=5
  ```

  To persist the settings, create or modify the file `/etc/sysctl.conf` with the following values then reboot your system\. 

  ```
  net.ipv4.tcp_keepalive_time=200
  net.ipv4.tcp_keepalive_intvl=200
  net.ipv4.tcp_keepalive_probes=5
  ```
+ Windows — If your client runs on Windows, edit the values for the following registry settings under HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\Tcpip\\Parameters\\: 
  + KeepAliveTime: 30000
  + KeepAliveInterval: 1000
  + TcpMaxDataRetransmissions: 10

  These settings use the DWORD data type\. If they do not exist under the registry path, you can create the settings and specify these recommended values\. For more information about editing the Windows registry, refer to Windows documentation\. 

  After you set these values, restart your computer for the changes to take effect\. 
+ Mac — If your client is running on a Mac, run the following commands to change the timeout settings for the current session: 

  ```
  sudo sysctl net.inet.tcp.keepintvl=200000
  sudo sysctl net.inet.tcp.keepidle=200000
  sudo sysctl net.inet.tcp.keepinit=200000
  sudo sysctl net.inet.tcp.always_keepalive=1
  ```

  To persist the settings, create or modify the file `/etc/sysctl.conf` with the following values: 

  ```
  net.inet.tcp.keepidle=200000
  net.inet.tcp.keepintvl=200000
  net.inet.tcp.keepinit=200000
  net.inet.tcp.always_keepalive=1
  ```

  Restart your computer, and then run the following commands to verify that the values are set\. 

  ```
  sysctl net.inet.tcp.keepidle
  sysctl net.inet.tcp.keepintvl
  sysctl net.inet.tcp.keepinit
  sysctl net.inet.tcp.always_keepalive
  ```

## Change DSN timeout settings<a name="connecting-firewall-guidance.change-dsn-settings"></a>

You can set keepalive behavior at the DSN level if you choose\. You do this by adding or modifying the following parameters in the odbc\.ini file: 

**KeepAlivesCount**  
The number of TCP keepalive packets that can be lost before the connection is considered broken\.

**KeepAlivesIdle**  
The number of seconds of inactivity before the driver sends a TCP keepalive packet\.

**KeepAlivesInterval**  
The number of seconds between each TCP keepalive retransmission\.

On Windows, you modify these parameters in the registry by adding or changing keys in HKEY\_LOCAL\_MACHINE\\SOFTWARE\\ODBC\\ODBC\.INI\\*your\_DSN*\. On Linux and macOS, you add or modify these parameters in the target DSN entry directly in the odbc\.ini file\. For more information on modifying the odbc\.ini file on Linux and macOS computers, see [Configure the ODBC driver on Linux and macOS X operating systems](configure-odbc-connection.md#odbc-driver-configure-linux-mac)\. 

If these parameters don't exist, or if they have a value of 0, the system uses the keepalive parameters specified for TCP/IP to determine DSN keepalive behavior\. On Windows, you can find the TCP/IP parameters in the registry in `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\`\. On Linux and macOS, you can find the TCP/IP parameters can be found in the sysctl\.conf file\. 