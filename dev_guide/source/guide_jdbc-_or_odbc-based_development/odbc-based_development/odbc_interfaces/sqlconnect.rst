:original_name: dws_04_0132.html

.. _dws_04_0132:

SQLConnect
==========

Function
--------

**SQLConnect** establishes a connection between a driver and a data source. After the connection, the connection handle can be used to access all information about the data source, including its application operating status, transaction processing status, and error information.

Prototype
---------

::

   SQLRETURN  SQLConnect(SQLHDBC        ConnectionHandle,
                         SQLCHAR        *ServerName,
                         SQLSMALLINT    NameLength1,
                         SQLCHAR        *UserName,
                         SQLSMALLINT    NameLength2,
                         SQLCHAR        *Authentication,
                         SQLSMALLINT    NameLength3);

Parameter
---------

.. table:: **Table 1** SQLConnect parameters

   ================ ====================================================
   Keyword          Description
   ================ ====================================================
   ConnectionHandle Connection handle, obtained from **SQLAllocHandle**.
   ServerName       Name of the data source to connect to.
   NameLength1      Length of **ServerName**.
   UserName         User name of the database in the data source.
   NameLength2      Length of **UserName**.
   Authentication   User password of the database in the data source.
   NameLength3      Length of **Authentication**.
   ================ ====================================================

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

Precautions
-----------

If **SQLConnect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
