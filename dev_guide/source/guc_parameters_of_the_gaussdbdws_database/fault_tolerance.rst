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

**Parameter description**: When performing character encoding conversion in the database, if a character encoding error occurs and the target character set encoding is UTF-8, the converted character with the error can be ignored and replaced with "?".

**Type**: USERSET

**Value range**: Boolean

-  **on** indicates that characters that have conversion errors will be ignored and replaced with question marks (?), and error information will be recorded in logs.
-  **off** indicates that characters that have conversion errors cannot be converted and error information will be directly displayed.

**Default value**: **off**

.. _en-us_topic_0000001811490753__sc9cd4d1562654b6ebb842765d3e398e4:

max_query_retry_times
---------------------

**Parameter description**: Specifies the maximum number of retries for the automatic retry feature when a SQL statement encounters an error. Currently, the supported error types for retry include **Connection reset by peer**, **Lock wait timeout**, and **Connection timed out**. Setting this parameter to **0** will disable the retry feature.

**Type**: USERSET

**Value range**: an integer ranging from 0 to 20

**Default value**: **6**

retry_ecode_list
----------------

**Parameter description**: Specifies the list of SQL error types that support automatic retry.

**Type**: USERSET

**Value range**: a string

**Default value**: YY001 YY002 YY003 YY004 YY005 YY006 YY007 YY008 YY009 YY010 YY011 YY012 YY013 YY014 YY015 53200 08006 08000 57P01 XX003 XX009 YY016 CG003 CG004 F0011 F0012 45003 42P30
