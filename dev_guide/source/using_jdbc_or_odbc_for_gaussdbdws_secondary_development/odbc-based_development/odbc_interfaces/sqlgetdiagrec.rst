:original_name: dws_04_0143.html

.. _dws_04_0143:

SQLGetDiagRec
=============

Function
--------

**SQLGetDiagRec** returns the current values of multiple fields of a diagnostic record that contains error, warning, and status information.

Prototype
---------

::

   SQLRETURN  SQLGetDiagRec(SQLSMALLINT    HandleType
                            SQLHANDLE      Handle,
                            SQLSMALLINT    RecNumber,
                            SQLCHAR        *SQLState,
                            SQLINTEGER     *NativeErrorPtr,
                            SQLCHAR        *MessageText,
                            SQLSMALLINT    BufferLength
                            SQLSMALLINT    *TextLengthPtr);

Parameter
---------

.. table:: **Table 1** SQLGetDiagRec parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword                           | Description                                                                                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================================================================================+
   | HandleType                        | A handle-type identifier that describes the type of handle for which diagnostics are desired. The value must be one of the following:                                                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                                                                    |
   |                                   | -  SQL_HANDLE_ENV                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  SQL_HANDLE_DBC                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  SQL_HANDLE_STMT                                                                                                                                                                                                                                                                                                                 |
   |                                   | -  SQL_HANDLE_DESC                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Handle                            | A handle for the diagnostic data structure. Its type is indicated by HandleType. If **HandleType** is **SQL_HANDLE_ENV**, **Handle** may be shared or non-shared environment handle.                                                                                                                                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RecNumber                         | Indicates the status record from which the application seeks information. RecNumber starts with 1.                                                                                                                                                                                                                                 |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQLState                          | **Output parameter**: pointer to a buffer that saves the 5-character **SQLSTATE** code pertaining to **RecNumber**.                                                                                                                                                                                                                |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NativeErrorPtr                    | **Output parameter**: pointer to a buffer that saves the native error code.                                                                                                                                                                                                                                                        |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | MessageText                       | Pointer to a buffer that saves text strings of diagnostic information.                                                                                                                                                                                                                                                             |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength                      | Length of MessageText.                                                                                                                                                                                                                                                                                                             |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TextLengthPtr                     | **Output parameter**: pointer to the buffer, the total number of bytes in the returned **MessageText**. If the number of bytes available to return is greater than **BufferLength**, then the diagnostics information text in **MessageText** is truncated to **BufferLength** minus the length of the null termination character. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

**SQLGetDiagRec** does not release diagnostic records for itself. It uses the following returned values to report execution results:

-  **SQL_SUCCESS**: The function successfully returns diagnostic information.
-  **SQL_SUCCESS_WITH_INFO**: The **\*MessageText** buffer is too small to hold the requested diagnostic message. No diagnostic records are generated.
-  **SQL_INVALID_HANDLE**: The handle indicated by **HandType** and **Handle** is not a valid handle.
-  **SQL_ERROR**: **RecNumber** is smaller than or equal to zero, or **BufferLength** is smaller than zero.

If an ODBC function returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call **SQLGetDiagRec** and obtain the **SQLSTATE** value. The possible **SQLSTATE** values are listed as follows:

.. table:: **Table 2** SQLSTATE values

   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | SQLSATATE | Error                                | Description                                                                                                 |
   +===========+======================================+=============================================================================================================+
   | HY000     | General error                        | An error occurred for which there is no specific SQLSTATE.                                                  |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | HY001     | Memory allocation error              | The driver is unable to allocate memory required to support execution or completion of the function.        |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | HY008     | Operation canceled                   | SQLCancel is called to terminate the statement execution, but the StatementHandle function is still called. |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | HY010     | Function sequence error              | The function is called prior to sending data to data parameters or columns being executed.                  |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | HY013     | Memory management error              | The function fails to be called. The error may be caused by low memory conditions.                          |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | HYT01     | Connection timed out                 | The timeout period expired before the application was able to connect to the data source.                   |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+
   | IM001     | Function not supported by the driver | The called function is not supported by the StatementHandle driver.                                         |
   +-----------+--------------------------------------+-------------------------------------------------------------------------------------------------------------+

Examples
--------

See :ref:`Examples <dws_04_0123>`.
