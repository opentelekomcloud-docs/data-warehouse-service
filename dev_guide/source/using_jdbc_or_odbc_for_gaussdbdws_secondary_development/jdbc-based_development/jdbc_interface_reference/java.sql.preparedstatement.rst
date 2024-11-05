:original_name: dws_04_0107.html

.. _dws_04_0107:

java.sql.PreparedStatement
==========================

This section describes **java.sql.PreparedStatement**, the interface for preparing statements.

.. table:: **Table 1** Support status for java.sql.PreparedStatement

   +-------------------------------------------------+-------------------+----------------+
   | Method Name                                     | Return Type       | Support JDBC 4 |
   +=================================================+===================+================+
   | clearParameters()                               | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | execute()                                       | boolean           | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | executeQuery()                                  | ResultSet         | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | excuteUpdate()                                  | int               | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | getMetaData()                                   | ResultSetMetaData | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setBoolean(int parameterIndex, boolean x)       | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setBigDecimal(int parameterIndex, BigDecimal x) | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setByte(int parameterIndex, byte x)             | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setBytes(int parameterIndex, byte[] x)          | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setDate(int parameterIndex, Date x)             | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setDouble(int parameterIndex, double x)         | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setFloat(int parameterIndex, float x)           | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setInt(int parameterIndex, int x)               | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setLong(int parameterIndex, long x)             | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setNString(int parameterIndex, String value)    | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setShort(int parameterIndex, short x)           | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | setString(int parameterIndex, String x)         | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | addBatch()                                      | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | executeBatch()                                  | int[]             | Yes            |
   +-------------------------------------------------+-------------------+----------------+
   | clearBatch()                                    | void              | Yes            |
   +-------------------------------------------------+-------------------+----------------+

.. note::

   -  Execute addBatch() and execute() only after running clearBatch().
   -  Batch is not cleared by calling executeBatch(). Clear batch by explicitly calling clearBatch().
   -  After bounded variables of a batch are added, if you want to reuse these values (add a batch again), set*() is not necessary.
   -  The following methods are inherited from java.sql.Statement: close, execute, executeQuery, executeUpdate, getConnection, getResultSet, getUpdateCount, isClosed, setMaxRows, and setFetchSize.
