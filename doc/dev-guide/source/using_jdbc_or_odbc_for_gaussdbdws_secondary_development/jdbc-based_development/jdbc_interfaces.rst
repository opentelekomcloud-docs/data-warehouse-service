:original_name: dws_04_0102.html

.. _dws_04_0102:

JDBC Interfaces
===============

JDBC interface is a set of API methods for users. This section describes some common interfaces. For other interfaces, see information in JDK1.6 (software package) and JDBC 4.0.

java.sql.Connection
-------------------

This section describes **java.sql.Connection**, the interface for connecting to a database.

.. table:: **Table 1** java.sql.Connection methods

   +-----------------------------------------+-------------------+-----------------------+
   | Method                                  | Return Type       | Support JDBC 4 or Not |
   +=========================================+===================+=======================+
   | close()                                 | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | commit()                                | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | createStatement()                       | Statement         | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | getAutoCommit()                         | boolean           | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | getClientInfo()                         | Properties        | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | getClientInfo(String name)              | String            | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | getTransactionIsolation()               | int               | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | isClosed()                              | boolean           | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | isReadOnly()                            | boolean           | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | prepareStatement(String sql)            | PreparedStatement | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | rollback()                              | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | setAutoCommit(boolean autoCommit)       | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | setClientInfo(Properties properties)    | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+
   | setClientInfo(String name,String value) | void              | Yes                   |
   +-----------------------------------------+-------------------+-----------------------+

.. important::

   The interface uses the AutoCommit mode by default, but you can disable it by setting **setAutoCommit** to **false**. This will package all subsequent statements in explicit transactions. Note that you will not be able to execute statements that cannot be executed within transactions.

java.sql.CallableStatement
--------------------------

This section describes **java.sql.CallableStatement**, the stored procedure execution interface.

.. table:: **Table 2** java.sql.CallableStatement methods

   +----------------------------------------------------+-------------+-----------------------+
   | Method                                             | Return Type | Support JDBC 4 or Not |
   +====================================================+=============+=======================+
   | registerOutParameter(int parameterIndex, int type) | void        | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | wasNull()                                          | boolean     | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getString(int parameterIndex)                      | String      | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getBoolean(int parameterIndex)                     | boolean     | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getByte(int parameterIndex)                        | byte        | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getShort(int parameterIndex)                       | short       | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getInt(int parameterIndex)                         | int         | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getLong(int parameterIndex)                        | long        | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getFloat(int parameterIndex)                       | float       | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getDouble(int parameterIndex)                      | double      | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getBigDecimal(int parameterIndex)                  | BigDecimal  | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getBytes(int parameterIndex)                       | byte[]      | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getDate(int parameterIndex)                        | Date        | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getTime(int parameterIndex)                        | Time        | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getTimestamp(int parameterIndex)                   | Timestamp   | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+
   | getObject(int parameterIndex)                      | Object      | Yes                   |
   +----------------------------------------------------+-------------+-----------------------+

.. note::

   -  Do not perform batch operations on statements containing **OUT** parameters.
   -  The following methods are inherited from **java.sql.Statement**: **close**, **execute**, **executeQuery**, **executeUpdate**, **getConnection**, **getResultSet**, **getUpdateCount**, **isClosed**, **setMaxRows**, and **setFetchSize**.
   -  The following methods are inherited from **java.sql.PreparedStatement**: **addBatch**, **clearParameters**, **execute**, **executeQuery**, **executeUpdate**, **getMetaData**, **setBigDecimal**, **setBoolean**, **setByte**, **setBytes**, **setDate**, **setDouble**, **setFloat**, **setInt**, **setLong**, **setNull**, **setObject**, **setString**, **setTime**, and **setTimestamp**.

java.sql.DatabaseMetaData
-------------------------

This section describes **java.sql.DatabaseMetaData**, the interface for defining database objects.

