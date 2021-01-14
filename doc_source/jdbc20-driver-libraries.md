# Referencing the JDBC driver libraries<a name="jdbc20-driver-libraries"></a>

The JDBC application or Java code that you use to connect to your data must access the driver JAR files\. In the application or code, specify all the JAR files that you extracted from the ZIP archive\. 

## Using the driver in a JDBC application<a name="jdbc20-use-driver-jdbc-app"></a>

JDBC applications usually provide a set of configuration options for adding a list of driver library files\. Use the provided options to include all the JAR files from the ZIP archive as part of the driver configuration in the application\. For more information, see the documentation for your JDBC application\. 

## Using the driver in Java code<a name="jdbc20-use-driver-java-code"></a>

You must include all the driver library files in the class path\. This is the path that the Java Runtime Environment searches for classes and other resource files\. For more information, see "Setting the Class Path" in the appropriate Java SE documentation\.  
+ Windows: [https://docs\.oracle\.com/javase/7/docs/technotes/tools/windows/classpath\.html](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/classpath.html)
+ Linux and Solaris: [https://docs\.oracle\.com/javase/7/docs/technotes/tools/solaris/classpath\.html](https://docs.oracle.com/javase/7/docs/technotes/tools/solaris/classpath.html)
+ Text