:original_name: dws_04_0103.html

.. _dws_04_0103:

java.sql.Connection
===================

This section describes **java.sql.Connection**, the interface for connecting to a database.

.. table:: **Table 1** Support status for java.sql.Connection

   ======================================= ================= ==============
   Method Name                             Return Type       Support JDBC 4
   ======================================= ================= ==============
   close()                                 void              Yes
   commit()                                void              Yes
   createStatement()                       Statement         Yes
   getAutoCommit()                         boolean           Yes
   getClientInfo()                         Properties        Yes
   getClientInfo(String name)              String            Yes
   getTransactionIsolation()               int               Yes
   isClosed()                              boolean           Yes
   isReadOnly()                            boolean           Yes
   prepareStatement(String sql)            PreparedStatement Yes
   rollback()                              void              Yes
   setAutoCommit(boolean autoCommit)       void              Yes
   setClientInfo(Properties properties)    void              Yes
   setClientInfo(String name,String value) void              Yes
   ======================================= ================= ==============

.. important::

   The AutoCommit mode is used by default within the interface. If you disable it running **setAutoCommit(false)**, all the statements executed later will be packaged in explicit transactions, and you cannot execute statements that cannot be executed within transactions.
