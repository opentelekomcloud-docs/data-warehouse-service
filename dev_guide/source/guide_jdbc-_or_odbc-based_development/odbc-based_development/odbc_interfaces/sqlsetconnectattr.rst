:original_name: dws_04_0144.html

.. _dws_04_0144:

SQLSetConnectAttr
=================

Function
--------

**SQLSetConnectAttr** sets connection attributes.

Prototype
---------

::

   SQLRETURN SQLSetConnectAttr(SQLHDBC       ConnectionHandle
                               SQLINTEGER    Attribute,
                               SQLPOINTER    ValuePtr,
                               SQLINTEGER    StringLength);

Parameter
---------

.. table:: **Table 1** SQLSetConnectAttr parameters

   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword          | Description                                                                                                                                                                                                                        |
   +==================+====================================================================================================================================================================================================================================+
   | StatementtHandle | Connection handle.                                                                                                                                                                                                                 |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Attribute        | Attribute to set.                                                                                                                                                                                                                  |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ValuePtr         | Pointer to the Attribute value. **ValuePtr** depends on the Attribute value, and can be a 32-bit unsigned integer value or a null-terminated string. If **ValuePtr** parameter is driver-specific value, it may be signed integer. |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLength     | If **ValuePtr** points to a string or a binary buffer, this parameter should be the length of **\*ValuePtr**. If **ValuePtr** points to an integer, **StringLength** is ignored.                                                   |
   +------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLSetConnectAttr** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
