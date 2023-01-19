:original_name: dws_04_0130.html

.. _dws_04_0130:

SQLBindParameter
================

Function
--------

**SQLBindParameter** is used to associate (bind) parameter markers in an SQL statement to a buffer.

Prototype
---------

::

   SQLRETURN SQLBindParameter(SQLHSTMT       StatementHandle,
                              SQLUSMALLINT   ParameterNumber,
                              SQLSMALLINT    InputOutputType,
                              SQLSMALLINT    ValuetType,
                              SQLSMALLINT    ParameterType,
                              SQLULEN        ColumnSize,
                              SQLSMALLINT    DecimalDigits,
                              SQLPOINTER     ParameterValuePtr,
                              SQLLEN         BufferLength,
                              SQLLEN         *StrLen_or_IndPtr);

Parameter
---------

.. table:: **Table 1** SQLBindParameter

   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | Keyword           | Description                                                                                                    |
   +===================+================================================================================================================+
   | StatementHandle   | Statement handle.                                                                                              |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | ParameterNumber   | Parameter marker number, starting at 1 and increasing in an ascending order.                                   |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | InputOutputType   | Input/output type of the parameter.                                                                            |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | ValueType         | C data type of the parameter.                                                                                  |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | ParameterType     | SQL data type of the parameter.                                                                                |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | ColumnSize        | Size of the column or expression of the corresponding parameter marker.                                        |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | DecimalDigits     | Digital number of the column or the expression of the corresponding parameter marker.                          |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | ParameterValuePtr | Pointer to the storage parameter buffer.                                                                       |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | BufferLength      | Size of the ParameterValuePtr buffer in bytes.                                                                 |
   +-------------------+----------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr  | Pointer to the length or indicator of the buffer. If StrLen_or_IndPtr is null, no length or indicator is used. |
   +-------------------+----------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLBindCol** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, with **HandleType** and **Handle** set to **SQL_HANDLE_STMT** and **StatementHandle**, respectively, to obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
