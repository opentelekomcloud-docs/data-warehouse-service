:original_name: dws_04_0124.html

.. _dws_04_0124:

ODBC Interfaces
===============

The ODBC interface is a set of API functions provided to users. This chapter describes its common interfaces. For details on other interfaces, see "ODBC Programmer's Reference" at MSDN (https://msdn.microsoft.com/en-us/library/windows/desktop/ms714177(v=vs.85).aspx).

SQLAllocEnv
-----------

In ODBC 3.\ *x*, **SQLAllocEnv** (a function in ODBC 2.\ *x*) was deprecated and replaced by **SQLAllocHandle**. For details, see :ref:`SQLAllocHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section19259553162912>`.

SQLAllocConnect
---------------

In ODBC 3.\ *x*, **SQLAllocConnect** (a function in ODBC 2.\ *x*) was deprecated and replaced by **SQLAllocHandle**. For details, see :ref:`SQLAllocHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section19259553162912>`.

.. _en-us_topic_0000002044151968__en-us_topic_0000001188163746_section19259553162912:

SQLAllocHandle
--------------

**Function**

**SQLAllocHandle** allocates environment, connection, or statement handles. It replaces the ODBC 2.\ *x* functions **SQLAllocEnv**, **SQLAllocConnect**, and **SQLAllocStmt**.

**Prototype**

::

   SQLRETURN SQLAllocHandle(SQLSMALLINT   HandleType,
                            SQLHANDLE     InputHandle,
                            SQLHANDLE     *OutputHandlePtr);

**Parameters**

.. table:: **Table 1** SQLAllocHandle parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                            |
   +===================================+========================================================================================================================================================================+
   | HandleType                        | Handle type allocated by **SQLAllocHandle**. The value must be one of the following:                                                                                   |
   |                                   |                                                                                                                                                                        |
   |                                   | -  **SQL_HANDLE_ENV** (environment handle)                                                                                                                             |
   |                                   | -  **SQL_HANDLE_DBC** (connection handle)                                                                                                                              |
   |                                   | -  **SQL_HANDLE_STMT** (statement handle)                                                                                                                              |
   |                                   | -  **SQL_HANDLE_DESC** (description handle)                                                                                                                            |
   |                                   |                                                                                                                                                                        |
   |                                   | The handle application sequence is: **SQL_HANDLE_ENV** > **SQL_HANDLE_DBC** > **SQL_HANDLE_STMT**. The handle applied later depends on the handle applied prior to it. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | InputHandle                       | Type of the new handle to be allocated.                                                                                                                                |
   |                                   |                                                                                                                                                                        |
   |                                   | -  If **HandleType** is **SQL_HANDLE_ENV**, the value is **SQL_NULL_HANDLE**.                                                                                          |
   |                                   | -  If **HandleType** is **SQL_HANDLE_DBC**, this must be an environment handle.                                                                                        |
   |                                   | -  If **HandleType** is **SQL_HANDLE_STMT** or **SQL_HANDLE_DESC**, it must be a connection handle.                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | OutputHandlePtr                   | **Output parameter**: Pointer to a buffer in which the handle returned for the newly allocated data structure is stored.                                               |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLAllocHandle** returns **SQL_ERROR** when it is used to allocate a non-environment handle, it sets **OutputHandlePtr** to **SQL_NULL_HDBC**, **SQL_NULL_HSTMT**, or **SQL_NULL_HDESC**. The application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **IntputHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLAllocStmt
------------

In ODBC 3.\ *x*, **SQLAllocStmt** (a function in ODBC 2.\ *x*) was deprecated and replaced by **SQLAllocHandle**. For details, see :ref:`SQLAllocHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section19259553162912>`.

SQLBindCol
----------

**Function**

**SQLBindCol** is used to associate (bind) columns in a result set to an application data buffer.

**Prototype**

::

   SQLRETURN SQLBindCol(SQLHSTMT       StatementHandle,
                        SQLUSMALLINT   ColumnNumber,
                        SQLSMALLINT    TargetType,
                        SQLPOINTER     TargetValuePtr,
                        SQLLEN         BufferLength,
                        SQLLEN         *StrLen_or_IndPtr);

**Parameters**

.. table:: **Table 2** SQLBindCol parameters

   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                  |
   +==================+==============================================================================================================================================================================================================+
   | StatementHandle  | Statement handle.                                                                                                                                                                                            |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ColumnNumber     | Number of the column to be bound. Column numbering begins at 0 and increases in ascending order. Column **0** functions as the bookmark. If no bookmark column is set, column numbering begins at 1 instead. |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetType       | The C data type in the buffer.                                                                                                                                                                               |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetValuePtr   | **Output parameter**: pointer to the buffer bound with the column. The **SQLFetch** function returns data in the buffer. If **TargetValuePtr** is null, **StrLen_or_IndPtr** is a valid value.               |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength     | Length of the buffer to which **TargetValuePtr** points, in bytes.                                                                                                                                           |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr | **Output parameter**: pointer to the length or indicator of the buffer. If **StrLen_or_IndPtr** is null, no length or indicator is used.                                                                     |
   +------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Note**

If **SQLBindCol** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLBindParameter
----------------

**Function**

**SQLBindParameter** binds a parameter flag in an SQL statement to a buffer.

**Prototype**

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

**Parameters**

.. table:: **Table 3** SQLBindParameter

   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | Keyword           | Description                                                                                                        |
   +===================+====================================================================================================================+
   | StatementHandle   | Statement handle.                                                                                                  |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | ParameterNumber   | Parameter marker number, starting at 1 and increasing in an ascending order.                                       |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | InputOutputType   | Input and output parameter types.                                                                                  |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | ValueType         | C data type of the parameter.                                                                                      |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | ParameterType     | SQL data type of the parameter.                                                                                    |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | ColumnSize        | Column size or the expression of the corresponding parameter marker.                                               |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | DecimalDigits     | Decimal number of the column or the expression of the corresponding parameter marker.                              |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | ParameterValuePtr | Pointer to the buffer for storing parameter data.                                                                  |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | BufferLength      | Length of the buffer to which the **ParameterValuePtr** points, in bytes.                                          |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr  | Pointer to the length or indicator of the buffer. If **StrLen_or_IndPtr** is null, no length or indicator is used. |
   +-------------------+--------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLBindCol** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLColAttribute
---------------

**Function**

**SQLColAttribute** returns the descriptor information about a column in the result set.

**Prototype**

::

   SQLRETURN SQLColAttribute(SQLHSTMT        StatementHandle,
                             SQLUSMALLINT    ColumnNumber,
                             SQLUSMALLINT    FieldIdentifier,
                             SQLPOINTER      CharacterAtrriburePtr,
                             SQLSMALLINT     BufferLength,
                             SQLSMALLINT     *StringLengthPtr,
                             SQLPOINTER      NumericAttributePtr);

**Parameters**

.. table:: **Table 4** SQLColAttribute parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================================================+
   | StatementHandle                   | Statement handle.                                                                                                                                                                                                |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ColumnNumber                      | Column number of the field to be queried, starting at 1 and increasing in an ascending order.                                                                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FieldIdentifier                   | Field identifier of **ColumnNumber** in IRD.                                                                                                                                                                     |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | CharacterAttributePtr             | **Output parameter**: pointer to the buffer that returns FieldIdentifier field value.                                                                                                                            |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength                      | -  **FieldIdentifier** indicates the buffer length when it refers to an ODBC-defined field and **CharacterAttributePtr** points to a string or binary buffer.                                                    |
   |                                   | -  Ignore this parameter if **FieldIdentifier** is an ODBC-defined field and **CharacterAttributePtr** points to an integer.                                                                                     |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLengthPtr                   | **Output parameter**: pointer to a buffer in which the total number of valid bytes (for string data) is stored in **\*CharacterAttributePtr**. Ignore the value of **BufferLength** if the data is not a string. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NumericAttributePtr               | **Output parameter**: pointer to an integer buffer in which the value of the **FieldIdentifier** field in the **ColumnNumber** row of the IRD is returned.                                                       |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLColAttribute** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLConnect
----------

**Function**

**SQLConnect** establishes a connection between a driver and a data source. Using the connection handle, you can obtain crucial information like the program's status, transaction processing status, and error messages after establishing a connection to the data source.

**Prototype**

::

   SQLRETURN  SQLConnect(SQLHDBC        ConnectionHandle,
                         SQLCHAR        *ServerName,
                         SQLSMALLINT    NameLength1,
                         SQLCHAR        *UserName,
                         SQLSMALLINT    NameLength2,
                         SQLCHAR        *Authentication,
                         SQLSMALLINT    NameLength3);

**Parameters**

.. table:: **Table 5** SQLConnect parameters

   ================ ====================================================
   Parameter        Description
   ================ ====================================================
   ConnectionHandle Connection handle, obtained from **SQLAllocHandle**.
   ServerName       Name of the data source to connect to.
   NameLength1      Length of **ServerName**.
   UserName         Database username in the data source.
   NameLength2      Length of **UserName**.
   Authentication   Password of the database user in the data source.
   NameLength3      Length of **Authentication**.
   ================ ====================================================

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

**Precautions**

If **SQLConnect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLDisconnect
-------------

**Function**

**SQLDisconnect** closes the connection associated with the database connection handle.

**Prototype**

::

   SQLRETURN SQLDisconnect(SQLHDBC    ConnectionHandle);

**Parameters**

.. table:: **Table 6** SQLDisconnect parameters

   ================ ====================================================
   Parameter        Description
   ================ ====================================================
   ConnectionHandle Connection handle, obtained from **SQLAllocHandle**.
   ================ ====================================================

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLDisconnect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLExecDirect
-------------

**Function**

**SQLExecDirect** executes a prepared SQL statement specified in this parameter. This is the fastest execution method for executing only one SQL statement at a time.

**Prototype**

::

   SQLRETURN SQLExecDirect(SQLHSTMT         StatementHandle,
                           SQLCHAR         *StatementText,
                           SQLINTEGER       TextLength);

**Parameters**

.. table:: **Table 7** SQLExecDirect parameters

   +-----------------+----------------------------------------------------------------------------+
   | Parameter       | Description                                                                |
   +=================+============================================================================+
   | StatementHandle | Statement handle, obtained from **SQLAllocHandle**.                        |
   +-----------------+----------------------------------------------------------------------------+
   | StatementText   | SQL statement to be executed. One SQL statement can be executed at a time. |
   +-----------------+----------------------------------------------------------------------------+
   | TextLength      | Length of **StatementText**.                                               |
   +-----------------+----------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_NEED_DATA** indicates that there are not enough parameters provided to execute the SQL statement.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.
-  **SQL_NO_DATA** indicates that no result set is returned for the SQL statement.

**Precautions**

If **SQLExecDirect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLExecute
----------

**Function**

When a statement includes a parameter marker, the **SQLExecute** function executes a prepared SQL statement using the current value of the marker.

**Prototype**

::

   SQLRETURN SQLExecute(SQLHSTMT    StatementHandle);

**Parameters**

.. table:: **Table 8** SQLExecute parameters

   =============== ================================
   Parameter       Description
   =============== ================================
   StatementHandle Statement handle to be executed.
   =============== ================================

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_NEED_DATA** indicates that there are not enough parameters provided to execute the SQL statement.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_NO_DATA** indicates that no result set is returned for the SQL statement.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

**Precautions**

If **SQLExecute** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLFetch
--------

**Function**

**SQLFetch** advances the cursor to the next row of the result set and retrieves any bound columns.

**Prototype**

::

   SQLRETURN SQLFetch(SQLHSTMT    StatementHandle);

**Parameters**

.. table:: **Table 9** SQLFetch parameters

   =============== ===================================================
   Parameter       Description
   =============== ===================================================
   StatementHandle Statement handle, obtained from **SQLAllocHandle**.
   =============== ===================================================

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_NO_DATA** indicates that no result set is returned for the SQL statement.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

**Precautions**

If **SQLFetch** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLFreeStmt
-----------

In ODBC 3.\ *x*, **SQLFreeStmt** (a function in ODBC 2.\ *x*) was deprecated and replaced with **SQLFreeHandle**. For details, see :ref:`SQLFreeHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section86888253574>`.

SQLFreeConnect
--------------

In ODBC 3.\ *x*, **SQLFreeConnect** (a function in ODBC 2.\ *x*) was deprecated and replaced with **SQLFreeHandle**. For details, see :ref:`SQLFreeHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section86888253574>`.

.. _en-us_topic_0000002044151968__en-us_topic_0000001188163746_section86888253574:

SQLFreeHandle
-------------

**Function**

**SQLFreeHandle** releases resources associated with a specific environment, connection, or statement handle. It replaces the ODBC 2.\ *x* functions: **SQLFreeEnv**, **SQLFreeConnect**, and **SQLFreeStmt**.

**Prototype**

::

   SQLRETURN SQLFreeHandle(SQLSMALLINT   HandleType,
                           SQLHANDLE     Handle);

**Parameters**

.. table:: **Table 10** SQLFreeHandle parameters

   +-----------------------------------+-------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                     |
   +===================================+=================================================================================================+
   | HandleType                        | Type of handle to be freed by **SQLFreeHandle**. The value must be one of the following:        |
   |                                   |                                                                                                 |
   |                                   | -  SQL_HANDLE_ENV                                                                               |
   |                                   | -  SQL_HANDLE_DBC                                                                               |
   |                                   | -  SQL_HANDLE_STMT                                                                              |
   |                                   | -  SQL_HANDLE_DESC                                                                              |
   |                                   |                                                                                                 |
   |                                   | If **HandleType** is not one of these values, **SQLFreeHandle** returns **SQL_INVALID_HANDLE**. |
   +-----------------------------------+-------------------------------------------------------------------------------------------------+
   | Handle                            | Handle to be released.                                                                          |
   +-----------------------------------+-------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLFreeHandle** returns **SQL_ERROR**, the handle is still valid.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLFreeEnv
----------

In ODBC 3.\ *x*, **SQLFreeEnv** (a function in ODBC 2.\ *x*) was deprecated and replaced with **SQLFreeHandle**. For details, see :ref:`SQLFreeHandle <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section86888253574>`.

SQLPrepare
----------

**Function**

**SQLPrepare** prepares an SQL statement to be executed.

**Prototype**

::

   SQLRETURN SQLPrepare(SQLHSTMT      StatementHandle,
                        SQLCHAR       *StatementText,
                        SQLINTEGER    TextLength);

**Parameters**

.. table:: **Table 11** SQLPrepare parameters

   =============== ============================
   Parameter       Description
   =============== ============================
   StatementHandle Statement handle.
   StatementText   SQL text string.
   TextLength      Length of **StatementText**.
   =============== ============================

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

**Precautions**

If **SQLPrepare** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLGetData
----------

**Function**

**SQLGetData** retrieves data for a single column in the current row of the result set. It can be called multiple times to retrieve data of variable lengths.

**Prototype**

::

   SQLRETURN SQLGetData(SQLHSTMT        StatementHandle,
                        SQLUSMALLINT    Col_or_Param_Num,
                        SQLSMALLINT     TargetType,
                        SQLPOINTER      TargetValuePtr,
                        SQLLEN          BufferLength,
                        SQLLEN          *StrLen_or_IndPtr);

**Parameters**

.. table:: **Table 12** SQLGetData parameters

   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                                                                                                                    |
   +==================+================================================================================================================================================================================================================================================================================================================+
   | StatementHandle  | Statement handle, obtained from **SQLAllocHandle**.                                                                                                                                                                                                                                                            |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Col_or_Param_Num | Column number of the data to be returned. The columns in the result set are numbered from 1 in ascending order. The number of the bookmark column is 0.                                                                                                                                                        |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetType       | Type identifier of the C data type in the **TargetValuePtr** buffer. If **TargetType** is **SQL_ARD_TYPE**, the driver uses the data type of the **SQL_DESC_CONCISE_TYPE** field in ARD. If **TargetType** is **SQL_C_DEFAULT**, the driver selects a default data type according to the source SQL data type. |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TargetValuePtr   | **Output parameter**: pointer to the pointer that points to the buffer where the data is located.                                                                                                                                                                                                              |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength     | Size of the buffer pointed to by **TargetValuePtr**.                                                                                                                                                                                                                                                           |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StrLen_or_IndPtr | **Output parameter**: pointer to the buffer where the length or identifier value is returned.                                                                                                                                                                                                                  |
   +------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_NO_DATA** indicates that no result set is returned for the SQL statement.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.
-  **SQL_STILL_EXECUTING** indicates that the statement is being executed.

**Precautions**

If **SQLFetch** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

.. _en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343:

SQLGetDiagRec
-------------

**Function**

**SQLGetDiagRec** returns the current values of multiple fields of a diagnostic record that contains error, warning, and status information.

**Prototype**

::

   SQLRETURN  SQLGetDiagRec(SQLSMALLINT    HandleType,
                            SQLHANDLE      Handle,
                            SQLSMALLINT    RecNumber,
                            SQLCHAR        *SQLState,
                            SQLINTEGER     *NativeErrorPtr,
                            SQLCHAR        *MessageText,
                            SQLSMALLINT    BufferLength,
                            SQLSMALLINT    *TextLengthPtr);

**Parameters**

.. table:: **Table 13** SQLGetDiagRec parameters

   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                                                                                                                                                                        |
   +===================================+====================================================================================================================================================================================================================================================================================================================================+
   | HandleType                        | Handle type identifier that describes the handle type required for diagnosis. The value must be one of the following:                                                                                                                                                                                                              |
   |                                   |                                                                                                                                                                                                                                                                                                                                    |
   |                                   | -  SQL_HANDLE_ENV                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  SQL_HANDLE_DBC                                                                                                                                                                                                                                                                                                                  |
   |                                   | -  SQL_HANDLE_STMT                                                                                                                                                                                                                                                                                                                 |
   |                                   | -  SQL_HANDLE_DESC                                                                                                                                                                                                                                                                                                                 |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Handle                            | Handle of the diagnosis data structure. Its type is indicated by HandleType. If **HandleType** is **SQL_HANDLE_ENV**, **Handle** may be shared or non-shared environment handle.                                                                                                                                                   |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | RecNumber                         | Status record from which the application seeks information. Status records are numbered from 1.                                                                                                                                                                                                                                    |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | SQLState                          | **Output parameter**: pointer to a buffer that saves the 5-character **SQLSTATE** code pertaining to **RecNumber**.                                                                                                                                                                                                                |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | NativeErrorPtr                    | **Output parameter**: pointer to a buffer that saves the native error code.                                                                                                                                                                                                                                                        |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | MessageText                       | Pointer to a buffer that saves text strings of diagnostic information.                                                                                                                                                                                                                                                             |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BufferLength                      | Length of **MessageText**.                                                                                                                                                                                                                                                                                                         |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | TextLengthPtr                     | **Output parameter**: pointer to the buffer, the total number of bytes in the returned **MessageText**. If the number of bytes available to return is greater than **BufferLength**, then the diagnostics information text in **MessageText** is truncated to **BufferLength** minus the length of the null termination character. |
   +-----------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

**SQLGetDiagRec** does not release diagnostic records for itself. It uses the following returned values to report execution results:

-  **SQL_SUCCESS**: The function successfully returns diagnostic information.
-  **SQL_SUCCESS_WITH_INFO**: The **\*MessageText** buffer is too small to hold the requested diagnostic message and no diagnostic records are generated.
-  SQL_INVALID_HANDLE: The handle specified by **HandType** and **Handle** is invalid.
-  **SQL_ERROR**: **RecNumber** is smaller than or equal to zero, or **BufferLength** is smaller than zero.

If an ODBC function returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call **SQLGetDiagRec** and obtain the **SQLSTATE** value. The possible **SQLSTATE** values are listed as follows:

.. table:: **Table 14** SQLSTATE values

   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | SQLSATATE | Error                                | Description                                                                                                         |
   +===========+======================================+=====================================================================================================================+
   | HY000     | General error                        | An error occurred for which there is no specific **SQLSTATE**.                                                      |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | HY001     | Memory allocation error              | The driver is unable to allocate memory required to support execution or completion of the function.                |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | HY008     | Operation canceled                   | **SQLCancel** is called to terminate the statement execution, but the **StatementHandle** function is still called. |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | HY010     | Function sequence error              | The function is called prior to sending data to data parameters or columns being executed.                          |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | HY013     | Memory management error              | The function fails to be called. The error may be caused by low memory conditions.                                  |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | HYT01     | Connection timeout                   | The connection times out before the data source responds to the request.                                            |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | IM001     | Function not supported by the driver | A function that is not supported by the **StatementHandle** driver is called.                                       |
   +-----------+--------------------------------------+---------------------------------------------------------------------------------------------------------------------+

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLSetConnectAttr
-----------------

**Function**

**SQLSetConnectAttr** sets connection attributes.

**Prototype**

::

   SQLRETURN SQLSetConnectAttr(SQLHDBC       ConnectionHandle
                               SQLINTEGER    Attribute,
                               SQLPOINTER    ValuePtr,
                               SQLINTEGER    StringLength);

**Parameters**

.. table:: **Table 15** SQLSetConnectAttr parameters

   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                                                     |
   +==================+=================================================================================================================================================================================================================================================+
   | StatementtHandle | Connection handle.                                                                                                                                                                                                                              |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Attribute        | Attribute to set.                                                                                                                                                                                                                               |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ValuePtr         | Pointer to the value of **Attribute**. **ValuePtr** depends on the value of **Attribute** and can be a 32-bit unsigned integer value or a null-terminated string. If **ValuePtr** parameter is driver-specific value, it may be signed integer. |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLength     | If **ValuePtr** points to a string or a binary buffer, this parameter should be the length of **\*ValuePtr**. If **ValuePtr** points to an integer, **StringLength** is ignored.                                                                |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLSetConnectAttr** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLSetEnvAttr
-------------

**Function**

**SQLSetEnvAttr** sets environment attributes.

**Prototype**

::

   SQLRETURN SQLSetEnvAttr(SQLHENV       EnvironmentHandle
                           SQLINTEGER    Attribute,
                           SQLPOINTER    ValuePtr,
                           SQLINTEGER    StringLength);

**Parameters**

.. table:: **Table 16** SQLSetEnvAttr parameters

   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter                         | Description                                                                                                                                                                      |
   +===================================+==================================================================================================================================================================================+
   | EnvironmentHandle                 | Environment handle.                                                                                                                                                              |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Attribute                         | Environment attribute to be set. Its value must be one of the following:                                                                                                         |
   |                                   |                                                                                                                                                                                  |
   |                                   | -  **SQL_ATTR_ODBC_VERSION**: ODBC version                                                                                                                                       |
   |                                   | -  **SQL_CONNECTION_POOLING**: connection pool attribute                                                                                                                         |
   |                                   | -  **SQL_OUTPUT_NTS**: string type returned by the driver                                                                                                                        |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ValuePtr                          | Pointer to the value of **Attribute**. **ValuePtr** depends on the value of **Attribute** and can be a 32-bit integer value or a null-terminated string.                         |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLength                      | If **ValuePtr** points to a string or a binary buffer, this parameter should be the length of **\*ValuePtr**. If **ValuePtr** points to an integer, **StringLength** is ignored. |
   +-----------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLSetEnvAttr** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_ENV** and **EnvironmentHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.

SQLSetStmtAttr
--------------

**Function**

**SQLSetStmtAttr** sets attributes related to a statement.

**Prototype**

::

   SQLRETURN SQLSetStmtAttr(SQLHSTMT      StatementHandle
                            SQLINTEGER    Attribute,
                            SQLPOINTER    ValuePtr,
                            SQLINTEGER    StringLength);

**Parameters**

.. table:: **Table 17** SQLSetStmtAttr parameters

   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Parameter        | Description                                                                                                                                                                                                                                                                                                 |
   +==================+=============================================================================================================================================================================================================================================================================================================+
   | StatementtHandle | Statement handle.                                                                                                                                                                                                                                                                                           |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Attribute        | Attribute to set.                                                                                                                                                                                                                                                                                           |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | ValuePtr         | Pointer to the value of **Attribute**. **ValuePtr** depends on the value of **Attribute** and can be a 32-bit unsigned integer value or a pointer to a null-terminated string, a binary buffer, and a driver-specified value. If **ValuePtr** parameter is driver-specific value, it may be signed integer. |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | StringLength     | If **ValuePtr** points to a string or a binary buffer, this parameter should be the length of **\*ValuePtr**. If **ValuePtr** points to an integer, **StringLength** is ignored.                                                                                                                            |
   +------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Return values**

-  **SQL_SUCCESS** indicates that the call is successful.
-  **SQL_SUCCESS_WITH_INFO** indicates warning information.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection setup failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the values returned by the API you have used.

**Precautions**

If **SQLSetStmtAttr** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <en-us_topic_0000002044151968__en-us_topic_0000001188163746_section2084121410343>`, set **HandleType** and **Handle** to **SQL_HANDLE_STMT** and **StatementHandle**, and obtain the **SQLSTATE** value. This value can be used to get more information about the function call.

**Examples**

See :ref:`ODBC Development Example <dws_04_0123>`.
