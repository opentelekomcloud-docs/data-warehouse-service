:original_name: dws_04_0111.html

.. _dws_04_0111:

javax.sql.ConnectionPoolDataSource
==================================

This section describes **javax.sql.ConnectionPoolDataSource**, the interface for data source connection pools.

.. table:: **Table 1** Support status for javax.sql.ConnectionPoolDataSource

   +--------------------------------------------------+------------------+----------------+
   | Method Name                                      | Return Type      | Support JDBC 4 |
   +==================================================+==================+================+
   | getLoginTimeout()                                | int              | Yes            |
   +--------------------------------------------------+------------------+----------------+
   | getLogWriter()                                   | PrintWriter      | Yes            |
   +--------------------------------------------------+------------------+----------------+
   | getPooledConnection()                            | PooledConnection | Yes            |
   +--------------------------------------------------+------------------+----------------+
   | getPooledConnection(String user,String password) | PooledConnection | Yes            |
   +--------------------------------------------------+------------------+----------------+
   | setLoginTimeout(int seconds)                     | void             | Yes            |
   +--------------------------------------------------+------------------+----------------+
   | setLogWriter(PrintWriter out)                    | void             | Yes            |
   +--------------------------------------------------+------------------+----------------+
