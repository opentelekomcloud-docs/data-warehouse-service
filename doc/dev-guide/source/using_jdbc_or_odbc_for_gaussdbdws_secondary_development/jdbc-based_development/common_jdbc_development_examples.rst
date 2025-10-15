:original_name: dws_04_0098.html

.. _dws_04_0098:

Common JDBC Development Examples
================================

Example 1
---------

Before completing the following example, you need to create a stored procedure.

::

   create or replace procedure testproc
   (
       psv_in1 in integer,
       psv_in2 in integer,
       psv_inout in out integer
   )
   as
   begin
       psv_inout := psv_in1 + psv_in2 + psv_inout;
   end;
   /

This example illustrates how to develop applications based on the GaussDB(DWS) JDBC interface.

::

   //DBtest.java
   //gsjdbc4.jar is used as an example.
   // This example illustrates the main processes of JDBC-based development, covering database connection creation, table creation, and data insertion.

   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.SQLException;
   import java.sql.Statement;
   import java.sql.CallableStatement;

   public class DBTest {

     //Establish a connection to the database.
     public static Connection GetConnection(String username, String passwd) {
       String driver = "org.postgresql.Driver";
       String sourceURL = "jdbc:postgresql://localhost:/gaussdb";
       Connection conn = null;
       try {
         //Load the database driver.
         Class.forName(driver).newInstance();
       } catch (Exception e) {
         e.printStackTrace();
         return null;
       }

       try {
         //Establish a connection to the database.
         conn = DriverManager.getConnection(sourceURL, username, passwd);
         System.out.println("Connection succeed!");
       } catch (Exception e) {
         e.printStackTrace();
         return null;
       }

       return conn;
     };

     //Run an ordinary SQL statement. Create a customer_t1 table.
     public static void CreateTable(Connection conn) {
       Statement stmt = null;
       try {
         stmt = conn.createStatement();

         //Run an ordinary SQL statement.
         int rc = stmt
             .executeUpdate("CREATE TABLE customer_t1(c_customer_sk INTEGER, c_customer_name VARCHAR(32));");

         stmt.close();
       } catch (SQLException e) {
         if (stmt != null) {
           try {
             stmt.close();
           } catch (SQLException e1) {
             e1.printStackTrace();
           }
         }
         e.printStackTrace();
       }
     }

     //Run the preprocessing statement to insert data in batches.
     public static void BatchInsertData(Connection conn) {
       PreparedStatement pst = null;

       try {
         //Generate a prepared statement.
         pst = conn.prepareStatement("INSERT INTO customer_t1 VALUES (?,?)");
         for (int i = 0; i < 3; i++) {
           //Add parameters.
           pst.setInt(1, i);
           pst.setString(2, "data " + i);
           pst.addBatch();
         }
         //Run batch processing.
         pst.executeBatch();
         pst.close();
       } catch (SQLException e) {
         if (pst != null) {
           try {
             pst.close();
           } catch (SQLException e1) {
           e1.printStackTrace();
           }
         }
         e.printStackTrace();
       }
     }

     //Run the precompilation statement to update data.
     public static void ExecPreparedSQL(Connection conn) {
       PreparedStatement pstmt = null;
       try {
         pstmt = conn
             .prepareStatement("UPDATE customer_t1 SET c_customer_name = ? WHERE c_customer_sk = 1");
         pstmt.setString(1, "new Data");
         int rowcount = pstmt.executeUpdate();
         pstmt.close();
       } catch (SQLException e) {
         if (pstmt != null) {
           try {
             pstmt.close();
           } catch (SQLException e1) {
             e1.printStackTrace();
           }
         }
         e.printStackTrace();
       }
     }


   //Run a stored procedure.
     public static void ExecCallableSQL(Connection conn) {
       CallableStatement cstmt = null;
       try {

         cstmt=conn.prepareCall("{? = CALL TESTPROC(?,?,?)}");
         cstmt.setInt(2, 50);
         cstmt.setInt(1, 20);
         cstmt.setInt(3, 90);
          cstmt.registerOutParameter(4, Types.INTEGER);  //Register an OUT parameter as an integer.
         cstmt.execute();
         int out = cstmt.getInt(4);  //Obtain the out parameter value.
         System.out.println("The CallableStatment TESTPROC returns:"+out);
         cstmt.close();
       } catch (SQLException e) {
         if (cstmt != null) {
           try {
             cstmt.close();
           } catch (SQLException e1) {
             e1.printStackTrace();
           }
         }
         e.printStackTrace();
       }
     }


     /**
      * Main process. Call static methods one by one.
      * @param args
     */
     public static void main(String[] args) {
       //Establish a connection to the database.
       Connection conn = GetConnection("tester", "password");

       //Create a table.
       CreateTable(conn);

       //Insert data in batches.
       BatchInsertData(conn);

     //Run the precompilation statement to update data.
       ExecPreparedSQL(conn);

       //Run a stored procedure.
       ExecCallableSQL(conn);

       //Close the connection to the database.
       try {
         conn.close();
       } catch (SQLException e) {
         e.printStackTrace();
       }

     }

   }

