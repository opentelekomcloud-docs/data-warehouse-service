:original_name: dws_04_0113.html

.. _dws_04_0113:

javax.sql.PooledConnection
==========================

This section describes **javax.sql.PooledConnection**, the connection interface created by a connection pool.

.. table:: **Table 1** Support status for javax.sql.PooledConnection

   +------------------------------------------------------------------+-------------+----------------+
   | Method Name                                                      | Return Type | Support JDBC 4 |
   +==================================================================+=============+================+
   | addConnectionEventListener (ConnectionEventListener listener)    | void        | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
   | close()                                                          | void        | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
   | getConnection()                                                  | Connection  | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
   | removeConnectionEventListener (ConnectionEventListener listener) | void        | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
   | addStatementEventListener (StatementEventListener listener)      | void        | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
   | removeStatementEventListener (StatementEventListener listener)   | void        | Yes            |
   +------------------------------------------------------------------+-------------+----------------+
