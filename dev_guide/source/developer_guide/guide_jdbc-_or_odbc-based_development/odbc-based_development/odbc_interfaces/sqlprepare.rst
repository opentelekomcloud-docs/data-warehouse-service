:original_name: dws_04_0141.html

.. _dws_04_0141:

SQLPrepare
==========

Function
--------

**SQLPrepare** prepares an SQL statement to be executed.

Prototype
---------

::

   SQLRETURN SQLPrepare(SQLHSTMT      StatementHandle,
                        SQLCHAR       *StatementText,
                        SQLINTEGER    TextLength);

Parameter
---------

.. table:: **Table 1** SQLPrepare parameters

   =============== ============================
   Keyword         Description
   =============== ============================
   StatementHandle Statement handle.
   StatementText   SQL text string.
   TextLength      Length of **StatementText**.
   =============== ============================

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

Precautions
-----------

If **SQLPrepare** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