Example 2: High Client Memory Usage
-----------------------------------

In this example, **setFetchSize** adjusts the memory usage of the client by using the database cursor to obtain server data in batches. It may increase network interaction and damage some performance.

The cursor is valid within a transaction. Therefore, you need to disable the autocommit function.

::

   // Disable the autocommit function.
   conn.setAutoCommit(false);
   Statement st = conn.createStatement();

   // Open the cursor and obtain 50 lines of data each time.
   st.setFetchSize(50);
   ResultSet rs = st.executeQuery("SELECT * FROM mytable");
   while (rs.next()){
       System.out.print("a row was returned.");
   }
   rs.close();

   // Disable the server cursor.
   st.setFetchSize(0);
   rs = st.executeQuery("SELECT * FROM mytable");
   while (rs.next()){
       System.out.print("many rows were returned.");
   }
   rs.close();

   // Close the statement.
   st.close();

Retrying SQL Queries for Applications
-------------------------------------

If the primary DN is faulty and cannot be restored within 40 seconds, its standby is automatically promoted to primary to ensure that the cluster runs properly. Jobs running during the switchover will fail and those started after the switchover will not be affected. To protect upper-layer services from being affected by the failover, refer to the following example to construct a SQL retry mechanism at the service layer.

**gsjdbc4.jar** is used as an example.

