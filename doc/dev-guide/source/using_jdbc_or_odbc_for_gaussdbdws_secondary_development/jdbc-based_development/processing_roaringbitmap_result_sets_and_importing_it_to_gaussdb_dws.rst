:original_name: dws_04_10001.html

.. _dws_04_10001:

Processing RoaringBitmap Result Sets and Importing It to GaussDB (DWS)
======================================================================

GaussDB(DWS) 8.1.3 and later versions support the RoaringBitmap function. When using the Java language to perform secondary development based on GaussDB(DWS), you can use the CopyManager interface to import a small amount of RoaringBitmap data to GaussDB(DWS).

.. note::

   To import a large amount of RoaringBitmap data, computing power of the application side needs to be increased. Otherwise, the import performance will be affected.

Processing RoaringBitmap Data
-----------------------------

#. Visit `Maven <https://mvnrepository.com/artifact/org.roaringbitmap/RoaringBitmap>`__ to download the open-source RoaringBitmap JAR package. Version 0.9.15 is recommended.

   The dependency items of the POM file are configured as follows:

   ::

      <dependencies>
       <dependency>
       <groupId>org.roaringbitmap</groupId>
       <artifactId>RoaringBitmap</artifactId>
       <version>0.9.15</version>
       </dependency>
       </dependencies>

   |image1|

#. Invoke the JAR package to convert data to the RoaringBitmap type.

   The general process is to declare a Roaring bitmap, call the add() method to convert data of the int type into the Roaringbitmap type, and then serialize the converted data. The sample code is as follows:

   ::

      RoaringBitmap rr2 = new RoaringBitmap ();
      for (int i = 1; i < 10000000;  i++)  {
            rr2.add(i);
      }
      ByteArrayOutputStream a = new ByteArrayOutputStream();
      DataOutputStream b = new DataOutputStream(a);
      rr2.serialize(b);

Data Import
-----------

Invoke CopyManager to import data to the database. In this way, a small amount of RoaringBitmap data can be imported to the database without having to be stored locally.

::

   //gsjdbc4.jar is used as an example.

   package rb_demo;

   import org.postgresql.copy.CopyManager;
   import org.postgresql.core.BaseConnection;
   import org.roaringbitmap.RoaringBitmap;

   import java.io.ByteArrayInputStream;
   import java.io.ByteArrayOutputStream;
   import java.io.DataOutputStream;
   import java.io.IOException;
   import java.io.InputStream;
   import java.io.StringReader;
   import java.sql.Connection;
   import java.sql.DriverManager;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.SQLException;
   import java.sql.Statement;

   public class rb_demo {

       private static String hexStr =  "0123456789ABCDEF";

       public static String bytesToHex(byte[] bytes) {
           StringBuffer sb = new StringBuffer();
           for (int i = 0; i < bytes.length; i++) {
               String hex = Integer.toHexString(bytes[i] & 0xFF);
               if (hex.length() < 2) {
                   sb.append(0);
               }
               sb.append(hex);
           }
           return sb.toString();
       }

       public static Connection GetConnection(String username, String passwd) {
           String driver = "org.postgresql.Driver";
   String sourceURL = "jdbc:postgresql://10.185.180.161: 8000/gaussdb"; //Database URL
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

       public static void main(String[] args) throws IOException {

           RoaringBitmap rr2 = new RoaringBitmap();

           for (int i = 1; i < 10000000; i++) {
               rr2.add(i);
           }

           ByteArrayOutputStream a = new ByteArrayOutputStream();

           DataOutputStream b = new DataOutputStream(a);
           rr2.serialize(b);

   Connection conn = GetConnection("test", "Gauss_234"); //User name and password.
           Statement pstmt = null;
           try {
               conn.setAutoCommit(true);
               pstmt = conn.createStatement();

               pstmt.execute("drop table if exists t_rb");
               pstmt.execute("create table t_rb(c1 int, c2 roaringbitmap) distribute by hash (c1);");

               StringReader sr = null;
               CopyManager cm = null;
               cm = new CopyManager((BaseConnection) conn);

               String delimiter = "|";
               StringBuffer tuples = new StringBuffer();
               tuples.append("1" + delimiter + "\\x" + bytesToHex(a.toByteArray()));


               StringBuffer sb = new StringBuffer();
               sb.append(tuples.toString());

               sr = new StringReader(tuples.toString());
               String sql = "copy t_rb from STDIN with (delimiter '|', NOESCAPING)";

   long rows = cm.copyIn(sql, sr);//Execute the COPY command to save data to the database.

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
   }

.. |image1| image:: /_static/images/en-us_image_0000001764383893.png
