:original_name: dws_04_0145.html

.. _dws_04_0145:

SQLSetEnvAttr
=============

Function
--------

**SQLSetEnvAttr** sets environment attributes.

Prototype
---------

::

   SQLRETURN SQLSetEnvAttr(SQLHENV       EnvironmentHandle
                           SQLINTEGER    Attribute,
                           SQLPOINTER    ValuePtr,
                           SQLINTEGER    StringLength);

Parameters
----------

.. table:: **Table 1** SQLSetEnvAttr parameters

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword                           | Description                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================+
   | EnviromentHandle                  | Environment handle.                                                                                                                                                              |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Attribute                         | Environment attribute to be set. Its value must be one of the following:                                                                                                         |
   |                                   |                                                                                                                                                                                  |
   |                                   | -  **SQL_ATTR_ODBC_VERSION**: ODBC version                                                                                                                                       |
   |                                   | -  **SQL_CONNECTION_POOLING**: connection pool attribute                                                                                                                         |
   |                                   | -  **SQL_OUTPUT_NTS**: string type returned by the driver                                                                                                                        |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ValuePtr                          | Pointer to the Attribute value. **ValuePtr** depends on the Attribute value, and can be a 32-bit integer value or a null-terminated string.                                      |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLength                      | If **ValuePtr** points to a string or a binary buffer, this parameter should be the length of **\*ValuePtr**. If **ValuePtr** points to an integer, **StringLength** is ignored. |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLSetEnvAttr** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_ENV** and **EnvironmentHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
