:original_name: dws_04_0131.html

.. _dws_04_0131:

SQLColAttribute
===============

Function
--------

**SQLColAttribute** returns the descriptor information about a column in the result set.

Prototype
---------

::

   SQLRETURN SQLColAttribute(SQLHSTMT        StatementHandle,
                             SQLUSMALLINT    ColumnNumber,
                             SQLUSMALLINT    FieldIdentifier,
                             SQLPOINTER      CharacterAtrriburePtr,
                             SQLSMALLINT     BufferLength,
                             SQLSMALLINT     *StringLengthPtr,
                             SQLPOINTER      NumericAttributePtr);

Parameter
---------

.. table:: **Table 1** SQLColAttribute parameter

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword                           | Description                                                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================================================+
   | StatementHandle                   | Statement handle.                                                                                                                                                                                                |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ColumnNumber                      | Column number of the field to be queried, starting at 1 and increasing in an ascending order.                                                                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FieldIdentifier                   | Field identifier of **ColumnNumber** in IRD.                                                                                                                                                                     |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CharacterAttributePtr             | **Output parameter**: pointer to the buffer that returns FieldIdentifier field value.                                                                                                                            |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength                      | -  **FieldIdentifier** indicates the length of the buffer if **FieldIdentifier** is an ODBC-defined field and **CharacterAttributePtr** points to a character string or a binary buffer.                         |
   |                                   | -  Ignore this parameter if **FieldIdentifier** is an ODBC-defined field and **CharacterAttributePtr** points to an integer.                                                                                     |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLengthPtr                   | **Output parameter**: pointer to a buffer in which the total number of valid bytes (for string data) is stored in **\*CharacterAttributePtr**. Ignore the value of **BufferLength** if the data is not a string. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NumericAttributePtr               | **Output parameter**: pointer to an integer buffer in which the value of the **FieldIdentifier** field in the **ColumnNumber** row of the IRD is returned.                                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLColAttribute** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
