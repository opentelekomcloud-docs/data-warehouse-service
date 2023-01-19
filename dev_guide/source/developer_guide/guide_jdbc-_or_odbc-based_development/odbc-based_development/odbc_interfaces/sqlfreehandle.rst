:original_name: dws_04_0139.html

.. _dws_04_0139:

SQLFreeHandle
=============

Function
--------

**SQLFreeHandle** releases resources associated with a specific environment, connection, or statement handle. It replaces the ODBC 2.x functions: **SQLFreeEnv**, **SQLFreeConnect**, and **SQLFreeStmt**.

Prototype
---------

::

   SQLRETURN SQLFreeHandle(SQLSMALLINT   HandleType,
                           SQLHANDLE     Handle);

Parameter
---------

.. table:: **Table 1** SQLFreeHandle parameters

   +-----------------------------------+---------------------------------------------------------------------------------------------------------+
   | Keyword                           | Description                                                                                             |
   +===================================+=========================================================================================================+
   | HandleType                        | The type of handle to be freed by SQLFreeHandle. The value must be one of the following:                |
   |                                   |                                                                                                         |
   |                                   | -  SQL_HANDLE_ENV                                                                                       |
   |                                   | -  SQL_HANDLE_DBC                                                                                       |
   |                                   | -  SQL_HANDLE_STMT                                                                                      |
   |                                   | -  SQL_HANDLE_DESC                                                                                      |
   |                                   |                                                                                                         |
   |                                   | If **HandleType** is not one of the preceding values, **SQLFreeHandle** returns **SQL_INVALID_HANDLE**. |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------+
   | Handle                            | The name of the handle to be freed.                                                                     |
   +-----------------------------------+---------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLFreeHandle** returns **SQL_ERROR**, the handle is still valid.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