.. table:: **Table 3** java.sql.DatabaseMetaData methods

   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | Method                                                                                              | Return Type | Support JDBC 4 or Not |
   +=====================================================================================================+=============+=======================+
   | getTables(String catalog, String schemaPattern, String tableNamePattern, String[] types)            | ResultSet   | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getColumns(String catalog, String schemaPattern, String tableNamePattern, String columnNamePattern) | ResultSet   | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getTableTypes()                                                                                     | ResultSet   | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getUserName()                                                                                       | String      | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | isReadOnly()                                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | nullsAreSortedHigh()                                                                                | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | nullsAreSortedLow()                                                                                 | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | nullsAreSortedAtStart()                                                                             | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | nullsAreSortedAtEnd()                                                                               | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDatabaseProductName()                                                                            | String      | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDatabaseProductVersion()                                                                         | String      | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDriverName()                                                                                     | String      | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDriverVersion()                                                                                  | String      | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDriverMajorVersion()                                                                             | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDriverMinorVersion()                                                                             | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | usesLocalFiles()                                                                                    | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | usesLocalFilePerTable()                                                                             | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsMixedCaseIdentifiers()                                                                      | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | storesUpperCaseIdentifiers()                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | storesLowerCaseIdentifiers()                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsMixedCaseQuotedIdentifiers()                                                                | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | storesUpperCaseQuotedIdentifiers()                                                                  | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | storesLowerCaseQuotedIdentifiers()                                                                  | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | storesMixedCaseQuotedIdentifiers()                                                                  | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsAlterTableWithAddColumn()                                                                   | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsAlterTableWithDropColumn()                                                                  | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsColumnAliasing()                                                                            | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | nullPlusNonNullIsNull()                                                                             | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsConvert()                                                                                   | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsConvert(int fromType, int toType)                                                           | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsTableCorrelationNames()                                                                     | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsDifferentTableCorrelationNames()                                                            | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsExpressionsInOrderBy()                                                                      | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsOrderByUnrelated()                                                                          | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsGroupBy()                                                                                   | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsGroupByUnrelated()                                                                          | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsGroupByBeyondSelect()                                                                       | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsLikeEscapeClause()                                                                          | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsMultipleResultSets()                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsMultipleTransactions()                                                                      | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsNonNullableColumns()                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsMinimumSQLGrammar()                                                                         | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsCoreSQLGrammar()                                                                            | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsExtendedSQLGrammar()                                                                        | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsANSI92EntryLevelSQL()                                                                       | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsANSI92IntermediateSQL()                                                                     | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsANSI92FullSQL()                                                                             | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsIntegrityEnhancementFacility()                                                              | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsOuterJoins()                                                                                | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsFullOuterJoins()                                                                            | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsLimitedOuterJoins()                                                                         | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | isCatalogAtStart()                                                                                  | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsSchemasInDataManipulation()                                                                 | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsSavepoints()                                                                                | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | supportsResultSetHoldability(int holdability)                                                       | boolean     | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getResultSetHoldability()                                                                           | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDatabaseMajorVersion()                                                                           | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getDatabaseMinorVersion()                                                                           | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getJDBCMajorVersion()                                                                               | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+
   | getJDBCMinorVersion()                                                                               | int         | Yes                   |
   +-----------------------------------------------------------------------------------------------------+-------------+-----------------------+

java.sql.Driver
---------------

This section describes **java.sql.Driver**, the database driver interface.

.. table:: **Table 4** java.sql.Driver methods

   ==================================== =========== =====================
   Method                               Return Type Support JDBC 4 or Not
   ==================================== =========== =====================
   acceptsURL(String url)               boolean     Yes
   connect(String url, Properties info) Connection  Yes
   jdbcCompliant()                      boolean     Yes
   getMajorVersion()                    int         Yes
   getMinorVersion()                    int         Yes
   ==================================== =========== =====================

java.sql.PreparedStatement
--------------------------

This section describes **java.sql.PreparedStatement**, the interface for preparing statements.

.. table:: **Table 5** java.sql.PreparedStatement methods

   +-------------------------------------------------+-------------------+-----------------------+
   | Method                                          | Return Type       | Support JDBC 4 or Not |
   +=================================================+===================+=======================+
   | clearParameters()                               | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | execute()                                       | boolean           | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | executeQuery()                                  | ResultSet         | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | executeUpdate()                                 | int               | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | getMetaData()                                   | ResultSetMetaData | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setBoolean(int parameterIndex, boolean x)       | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setBigDecimal(int parameterIndex, BigDecimal x) | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setByte(int parameterIndex, byte x)             | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setBytes(int parameterIndex, byte[] x)          | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setDate(int parameterIndex, Date x)             | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setDouble(int parameterIndex, double x)         | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setFloat(int parameterIndex, float x)           | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setInt(int parameterIndex, int x)               | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setLong(int parameterIndex, long x)             | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setNString(int parameterIndex, String value)    | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setShort(int parameterIndex, short x)           | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | setString(int parameterIndex, String x)         | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | addBatch()                                      | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | executeBatch()                                  | int[]             | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+
   | clearBatch()                                    | void              | Yes                   |
   +-------------------------------------------------+-------------------+-----------------------+

.. note::

   -  **addBatch()** and **execute()** can be executed only after **clearBatch()**.
   -  Calling the **executeBatch()** method does not clear the batch. Clear batch by explicitly calling **clearBatch()**.
   -  You do not need to use **set*()** to reuse the values of bounded variables in a batch after they have been added.
   -  The following methods are inherited from **java.sql.Statement**: **close**, **execute**, **executeQuery**, **executeUpdate**, **getConnection**, **getResultSet**, **getUpdateCount**, **isClosed**, **setMaxRows**, and **setFetchSize**.

