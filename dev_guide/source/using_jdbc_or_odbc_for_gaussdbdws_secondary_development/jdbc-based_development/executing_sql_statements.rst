:original_name: dws_04_0095.html

.. _dws_04_0095:

Executing SQL Statements
========================

Executing an Ordinary SQL Statement
-----------------------------------

The application performs data (parameter statements do not need to be transferred) in the database by running SQL statements, and you need to perform the following steps:

#. Create a statement object by triggering the createStatement method in Connection.

   ::

      Statement stmt = con.createStatement();

#. Execute the SQL statement by triggering the executeUpdate method in Statement.

   ::

      int rc = stmt.executeUpdate("CREATE TABLE customer_t1(c_customer_sk INTEGER, c_customer_name VARCHAR(32));");

   .. note::

      If an execution request (not in a transaction block) received in the database contains multiple statements, the request is packed into a transaction. **VACUUM** is not supported in a transaction block. If one of the statements fails, the entire request will be rolled back.

#. Close the statement object.

   ::

      stmt.close();

Executing a Prepared SQL Statement
----------------------------------

Pre-compiled statements were once complied and optimized and can have additional parameters for different usage. For the statements have been pre-compiled, the execution efficiency is greatly improved. If you want to execute a statement for several times, use a precompiled statement. Perform the following procedure:

#. Create a prepared statement object by calling the prepareStatement method in Connection.

   ::

      PreparedStatement pstmt = con.prepareStatement("UPDATE customer_t1 SET c_customer_name = ? WHERE c_customer_sk = 1");

#. Set parameters by triggering the setShort method in PreparedStatement.

   ::

      pstmt.setShort(1, (short)2);

#. Execute the precompiled SQL statement by triggering the executeUpdate method in PreparedStatement.

   ::

      int rowcount = pstmt.executeUpdate();

#. Close the precompiled statement object by calling the close method in PreparedStatement.

   ::

      pstmt.close();

Calling a Stored Procedure
--------------------------

Perform the following steps to call existing stored procedures through the JDBC interface in GaussDB(DWS):

#. Create a call statement object by calling the prepareCall method in Connection.

   ::

      CallableStatement cstmt = myConn.prepareCall("{? = CALL TESTPROC(?,?,?)}");

#. Set parameters by calling the setInt method in CallableStatement.

   ::

      cstmt.setInt(2, 50);
      cstmt.setInt(1, 20);
      cstmt.setInt(3, 90);

#. Register with an output parameter by calling the registerOutParameter method in CallableStatement.

   ::

      cstmt.registerOutParameter(4, Types.INTEGER);  //Register an OUT parameter as an integer.

#. Call the stored procedure by calling the execute method in CallableStatement.

   ::

      cstmt.execute();

#. Obtain the output parameter by calling the getInt method in CallableStatement.

   ::

      int out = cstmt.getInt(4);  //Obtain the OUT parameter.

   For example:

   ::

      //The following stored procedure has been created with the OUT parameter:
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

#. Close the call statement by calling the close method in CallableStatement.

   ::

      cstmt.close();

   .. note::

      -  Many database classes such as Connection, Statement, and ResultSet have a close() method. Close these classes after using their objects. Close these actions after using their objects. Closing Connection will close all the related Statements, and closing a Statement will close its ResultSet.
      -  Some JDBC drivers support named parameters, which can be used to set parameters by name rather than sequence. If a parameter has a default value, you do not need to specify any parameter value but can use the default value directly. Even though the parameter sequence changes during a stored procedure, the application does not need to be modified. Currently, the GaussDB(DWS) JDBC driver does not support this method.
      -  GaussDB(DWS) does not support functions containing OUT parameters, or default values of stored procedures and function parameters.

.. important::

   -  If JDBC is used to call a stored procedure whose returned value is a cursor, the returned cursor cannot be used.
   -  A stored procedure and an SQL statement must be executed separately.

Batch Processing
----------------

When a prepared statement batch processes multiple pieces of similar data, the database creates only one execution plan. This improves the compilation and optimization efficiency. Perform the following procedure:

#. Create a prepared statement object by calling the prepareStatement method in Connection.

   ::

      PreparedStatement pstmt = con.prepareStatement("INSERT INTO customer_t1 VALUES (?)");

#. Call the setShort parameter for each piece of data, and call addBatch to confirm that the setting is complete.

   ::

      pstmt.setShort(1, (short)2);
      pstmt.addBatch();

#. Execute batch processing by calling the executeBatch method in PreparedStatement.

   ::

      int[] rowcount = pstmt.executeBatch();

#. Close the precompiled statement object by calling the close method in PreparedStatement.

   ::

      pstmt.close();

   .. note::

      Do not terminate a batch processing action when it is ongoing; otherwise, the database performance will deteriorate. Therefore, disable the automatic submission function during batch processing, and manually submit every several lines. The statement for disabling automatic submission is **conn.setAutoCommit(false)**.