::

   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;

   /**
    *
    *
    */

   class ExitHandler extends Thread {
       private Statement cancel_stmt = null;

       public ExitHandler(Statement stmt) {
           super("Exit Handler");
           this.cancel_stmt = stmt;
       }
       public void run() {
           System.out.println("exit handle");
           try {
               this.cancel_stmt.cancel();
           } catch (SQLException e) {
               System.out.println("cancel query failed.");
               e.printStackTrace();
           }
       }
   }

   public class SQLRetry {
     //Establish a connection to the database.
      public static Connection GetConnection(String username, String passwd) {
        String driver = "org.postgresql.Driver";
        String sourceURL = "jdbc:postgresql://10.131.72.136:8000/gaussdb";
        Connection conn = null;
        try {
         //Load the database driver.
          Class.forName(driver).newInstance();
        } catch (Exception e) {
          e.printStackTrace();
          return null;
        }

        try {
         //Establish a connection to the database.
          conn = DriverManager.getConnection(sourceURL, username, passwd);
          System.out.println("Connection succeed!");
        } catch (Exception e) {
          e.printStackTrace();
          return null;
        }

        return conn;
   }

Run an ordinary SQL statement. Create the **jdbc_test1** table.

::

   public static void CreateTable(Connection conn) {
        Statement stmt = null;
        try {
          stmt = conn.createStatement();

          // add ctrl+c handler
          Runtime.getRuntime().addShutdownHook(new ExitHandler(stmt));

         //Run an ordinary SQL statement.
          int rc2 = stmt
             .executeUpdate("DROP TABLE if exists jdbc_test1;");

          int rc1 = stmt
             .executeUpdate("CREATE TABLE jdbc_test1(col1 INTEGER, col2 VARCHAR(10));");

          stmt.close();
        } catch (SQLException e) {
          if (stmt != null) {
            try {
              stmt.close();
            } catch (SQLException e1) {
              e1.printStackTrace();
            }
          }
          e.printStackTrace();
        }
      }

Run the preprocessing statement to insert data in batches.

::

   public static void BatchInsertData(Connection conn) {
        PreparedStatement pst = null;

        try {
         //Generate a prepared statement.
          pst = conn.prepareStatement("INSERT INTO jdbc_test1 VALUES (?,?)");
          for (int i = 0; i < 100; i++) {
           //Add parameters.
            pst.setInt(1, i);
            pst.setString(2, "data " + i);
            pst.addBatch();
          }
         //Run batch processing.
          pst.executeBatch();
          pst.close();
        } catch (SQLException e) {
          if (pst != null) {
            try {
              pst.close();
            } catch (SQLException e1) {
            e1.printStackTrace();
            }
          }
          e.printStackTrace();
        }
      }


Run the precompilation statement to update data.

::

    private static boolean QueryRedo(Connection conn){
        PreparedStatement pstmt = null;
        boolean retValue = false;
        try {
          pstmt = conn
              .prepareStatement("SELECT col1 FROM jdbc_test1 WHERE col2 = ?");

              pstmt.setString(1, "data 10");
              ResultSet rs = pstmt.executeQuery();

              while (rs.next()) {
                  System.out.println("col1 = " + rs.getString("col1"));
              }
              rs.close();

          pstmt.close();
           retValue = true;
         } catch (SQLException e) {
          System.out.println("catch...... retValue " + retValue);
          if (pstmt != null) {
            try {
             pstmt.close();
           } catch (SQLException e1) {
             e1.printStackTrace();
            }
          }
          e.printStackTrace();
        }

         System.out.println("finish......");
        return retValue;
      }


Run a query statement and retry upon a failure. The number of retry times can be configured.

::

    public static void ExecPreparedSQL(Connection conn) throws InterruptedException {
            int maxRetryTime = 50;
            int time = 0;
            String result = null;
            do {
                time++;
                try {
     System.out.println("time:" + time);
     boolean ret = QueryRedo(conn);
     if(ret == false){
      System.out.println("retry, time:" + time);
      Thread.sleep(10000);
      QueryRedo(conn);
     }
                } catch (Exception e) {
                    e.printStackTrace();
                }
            } while (null == result && time < maxRetryTime);

      }

      /**
      * Main process. Call static methods one by one.
       * @param args
     * @throws InterruptedException
      */
      public static void main(String[] args) throws InterruptedException {
     //Establish a connection to the database.
        Connection conn = GetConnection("testuser", "test@123");

       //Create a table.
        CreateTable(conn);

       //Insert data in batches.
        BatchInsertData(conn);

     //Run the precompilation statement to update data.
        ExecPreparedSQL(conn);

       //Close the connection to the database.
        try {
          conn.close();
        } catch (SQLException e) {
          e.printStackTrace();
        }

      }

    }

Importing and Exporting Data Through Local Files
------------------------------------------------

When the JAVA language is used for secondary development based on GaussDB(DWS), you can use the CopyManager interface to export data from the database to a local file or import a local file to the database by streaming. The file can be in CSV or TEXT format.

The sample program is as follows. Load the GaussDB(DWS) JDBC driver before running it.

**gsjdbc4.jar** is used as an example.