java.sql.ResultSet
------------------

This section describes **java.sql.ResultSet**, the interface for execution result sets.

.. table:: **Table 6** java.sql.ResultSet methods

   ================================= =========== =====================
   Method                            Return Type Support JDBC 4 or Not
   ================================= =========== =====================
   findColumn(String columnLabel)    int         Yes
   getBigDecimal(int columnIndex)    BigDecimal  Yes
   getBigDecimal(String columnLabel) BigDecimal  Yes
   getBoolean(int columnIndex)       boolean     Yes
   getBoolean(String columnLabel)    boolean     Yes
   getByte(int columnIndex)          byte        Yes
   getBytes(int columnIndex)         byte[]      Yes
   getByte(String columnLabel)       byte        Yes
   getBytes(String columnLabel)      byte[]      Yes
   getDate(int columnIndex)          Date        Yes
   getDate(String columnLabel)       Date        Yes
   getDouble(int columnIndex)        double      Yes
   getDouble(String columnLabel)     double      Yes
   getFloat(int columnIndex)         float       Yes
   getFloat(String columnLabel)      float       Yes
   getInt(int columnIndex)           int         Yes
   getInt(String columnLabel)        int         Yes
   getLong(int columnIndex)          long        Yes
   getLong(String columnLabel)       long        Yes
   getShort(int columnIndex)         short       Yes
   getShort(String columnLabel)      short       Yes
   getString(int columnIndex)        String      Yes
   getString(String columnLabel)     String      Yes
   getTime(int columnIndex)          Time        Yes
   getTime(String columnLabel)       Time        Yes
   getTimestamp(int columnIndex)     Timestamp   Yes
   getTimestamp(String columnLabel)  Timestamp   Yes
   isAfterLast()                     boolean     Yes
   isBeforeFirst()                   boolean     Yes
   isFirst()                         boolean     Yes
   next()                            boolean     Yes
   ================================= =========== =====================

.. note::

   -  A statement cannot have multiple open result sets.
   -  The cursor used to traverse the result set cannot remain in the open state after being committed.

java.sql.ResultSetMetaData
--------------------------

This section describes **java.sql.ResultSetMetaData**, which provides details about ResultSet object information.

.. table:: **Table 7** java.sql.ResultSetMetaData methods

   ============================= =========== =====================
   Method                        Return Type Support JDBC 4 or Not
   ============================= =========== =====================
   getColumnCount()              int         Yes
   getColumnName(int column)     String      Yes
   getColumnType(int column)     int         Yes
   getColumnTypeName(int column) String      Yes
   ============================= =========== =====================

java.sql.Statement
------------------

This section describes **java.sql.Statement**, the interface for executing SQL statements.

.. table:: **Table 8** java.sql.Statement methods

   ============================ =========== =====================
   Method                       Return Type Support JDBC 4 or Not
   ============================ =========== =====================
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
   ============================ =========== =====================

.. note::

   **setFetchSize** can reduce the memory occupied by the result set on the client. Result sets are packaged into cursors and segmented for processing, which will increase the communication traffic between the database and the client, affecting performance.

   Database cursors are valid only within their transactions. Therefore, when setting **setFetchSize**, set **setAutoCommit** to **false** and commit transactions on the connection to flush service data to a database.

javax.sql.ConnectionPoolDataSource
----------------------------------

This section describes **javax.sql.ConnectionPoolDataSource**, the interface for data source connection pools.

.. table:: **Table 9** javax.sql.ConnectionPoolDataSource methods

   +--------------------------------------------------+------------------+-----------------------+
   | Method                                           | Return Type      | Support JDBC 4 or Not |
   +==================================================+==================+=======================+
   | getLoginTimeout()                                | int              | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+
   | getLogWriter()                                   | PrintWriter      | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+
   | getPooledConnection()                            | PooledConnection | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+
   | getPooledConnection(String user,String password) | PooledConnection | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+
   | setLoginTimeout(int seconds)                     | void             | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+
   | setLogWriter(PrintWriter out)                    | void             | Yes                   |
   +--------------------------------------------------+------------------+-----------------------+

javax.sql.DataSource
--------------------

This section describes **javax.sql.DataSource**, the interface for data sources.

