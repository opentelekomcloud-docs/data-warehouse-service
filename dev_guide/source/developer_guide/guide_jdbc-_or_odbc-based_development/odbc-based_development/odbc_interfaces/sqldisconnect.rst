:original_name: dws_04_0133.html

.. _dws_04_0133:

SQLDisconnect
=============

Function
--------

**SQLDisconnect** closes the connection associated with the database connection handle.

Prototype
---------

::

   SQLRETURN SQLDisconnect(SQLHDBC    ConnectionHandle);

Parameter
---------

.. table:: **Table 1** SQLDisconnect parameters

   ================ ====================================================
   Keyword          Description
   ================ ====================================================
   ConnectionHandle Connection handle, obtained from **SQLAllocHandle**.
   ================ ====================================================

Return Values
-------------

-  **SQL_SUCCESS** indicates that the call succeeded.
-  **SQL_SUCCESS_WITH_INFO** indicates some warning information is displayed.
-  **SQL_ERROR** indicates major errors, such as memory allocation and connection failures.
-  **SQL_INVALID_HANDLE** indicates that invalid handles were called. Values returned by other APIs are similar to the preceding values.

Precautions
-----------

If **SQLDisconnect** returns **SQL_ERROR** or **SQL_SUCCESS_WITH_INFO**, the application can then call :ref:`SQLGetDiagRec <dws_04_0143>`, set **HandleType** and **Handle** to **SQL_HANDLE_DBC** and **ConnectionHandle**, and obtain the **SQLSTATE** value. The **SQLSTATE** value provides the detailed function calling information.

Examples
--------

See :ref:`Examples <dws_04_0123>`.
