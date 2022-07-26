# Connect to your cluster programmatically<a name="connecting-in-code"></a>

This section explains how to connect to your cluster programmatically\. If you are using an application like SQL Workbench/J that manages your client connections for you, then you can skip this section\.

## Connecting to a cluster by using Java<a name="connecting-in-code-java"></a>

When you use Java to programmatically connect to your cluster, you can do so with or without server authentication\. If you plan to use server authentication, follow the instructions in [Configuring security options for connections](connecting-ssl-support.md) to put the Amazon Redshift server certificate into a keystore\. You can refer to the keystore by specifying a property when you run your code as follows:

```
-Djavax.net.ssl.trustStore=<path to keystore>
-Djavax.net.ssl.trustStorePassword=<keystore password>
```

**Example : Connect to a cluster by using Java**  
The following example connects to a cluster and runs a sample query that returns system tables\. It is not necessary to have data in your database to use this example\.   
If you are using a server certificate to authenticate your cluster, you can restore the line that uses the keystore, which is commented out:   

```
props.setProperty("ssl", "true");
```
For more information about the server certificate, see [Configuring security options for connections](connecting-ssl-support.md)\.   
For step\-by\-step instructions to run the following example, see [Running Java examples for Amazon Redshift using Eclipse](using-aws-sdk-for-java.md#setting-up-and-testing-sdk-java)\.   

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

// snippet-sourcedescription:[ConnectToClusterExample demonstrates how to connect to an Amazon Redshift cluster and run a sample query.]
// snippet-service:[redshift]
// snippet-keyword:[Java]
// snippet-keyword:[Amazon Redshift]
// snippet-keyword:[Code Sample]
// snippet-keyword:[Connect]
// snippet-keyword:[JDBC]
// snippet-sourcetype:[full-example]
// snippet-sourcedate:[2019-02-01]
// snippet-sourceauthor:[AWS]
// snippet-start:[redshift.java.ConnectToCluster.complete]

package connection;

import java.sql.*;
import java.util.Properties;

public class ConnectToCluster {
    //Redshift driver: "jdbc:redshift://x.y.us-west-2.redshift.amazonaws.com:5439/dev";
    static final String dbURL = "***jdbc cluster connection string ****";
    static final String MasterUsername = "***master user name***";
    static final String MasterUserPassword = "***master user password***";

    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try{
           //Dynamically load driver at runtime.
           //Redshift JDBC 4.1 driver: com.amazon.redshift.jdbc41.Driver
           //Redshift JDBC 4 driver: com.amazon.redshift.jdbc4.Driver
           Class.forName("com.amazon.redshift.jdbc.Driver");

           //Open a connection and define properties.
           System.out.println("Connecting to database...");
           Properties props = new Properties();

           //Uncomment the following line if using a keystore.
           //props.setProperty("ssl", "true");
           props.setProperty("user", MasterUsername);
           props.setProperty("password", MasterUserPassword);
           conn = DriverManager.getConnection(dbURL, props);

           //Try a simple query.
           System.out.println("Listing system tables...");
           stmt = conn.createStatement();
           String sql;
           sql = "select * from information_schema.tables;";
           ResultSet rs = stmt.executeQuery(sql);

           //Get the data from the result set.
           while(rs.next()){
              //Retrieve two columns.
              String catalog = rs.getString("table_catalog");
              String name = rs.getString("table_name");

              //Display values.
              System.out.print("Catalog: " + catalog);
              System.out.println(", Name: " + name);
           }
           rs.close();
           stmt.close();
           conn.close();
        }catch(Exception ex){
           //For convenience, handle all errors here.
           ex.printStackTrace();
        }finally{
           //Finally block to close resources.
           try{
              if(stmt!=null)
                 stmt.close();
           }catch(Exception ex){
           }// nothing we can do
           try{
              if(conn!=null)
                 conn.close();
           }catch(Exception ex){
              ex.printStackTrace();
           }
        }
        System.out.println("Finished connectivity test.");
     }
}
// snippet-end:[redshift.java.ConnectToCluster.complete]
```

## Connecting to a cluster by using \.NET<a name="connecting-in-code-dotnet"></a>

When you use \.NET \(C\#\) to programmatically connect to your cluster, you can do so with or without server authentication\. If you plan to use server authentication, follow the instructions in [Configuring security options for connections](connecting-ssl-support.md) to download the Amazon Redshift server certificate, and then put the certificate in the correct form for your \.NET code\.

**Example Connect to a cluster by using \.NET**  
 The following example connects to a cluster and runs a sample query that returns system tables\. It does not show server authentication\. It is not necessary to have data in your database to use this example\. This example uses the [System\.Data\.Odbc namespace](https://msdn.microsoft.com/en-us/library/system.data.odbc.aspx), a \.NET Framework Data Provider for ODBC\.   

```
using System;
using System.Data;
using System.Data.Odbc;

namespace redshift.amazon.com.docsamples
{
    class ConnectToClusterExample
    {
        public static void Main(string[] args)
        {

            DataSet ds = new DataSet();
            DataTable dt = new DataTable();

            // Server, e.g. "examplecluster.xyz.us-west-2.redshift.amazonaws.com"
            string server = "***provide server name part of connection string****";

            // Port, e.g. "5439"
            string port = "***provide port***";

            // MasterUserName, e.g. "masteruser".
            string masterUsername = "***provide master user name***";

            // MasterUserPassword, e.g. "mypassword".
            string masterUserPassword = "***provide master user password***";

            // DBName, e.g. "dev"
            string DBName = "***provide name of database***";

            string query = "select * from information_schema.tables;";

            try
            {
                // Create the ODBC connection string.
                //Redshift ODBC Driver - 64 bits
                /*
                string connString = "Driver={Amazon Redshift (x64)};" +
                    String.Format("Server={0};Database={1};" +
                    "UID={2};PWD={3};Port={4};SSL=true;Sslmode=Require",
                    server, DBName, masterUsername,
                    masterUserPassword, port);
                */

                //Redshift ODBC Driver - 32 bits
                string connString = "Driver={Amazon Redshift (x86)};" +
                    String.Format("Server={0};Database={1};" +
                    "UID={2};PWD={3};Port={4};SSL=true;Sslmode=Require",
                    server, DBName, masterUsername,
                    masterUserPassword, port);

                // Make a connection using the psqlODBC provider.
                OdbcConnection conn = new OdbcConnection(connString);
                conn.Open();

                // Try a simple query.
                string sql = query;
                OdbcDataAdapter da = new OdbcDataAdapter(sql, conn);
                da.Fill(ds);
                dt = ds.Tables[0];
                foreach (DataRow row in dt.Rows)
                {
                    Console.WriteLine(row["table_catalog"] + ", " + row["table_name"]);
                }

                conn.Close();
                Console.ReadKey();
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine(ex.Message);
            }

        }
    }
}
```