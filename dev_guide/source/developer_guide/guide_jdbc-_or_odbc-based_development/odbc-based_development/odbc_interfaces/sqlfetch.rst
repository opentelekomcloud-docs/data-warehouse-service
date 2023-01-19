:original_name: dws_04_0136.html

.. _dws_04_0136:

SQLFetch
========

Function
--------

**SQLFetch** advances the cursor to the next row of the result set and retrieves any bound columns.

Prototype
---------

::

   SQLRETURN SQLFetch(SQLHSTMT    StatementHandle);

Parameter
---------

.. table:: **Table 1** SQLFetch parameters

   =============== ===================================================
   Keyword         Description
   =============== ===================================================
   StatementHandle Statement handle, obtained from **SQLAllocHandle**.
   =============== ===================================================

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_NO_DATA** indicates that the SQL statement does not return a result set.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

Precautions
-----------

If **SQLFetch** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
