:original_name: dws_04_0129.html

.. _dws_04_0129:

SQLBindCol
==========

Function
--------

**SQLBindCol** is used to associate (bind) columns in a result set to an application data buffer.

Prototype
---------

::

   SQLRETURN SQLBindCol(SQLHSTMT       StatementHandle,
                        SQLUSMALLINT   ColumnNumber,
                        SQLSMALLINT    TargetType,
                        SQLPOINTER     TargetValuePtr,
                        SQLLEN         BufferLength,
                        SQLLEN         *StrLen_or_IndPtr);

Parameter
---------

.. table:: **Table 1** SQLBindCol parameters

   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword          | Description                                                                                                                                                                                   |
   +==================+===============================================================================================================================================================================================+
   | StatementHandle  | Statement handle.                                                                                                                                                                             |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ColumnNumber     | Number of the column to be bound. The column number starts with 0 and increases in ascending order. Column 0 is the bookmark column. If no bookmark column is set, column numbers start at 1. |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetType       | The C data type in the buffer.                                                                                                                                                                |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetValuePtr   | **Output parameter**: pointer to the buffer bound with the column. The SQLFetch function returns data in the buffer. If **TargetValuePtr** is null, **StrLen_or_IndPtr** is a valid value.    |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength     | Size of the **TargetValuePtr** buffer in bytes available to store the column data.                                                                                                            |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr | **Output parameter**: pointer to the length or indicator of the buffer. If **StrLen_or_IndPtr** is null, no length or indicator is used.                                                      |
   +------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  SQL_SUCCESS indicates that the call succeeded.
-  SQL_SUCCESS_WITH_INFO indicates some warning information is displayed.
-  SQL_ERROR indicates major errors, such as memory allocation and connection failures.
-  SQL_INVALID_HANDLE indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLBindCol** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, with **HandleType** and **Handle** set to **SQL_HANDLE_STMT** and **StatementHandle**, respectively, to obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