::

   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.io.IOException;
   import java.io.FileInputStream;
   import java.io.FileOutputStream;
   import java.sql.SQLException;
   import org.postgresql.copy.CopyManager;
   import org.postgresql.core.BaseConnection;

   public class Copy{

        public static void main(String[] args)
        {
         String urls = new String("jdbc:postgresql://10.180.155.74:8000/gaussdb"); //URL of the database
         String username = new String("jack");            //Username
         String password = new String("********");       //Password
         String tablename = new String("migration_table"); //Define table information.
         String tablename1 = new String("migration_table_1"); //Define table information.
         String driver = "org.postgresql.Driver";
         Connection conn = null;

         try {
               Class.forName(driver);
               conn = DriverManager.getConnection(urls, username, password);
             } catch (ClassNotFoundException e) {
                  e.printStackTrace(System.out);
             } catch (SQLException e) {
                  e.printStackTrace(System.out);
             }


Import and export data.

::

         //Export the query result of migration_table to the local file d:/data.txt.
         try {
        copyToFile(conn, "d:/data.txt", "(SELECT * FROM migration_table)");
      } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
      } catch (IOException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
      }
         //Import data from the d:/data.txt file to the migration_table_1 table.
         try {
         copyFromFile(conn, "d:/data.txt", migration_table_1);
      } catch (SQLException e) {
     // TODO Auto-generated catch block
            e.printStackTrace();
    } catch (IOException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }

         //Export the data from the migration_table_1 table to the d:/data1.txt file.
         try {
         copyToFile(conn, "d:/data1.txt", migration_table_1);
      } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
      } catch (IOException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
        }

     public static void copyFromFile(Connection connection, String filePath, String tableName)
            throws SQLException, IOException {

        FileInputStream fileInputStream = null;

        try {
            CopyManager copyManager = new CopyManager((BaseConnection)connection);
            fileInputStream = new FileInputStream(filePath);
            copyManager.copyIn("COPY " + tableName + " FROM STDIN", fileInputStream);
        } finally {
            if (fileInputStream != null) {
                try {
                    fileInputStream.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

     public static void copyToFile(Connection connection, String filePath, String tableOrQuery)
             throws SQLException, IOException {

         FileOutputStream fileOutputStream = null;

         try {
             CopyManager copyManager = new CopyManager((BaseConnection)connection);
             fileOutputStream = new FileOutputStream(filePath);
             copyManager.copyOut("COPY " + tableOrQuery + " TO STDOUT", fileOutputStream);
         } finally {
             if (fileOutputStream != null) {
                 try {
                     fileOutputStream.close();
                 } catch (IOException e) {
                     e.printStackTrace();
                 }
             }
         }
     }
   }

Migrating Data from MySQL to GaussDB(DWS)
-----------------------------------------

The following example shows how to use CopyManager to migrate data from MySQL to GaussDB(DWS).

**gsjdbc4.jar** is used as an example.

::

   import java.io.StringReader;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;

   import org.postgresql.copy.CopyManager;
   import org.postgresql.core.BaseConnection;

   public class Migration{

       public static void main(String[] args) {
           String url = new String("jdbc:postgresql://10.180.155.74:8000/gaussdb"); //URL of the database
           String user = new String("jack");            //GaussDB(DWS) username
           String pass = new String("********");             //GaussDB(DWS) password
           String tablename = new String("migration_table"); //Define table information.
           String delimiter = new String("|");              //Define a delimiter.
           String encoding = new String("UTF8");            //Define a character set.
           String driver = "org.postgresql.Driver";
           StringBuffer buffer = new StringBuffer();       //Define the buffer to store formatted data.

           try {
               //Obtain the query result set of the source database.
               ResultSet rs = getDataSet();

               //Traverse the result set and obtain records row by row.
               //The values of columns in each record are separated by the specified delimiter and end with a newline character to form strings.
               ////Add the strings to the buffer.
               while (rs.next()) {
                   buffer.append(rs.getString(1) + delimiter
                           + rs.getString(2) + delimiter
                           + rs.getString(3) + delimiter
                           + rs.getString(4)
                           + "\n");
               }
               rs.close();

               try {
                   //Connect to the target database.
                   Class.forName(driver);
                   Connection conn = DriverManager.getConnection(url, user, pass);
                   BaseConnection baseConn = (BaseConnection) conn;
                   baseConn.setAutoCommit(false);

                   //Initialize table information.
                   String sql = "Copy " + tablename + " from STDIN DELIMITER " + "'" + delimiter + "'" + " ENCODING " + "'" + encoding + "'";

                   //Submit data in the buffer.
                   CopyManager cp = new CopyManager(baseConn);
                   StringReader reader = new StringReader(buffer.toString());
                   cp.copyIn(sql, reader);
                   baseConn.commit();
                   reader.close();
                   baseConn.close();
               } catch (ClassNotFoundException e) {
                   e.printStackTrace(System.out);
               } catch (SQLException e) {
                   e.printStackTrace(System.out);
               }

           } catch (Exception e) {
               e.printStackTrace();
           }
       }



Return the query result from the source database.

.. code-block::

       private static ResultSet getDataSet() {
           ResultSet rs = null;
           try {
               Class.forName("com.mysql.jdbc.Driver").newInstance();
               Connection conn = DriverManager.getConnection("jdbc:mysql://10.119.179.227:3306/jack?useSSL=false&allowPublicKeyRetrieval=true", "jack", "********");
               Statement stmt = conn.createStatement();
               rs = stmt.executeQuery("select * from migration_table");
           } catch (SQLException e) {
               e.printStackTrace();
           } catch (Exception e) {
               e.printStackTrace();
           }
           return rs;
       }
   }
