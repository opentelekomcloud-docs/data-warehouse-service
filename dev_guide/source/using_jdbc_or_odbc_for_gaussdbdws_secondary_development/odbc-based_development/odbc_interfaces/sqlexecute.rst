:original_name: dws_04_0135.html

.. _dws_04_0135:

SQLExecute
==========

Function
--------

The **SQLExecute** function executes a prepared SQL statement using **SQLPrepare**. The statement is executed using the current value of any application variables that were bound to parameter markers by **SQLBindParameter**.

Prototype
---------

::

   SQLRETURN SQLExecute(SQLHSTMT    StatementHandle);

Parameter
---------

.. table:: **Table 1** SQLExecute parameters

   =============== ================================
   Keyword         Description
   =============== ================================
   StatementHandle Statement handle to be executed.
   =============== ================================

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_NEED_DATA** indicates insufficient parameters provided before executing the SQL statement.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_NO_DATA** indicates that the SQL statement does not return a result set.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

Precautions
-----------

If **SQLExecute** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
