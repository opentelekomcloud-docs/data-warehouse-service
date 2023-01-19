:original_name: dws_04_0101.html

.. _dws_04_0101:

Example: Migrating Data from MySQL to GaussDB(DWS)
==================================================

The following example shows how to use CopyManager to migrate data from MySQL to GaussDB(DWS).

::

   //gsjdbc4.jar is used as an example.
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
           String url = new String("jdbc:postgresql://10.180.155.74:8000/gaussdb"); //Database URL
           String user = new String("jack");            //Database username
           String pass = new String("********");             //Database password
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

       //********************************
       //Return the query result set from the source database.
       //*********************************
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
