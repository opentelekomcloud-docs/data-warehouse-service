:original_name: dws_04_0134.html

.. _dws_04_0134:

SQLExecDirect
=============

Function
--------

**SQLExecDirect** executes a prepared SQL statement specified in this parameter. This is the fastest execution method for executing only one SQL statement at a time.

Prototype
---------

::

   SQLRETURN SQLExecDirect(SQLHSTMT         StatementHandle,
                           SQLCHAR         *StatementText,
                           SQLINTEGER       TextLength);

Parameter
---------

.. table:: **Table 1** SQLExecDirect parameters

   +-----------------+----------------------------------------------------------------------------+
   | Keyword         | Description                                                                |
   +=================+============================================================================+
   | StatementHandle | Statement handle, obtained from **SQLAllocHandle**.                        |
   +-----------------+----------------------------------------------------------------------------+
   | StatementText   | SQL statement to be executed. One SQL statement can be executed at a time. |
   +-----------------+----------------------------------------------------------------------------+
   | TextLength      | Length of **StatementText**.                                               |
   +-----------------+----------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_NEED_DATA** indicates insufficient parameters provided before executing the SQL statement.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.
-  **SQL_NO_DATA** indicates that the SQL statement does not return a result set.

Precautions
-----------

If **SQLExecDirect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
