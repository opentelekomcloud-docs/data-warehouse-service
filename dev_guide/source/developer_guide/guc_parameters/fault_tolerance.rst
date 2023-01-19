:original_name: dws_04_0932.html

.. _dws_04_0932:

Fault Tolerance
===============

This section describes parameters used for controlling the methods that the server processes an error occurring in the database system.

exit_on_error
-------------

**Parameter description**: Specifies whether to terminate the current session.

**Type**: SUSET

**Value range**: Boolean

-  **on** indicates that any error will terminate the current session.
-  **off** indicates that only a FATAL error will terminate the current session.

**Default value**: **off**

omit_encoding_error
-------------------

**Parameter description**: If this parameter is set to **on** and the client character set of the database is encoded in UTF-8 format, the occurring character encoding conversion errors will be recorded in logs. Additionally, converted characters that have conversion errors will be ignored and replaced with question marks (?).

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that characters that have conversion errors will be ignored and replaced with question marks (?), and error information will be recorded in logs.
-  **off** indicates that characters that have conversion errors cannot be converted and error information will be directly displayed.

**Default value**: **off**

max_query_retry_times
---------------------

**Parameter description**: Specifies the maximum number of automatic retry times when an SQL statement error occurs. Currently, a statement can start retrying if the following errors occur: **Connection reset by peer**, **Lock wait timeout**, and **Connection timed out**. If this parameter is set to **0**, the retry function is disabled.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 20

**Default value**: **6**

cn_send_buffer_size
-------------------

**Parameter description**: Specifies the size of the data buffer used for data transmission on the CN.

**Type**: POSTMASTER

**Value range**: an integer ranging from 8 to 128. The unit is KB.

**Default value**: **8 KB**

max_cn_temp_file_size
---------------------

**Parameter description**: Specifies the maximum number of temporary files that can be used by the CN during automatic SQL statement retries. The value **0** indicates that no temporary file is used.

**Type**: SIGHUP

**Value range**: an integer ranging from 0 to 10485760. The unit is KB.

**Default value**: **5 GB**

retry_ecode_list
----------------

**Parameter description**: Specifies the list of SQL error types that support automatic retry.

**Type**: USERSET

**Value range**: a string

**Default value**: YY001 YY002 YY003 YY004 YY005 YY006 YY007 YY008 YY009 YY010 YY011 YY012 YY013 YY014 YY015 53200 08006 08000 57P01 XX003 XX009 YY016 CG003 CG004 F0011

data_sync_retry
---------------

**Parameter description**: Specifies whether to keep running the database when updated data fails to be written into disks by using the **fsync** function. In some OSs, no error is reported even if **fsync** has failed for multiple times. As a result, data is lost.

**Type**: POSTMASTER

**Value range**: Boolean

-  **on**: The database keeps running and **fsync** is executed again after **fsync** fails.
-  **off**: **PANIC** is reported and the database is stopped after **fsync** fails.

**Default value**: **off**
