# Client and driver are incompatible<a name="connecting-architecture-mismatch"></a>

 **Example error** 

 "The specified DSN contains an architecture mismatch between the Driver and Application\." 

 **Possible solution**

 When you attempt to connect and get an error about an architecture mismatch, this means that the client tool and the driver aren't compatible\. This occurs because their system architecture doesn't match\. For example, this can happen if you have a 32\-bit client tool but have installed the 64\-bit version of the driver\. Sometimes 64\-bit client tools can use 32\-bit drivers, but you can't use 32\-bit applications with 64\-bit drivers\. Make sure that the driver and client tool are using the same version of the system architecture\. 