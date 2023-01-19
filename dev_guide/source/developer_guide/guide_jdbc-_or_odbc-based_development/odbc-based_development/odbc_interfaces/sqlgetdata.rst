:original_name: dws_04_0142.html

.. _dws_04_0142:

SQLGetData
==========

Function
--------

**SQLGetData** retrieves data for a single column in the current row of the result set. It can be called for many times to retrieve data of variable lengths.

Prototype
---------

::

   SQLRETURN SQLGetData(SQLHSTMT        StatementHandle,
                        SQLUSMALLINT    Col_or_Param_Num,
                        SQLSMALLINT     TargetType,
                        SQLPOINTER      TargetValuePtr,
                        SQLLEN          BufferLength,
                        SQLLEN          *StrLen_or_IndPtr);

Parameter
---------

.. table:: **Table 1** SQLGetData parameters

   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Keyword          | Description                                                                                                                                                                                                                                                                         |
   +==================+=====================================================================================================================================================================================================================================================================================+
   | StatementHandle  | Statement handle, obtained from **SQLAllocHandle**.                                                                                                                                                                                                                                 |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Col_or_Param_Num | Column number for which the data retrieval is requested. The column number starts with 1 and increases in ascending order. The number of the bookmark column is 0.                                                                                                                  |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetType       | C data type in the TargetValuePtr buffer. If **TargetType** is **SQL_ARD_TYPE**, the driver uses the data type of the **SQL_DESC_CONCISE_TYPE** field in ARD. If **TargetType** is **SQL_C_DEFAULT**, the driver selects a default data type according to the source SQL data type. |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetValuePtr   | **Output parameter**: pointer to the pointer that points to the buffer where the data is located.                                                                                                                                                                                   |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength     | Size of the buffer pointed to by **TargetValuePtr**.                                                                                                                                                                                                                                |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr | **Output parameter**: pointer to the buffer where the length or identifier value is returned.                                                                                                                                                                                       |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

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
