:original_name: dws_04_0098.html

.. _dws_04_0098:

Example: Common Operations
==========================

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
   while (rs.next()) {
       System.out.print("a row was returned.");
   }
   rs.close();

   // Disable the server cursor.
   st.setFetchSize(0);
   rs = st.executeQuery("SELECT * FROM mytable");
   while (rs.next()) {
       System.out.print("many rows were returned.");
   }
   rs.close();

   // Close the statement.
   st.close();