.. table:: **Table 10** javax.sql.DataSource methods

   +------------------------------------------------+-------------+-----------------------+
   | Method                                         | Return Type | Support JDBC 4 or Not |
   +================================================+=============+=======================+
   | getConnection()                                | Connection  | Yes                   |
   +------------------------------------------------+-------------+-----------------------+
   | getConnection(String username,String password) | Connection  | Yes                   |
   +------------------------------------------------+-------------+-----------------------+
   | getLoginTimeout()                              | int         | Yes                   |
   +------------------------------------------------+-------------+-----------------------+
   | getLogWriter()                                 | PrintWriter | Yes                   |
   +------------------------------------------------+-------------+-----------------------+
   | setLoginTimeout(int seconds)                   | void        | Yes                   |
   +------------------------------------------------+-------------+-----------------------+
   | setLogWriter(PrintWriter out)                  | void        | Yes                   |
   +------------------------------------------------+-------------+-----------------------+

javax.sql.PooledConnection
--------------------------

This section describes **javax.sql.PooledConnection**, the connection interface created by a connection pool.

.. table:: **Table 11** javax.sql.PooledConnection methods

   +------------------------------------------------------------------+-------------+-----------------------+
   | Method                                                           | Return Type | Support JDBC 4 or Not |
   +==================================================================+=============+=======================+
   | addConnectionEventListener (ConnectionEventListener listener)    | void        | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+
   | close()                                                          | void        | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+
   | getConnection()                                                  | Connection  | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+
   | removeConnectionEventListener (ConnectionEventListener listener) | void        | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+
   | addStatementEventListener (StatementEventListener listener)      | void        | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+
   | removeStatementEventListener (StatementEventListener listener)   | void        | Yes                   |
   +------------------------------------------------------------------+-------------+-----------------------+

javax.naming.Context
--------------------

This section describes **javax.naming.Context**, the context interface for connection configuration.

.. table:: **Table 12** javax.naming.Context methods

   ====================================== =========== =====================
   Method                                 Return Type Support JDBC 4 or Not
   ====================================== =========== =====================
   bind(Name name, Object obj)            void        Yes
   bind(String name, Object obj)          void        Yes
   lookup(Name name)                      Object      Yes
   lookup(String name)                    Object      Yes
   rebind(Name name, Object obj)          void        Yes
   rebind(String name, Object obj)        void        Yes
   rename(Name oldName, Name newName)     void        Yes
   rename(String oldName, String newName) void        Yes
   unbind(Name name)                      void        Yes
   unbind(String name)                    void        Yes
   ====================================== =========== =====================

javax.naming.spi.InitialContextFactory
--------------------------------------

This section describes **javax.naming.spi.InitialContextFactory**, the initial context factory interface.

.. table:: **Table 13** javax.naming.spi.InitialContextFactory methods

   +-----------------------------------------------+-------------+-----------------------+
   | Method                                        | Return Type | Support JDBC 4 or Not |
   +===============================================+=============+=======================+
   | getInitialContext(Hashtable<?,?> environment) | Context     | Yes                   |
   +-----------------------------------------------+-------------+-----------------------+

.. _en-us_topic_0000002080099797__en-us_topic_0000001188642156_section63487150165:

CopyManager
-----------

CopyManager is an API interface class provided by the JDBC driver in GaussDB(DWS). It is used to import data to GaussDB(DWS) in batches.

**Inheritance relationship of CopyManager**

The CopyManager class is in the **org.postgresql.copy** package class and inherits the java.lang.Object class. The declaration of the class is as follows:

.. code-block::

   public class CopyManager
   extends Object

**Construction method**

.. code-block::

   public CopyManager(BaseConnection connection)
   throws SQLException

**Common methods**

.. table:: **Table 14** Common methods of CopyManager

   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | Returned Value | Method                                               | Description                                                                               | throws                   |
   +================+======================================================+===========================================================================================+==========================+
   | CopyIn         | copyIn(String sql)                                   | ``-``                                                                                     | SQLException             |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyIn(String sql, InputStream from)                 | Uses **COPY FROM STDIN** to quickly load data to tables in the database from InputStream. | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyIn(String sql, InputStream from, int bufferSize) | Uses **COPY FROM STDIN** to quickly load data to tables in the database from InputStream. | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyIn(String sql, Reader from)                      | Uses **COPY FROM STDIN** to quickly load data to tables in the database from Reader.      | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyIn(String sql, Reader from, int bufferSize)      | Uses **COPY FROM STDIN** to quickly load data to tables in the database from Reader.      | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | CopyOut        | copyOut(String sql)                                  | ``-``                                                                                     | SQLException             |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyOut(String sql, OutputStream to)                 | Sends the result set of **COPY TO STDOUT** from the database to the OutputStream class.   | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
   | long           | copyOut(String sql, Writer to)                       | Sends the result set of **COPY TO STDOUT** from the database to the Writer class.         | SQLException,IOException |
   +----------------+------------------------------------------------------+-------------------------------------------------------------------------------------------+--------------------------+
