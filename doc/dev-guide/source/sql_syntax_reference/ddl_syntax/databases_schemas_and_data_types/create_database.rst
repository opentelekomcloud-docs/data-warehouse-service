:original_name: dws_06_0156.html

.. _dws_06_0156:

CREATE DATABASE
===============

Function
--------

**CREATE DATABASE** creates a database.

DWS creates a database by copying a database template. DWS has two default template databases **template0** and **template1** and a default user database **gaussdb**. By default, a new database is created based on **template1**. The character encoding of **template1** cannot be changed. If you want to change it, you can set the **template** parameter to **template0**.

Precautions
-----------

-  A user that has the **CREATEDB** permission or a sysadmin can create a database.
-  **CREATE DATABASE** cannot be executed within a transaction block.
-  Errors along the line of "could not initialize database directory" are most likely related to insufficient permissions on the data directory, a full disk, or other file system problems.

Syntax
------

::

   CREATE DATABASE database_name
       [ [ WITH ] { [ OWNER [=] user_name ] |
                  [ TEMPLATE [=] template ] |
                  [ ENCODING [=] encoding ] |
                  [ LC_COLLATE [=] lc_collate ] |
                  [ LC_CTYPE [=] lc_ctype ] |
                  [ DBCOMPATIBILITY [=] compatibility_type ] |
                  [ DBCOMPATIBILITY_BEHAVIOR [=] opt_compat_behavior] |

                  [ CONNECTION LIMIT [=] connlimit ]}[...] ];

Parameter Description
---------------------

.. table:: **Table 1** CREATE DATABASE parameters

   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | Parameter             | Description                                                                                                                                                                                                                                                                                                                                                        | Value Range                                                                                                         |
   +=======================+====================================================================================================================================================================================================================================================================================================================================================================+=====================================================================================================================+
   | database_name         | Name of the database to be created                                                                                                                                                                                                                                                                                                                                 | A string, which must comply with the :ref:`naming convention <en-us_topic_0000001811634529__section1475018612353>`. |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | user_name             | Specifies the owner of the new database. By default, the owner of the database is the current user.                                                                                                                                                                                                                                                                | An existing user name.                                                                                              |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | template              | Specifies the name of the template used to create a database. If it is not specified, the system uses **template1** by default.                                                                                                                                                                                                                                    | Value range: **template0** or **template1**.                                                                        |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    | -  **template0**: The character set encoding can be specified if **template0** is used.                             |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    | -  **template1** (default value): The character set encoding cannot be specified if **template1** is used.          |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | encoding              | Specifies the character set encoding used by the new database. The value can be a string (for example, **SQL_ASCII**) or an integer. By default, the encoding format of the template database is used.                                                                                                                                                             | Common values: **GBK**, **UTF8**, **Latin1**, and **SQL_ASCII**.                                                    |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       | The encoding formats of the template databases **template0** and **template1** vary based on OS environments by default.                                                                                                                                                                                                                                           |                                                                                                                     |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | lc_collate            | Specifies the collation order to use in the new database. For example, this parameter can be set using lc_collate = 'zh_CN.gbk'.                                                                                                                                                                                                                                   | Valid collation order                                                                                               |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       | The use of this parameter affects the sort order applied to strings, for example, in queries with **ORDER BY**, as well as the order used in indexes on text columns. The default is to use the collation order of the template database. For other restrictions, see :ref:`Character Set Encoding Description <en-us_topic_0000001764516262__section0930870529>`. |                                                                                                                     |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | lc_ctype              | Specifies the character classification to use in the new database. For example, this parameter can be set using lc_ctype = 'zh_CN.gbk'.                                                                                                                                                                                                                            | Valid character classification                                                                                      |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       | The use of this parameter affects the categorization of characters, for example, lower, upper and digit. The default is to use the character classification of the template database.                                                                                                                                                                              |                                                                                                                     |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | compatibility_type    | Specifies the compatible database type. If this parameter is not specified, the default value **ORA** is used.                                                                                                                                                                                                                                                     | The value can be **ORA**, **TD**, or **MySQL**,                                                                     |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    | indicating Oracle, Teradata, or MySQL database, respectively.                                                       |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | opt_compat_behavior   | Specifies the compatibility behavior of the database. This parameter is supported only by clusters of version 9.1.0 or later.                                                                                                                                                                                                                                      | The value can be **td_rtrim** or **pg_char**.                                                                       |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       | If this parameter is not specified, the default value **NO_BEHAVIOR** is used, indicating no special behavior.                                                                                                                                                                                                                                                     | -  **td_rtrim**: removes the trailing part of a variable-length character string in Teradata compatibility mode.    |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    | -  **pg_char**: converts the varchar type to the nvarchar2 type in PostgreSQL compatibility mode.                   |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+
   | connlimit             | Number of concurrent connections to a database. The default value **-1** means no limit.                                                                                                                                                                                                                                                                           | An integer greater than or equal to -1                                                                              |
   |                       |                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                     |
   |                       | -  This limit does not apply to system administrators.                                                                                                                                                                                                                                                                                                             |                                                                                                                     |
   |                       | -  To ensure that a cluster runs properly, the minimum value of **CONNECTION LIMIT** is the number of CNs in the cluster. because when a cluster runs ANALYZE on a CN, other CNs will connect to the running CN for metadata synchronization. For example, if there are three CNs in the cluster, set **CONNECTION LIMIT** to **3** or a larger value.             |                                                                                                                     |
   +-----------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------------------------------------------------------------+

