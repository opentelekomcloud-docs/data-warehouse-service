:original_name: dws_04_0127.html

.. _dws_04_0127:

SQLAllocHandle
==============

Function
--------

**SQLAllocHandle** allocates environment, connection, or statement handles. This function is a generic function for allocating handles that replaces the deprecated ODBC 2.x functions **SQLAllocEnv**, **SQLAllocConnect**, and **SQLAllocStmt**.

Prototype
---------

::

   SQLRETURN SQLAllocHandle(SQLSMALLINT   HandleType,
                            SQLHANDLE     InputHandle,
                            SQLHANDLE     *OutputHandlePtr);

Parameter
---------

.. table:: **Table 1** SQLAllocHandle parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword                           | Description                                                                                                                                                            |
   +===================================+========================================================================================================================================================================+
   | HandleType                        | The type of handle to be allocated by SQLAllocHandle. The value must be one of the following:                                                                          |
   |                                   |                                                                                                                                                                        |
   |                                   | -  SQL_HANDLE_ENV (environment handle)                                                                                                                                 |
   |                                   | -  SQL_HANDLE_DBC (connection handle)                                                                                                                                  |
   |                                   | -  SQL_HANDLE_STMT (statement handle)                                                                                                                                  |
   |                                   | -  SQL_HANDLE_DESC (description handle)                                                                                                                                |
   |                                   |                                                                                                                                                                        |
   |                                   | The handle application sequence is: **SQL_HANDLE_ENV** > **SQL_HANDLE_DBC** > **SQL_HANDLE_STMT**. The handle applied later depends on the handle applied prior to it. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | InputHandle                       | Existing handle to use as a context for the new handle being allocated.                                                                                                |
   |                                   |                                                                                                                                                                        |
   |                                   | -  If **HandleType** is **SQL_HANDLE_ENV**, this is SQL_NULL_HANDLE.                                                                                                   |
   |                                   | -  If **HandleType** is **SQL_HANDLE_DBC**, this must be an environment handle.                                                                                        |
   |                                   | -  If **HandleType** is **SQL_HANDLE_STMT** or **SQL_HANDLE_DESC**, it must be a connection handle.                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OutputHandlePtr                   | **Output parameter**: Pointer to a buffer in which to return the handle to the newly allocated data structure.                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return value
------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

When allocating a non-environment handle, if **SQLAllocHandle** returns **SQL_ERROR**, it sets **OutputHandlePtr** to **SQL_NULL_HENV**, **SQL_NULL_HDBC**, **SQL_NULL_HSTMT**, or **SQL_NULL_HDESC**. The application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, with **HandleType** and **Handle** set to **IntputHandle**, to obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
