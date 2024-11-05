:original_name: dws_04_0099.html

.. _dws_04_0099:

Example: Retrying SQL Queries for Applications
==============================================

If the primary DN is faulty and cannot be restored within 40s, its standby is automatically promoted to primary to ensure the normal running of the cluster. Jobs running during the failover will fail and those started after the failover will not be affected. To protect upper-layer services from being affected by the failover, refer to the following example to construct a SQL retry mechanism at the service layer.

::

   //gsjdbc4.jar is used as an example.
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

     //Run an ordinary SQL statement. Create a jdbc_test1 table.
      public static void CreateTable(Connection conn) {
        Statement stmt = null;
        try {
          stmt = conn.createStatement();

          // add ctrl+c handler
          Runtime.getRuntime().addShutdownHook(new ExitHandler(stmt));

         // Run an ordinary SQL statement.
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

     //Run the preprocessing statement to insert data in batches.
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
         //Perform batch processing.
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

         System.out.println("finesh......");
        return retValue;
      }

   //Run a query statement and retry upon a failure. The number of retry times can be configured.
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

       //Disconnect from the database.
        try {
          conn.close();
        } catch (SQLException e) {
          e.printStackTrace();
        }

      }

    }
