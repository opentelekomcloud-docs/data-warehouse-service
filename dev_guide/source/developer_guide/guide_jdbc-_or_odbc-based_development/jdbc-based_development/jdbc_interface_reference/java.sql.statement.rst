:original_name: dws_04_0110.html

.. _dws_04_0110:

java.sql.Statement
==================

This section describes **java.sql.Statement**, the interface for executing SQL statements.

.. table:: **Table 1** Support status for java.sql.Statement

   ============================ =========== ==============
   Method Name                  Return Type Support JDBC 4
   ============================ =========== ==============
   close()                      void        Yes
   execute(String sql)          boolean     Yes
   executeQuery(String sql)     ResultSet   Yes
   executeUpdate(String sql)    int         Yes
   getConnection()              Connection  Yes
   getResultSet()               ResultSet   Yes
   getQueryTimeout()            int         Yes
   getUpdateCount()             int         Yes
   isClosed()                   boolean     Yes
   setQueryTimeout(int seconds) void        Yes
   setFetchSize(int rows)       void        Yes
   cancel()                     void        Yes
   ============================ =========== ==============

.. note::

   Using setFetchSize can reduce the memory occupied by result sets on the client. Result sets are packaged into cursors and segmented for processing, which will increase the communication traffic between the database and the client, affecting performance.

   Database cursors are valid only within their transaction. If **setFetchSize** is set, set **setAutoCommit(false)** and commit transactions on the connection to flush service data to a database.
