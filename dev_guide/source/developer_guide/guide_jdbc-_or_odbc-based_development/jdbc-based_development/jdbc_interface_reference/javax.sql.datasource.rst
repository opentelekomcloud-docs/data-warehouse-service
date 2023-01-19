:original_name: dws_04_0112.html

.. _dws_04_0112:

javax.sql.DataSource
====================

This section describes **javax.sql.DataSource**, the interface for data sources.

.. table:: **Table 1** Support status for javax.sql.DataSource

   +------------------------------------------------+-------------+----------------+
   | Method Name                                    | Return Type | Support JDBC 4 |
   +================================================+=============+================+
   | getConneciton()                                | Connection  | Yes            |
   +------------------------------------------------+-------------+----------------+
   | getConnection(String username,String password) | Connection  | Yes            |
   +------------------------------------------------+-------------+----------------+
   | getLoginTimeout()                              | int         | Yes            |
   +------------------------------------------------+-------------+----------------+
   | getLogWriter()                                 | PrintWriter | Yes            |
   +------------------------------------------------+-------------+----------------+
   | setLoginTimeout(int seconds)                   | void        | Yes            |
   +------------------------------------------------+-------------+----------------+
   | setLogWriter(PrintWriter out)                  | void        | Yes            |
   +------------------------------------------------+-------------+----------------+
