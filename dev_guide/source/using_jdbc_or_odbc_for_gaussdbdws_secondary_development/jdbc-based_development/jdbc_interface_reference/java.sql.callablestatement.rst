:original_name: dws_04_0104.html

.. _dws_04_0104:

java.sql.CallableStatement
==========================

This section describes **java.sql.CallableStatement**, the stored procedure execution interface.

.. table:: **Table 1** Support status for java.sql.CallableStatement

   +----------------------------------------------------+-------------+----------------+
   | Method Name                                        | Return Type | Support JDBC 4 |
   +====================================================+=============+================+
   | registerOutParameter(int parameterIndex, int type) | void        | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | wasNull()                                          | boolean     | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getString(int parameterIndex)                      | String      | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getBoolean(int parameterIndex)                     | boolean     | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getByte(int parameterIndex)                        | byte        | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getShort(int parameterIndex)                       | short       | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getInt(int parameterIndex)                         | int         | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getLong(int parameterIndex)                        | long        | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getFloat(int parameterIndex)                       | float       | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getDouble(int parameterIndex)                      | double      | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getBigDecimal(int parameterIndex)                  | BigDecimal  | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getBytes(int parameterIndex)                       | byte[]      | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getDate(int parameterIndex)                        | Date        | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getTime(int parameterIndex)                        | Time        | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getTimestamp(int parameterIndex)                   | Timestamp   | Yes            |
   +----------------------------------------------------+-------------+----------------+
   | getObject(int parameterIndex)                      | Object      | Yes            |
   +----------------------------------------------------+-------------+----------------+

.. note::

   -  The batch operation of statements containing OUT parameter is not allowed.
   -  The following methods are inherited from java.sql.Statement: close, execute, executeQuery, executeUpdate, getConnection, getResultSet, getUpdateCount, isClosed, setMaxRows, and setFetchSize.
   -  The following methods are inherited from java.sql.PreparedStatement: addBatch, clearParameters, execute, executeQuery, executeUpdate, getMetaData, setBigDecimal, setBoolean, setByte, setBytes, setDate, setDouble, setFloat, setInt, setLong, setNull, setObject, setString, setTime, and setTimestamp.