.. _en-us_topic_0000001764516262__section0930870529:

Character Set Encoding Description
----------------------------------

-  To view the character encoding of the current database, run the **show server_encoding;** command.
-  To make your database compatible with most characters, you are advised to use the UTF-8 encoding when creating a database.

-  The character set encoding of the new database must be compatible with the language settings (**LC_COLLATE** and **LC_CTYPE**).
-  If the language is **C** (or equivalently **POSIX**), then all encoding modes are allowed, but for other language settings only the encoding mode for character sets consistent with that of the locale will work properly.
-  The encoding and locale settings must match those of the template database, except when template0 is used as template. This is because other databases might contain data that does not match the specified encoding, or might contain indexes whose sort ordering is affected by **LC_COLLATE** and **LC_CTYPE**. Copying such data would result in a database that is corrupt according to the new settings. template0, however, is known to not contain any data or indexes that would be affected.
-  Supported encoding depends on the environment. If the message "invalid locale name" is displayed, run the **locale -a** command to check the encoding set supported by the environment.
-  When the specified character encoding set is **GBK**, some uncommon Chinese characters cannot be directly used as object names. This is because when the encoding range of the second byte of GBK is between 0x40 and 0x7E, the byte encoding overlaps with the ASCII character @A-Z[\\]^_`a-z{|}. ``@[\]^_?{|}`` is an operator in the database. If it is directly used as an object name, a syntax error will be reported. For example, the GBK hexadecimal code is **0x8240**, and the second byte is **0x40**, which is the same as the ASCII character @. Therefore, the character cannot be used as an object name. If you really want to use it, you can avoid this problem by adding double quotation marks when creating and accessing objects.
-  In the current version, the GBK character set supports the character **€**, which is represented as **0x80** in hexadecimal code. You can use the **€** character in the GBK library, and the GBK character set of DWS is compatible with the CP936 character set. Note that the GBK character set is approximately equal to the CP936 character set, but the GBK character set does not contain the definition of the character **€**.

Examples
--------

Create database **music** using GBK (the local encoding type is also GBK).

::

   CREATE DATABASE music ENCODING 'GBK' template = template0;

Create database **music2** and specify **jim** as its owner.

::

   CREATE USER jim PASSWORD "{Password}";
   CREATE DATABASE music2 OWNER jim;

Create database **music3** using template **template0** and specify **jim** as its owner.

::

   CREATE DATABASE music3 OWNER jim TEMPLATE template0;

Create a database compatible with Oracle and set the connection limit to **10**.

::

   CREATE DATABASE ora_compatible_db DBCOMPATIBILITY 'ORA' CONNECTION LIMIT = 10;

Clean up the environment.

::

   DROP DATABASE music;
   DROP DATABASE music2;
   DROP DATABASE music3;
   DROP DATABASE ora_compatible_db;

Helpful Links
-------------

:ref:`ALTER DATABASE <dws_06_0120>`, :ref:`DROP DATABASE <dws_06_0189>`

FAQs
----

For details, see "FAQs" > "Database Use" > "How Do I View and Set the Character Set Encoding Format of a DWS Database?" in *Data Warehouse Service (DWS) User Guide*.
