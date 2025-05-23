:original_name: dws_06_0156.html

.. _dws_06_0156:

CREATE DATABASE
===============

Function
--------

**CREATE DATABASE** creates a database. By default, the new database will be created by cloning the standard system database template1. A different template can be specified using **TEMPLATE** *template name*.

Precautions
-----------

-  A user that has the **CREATEDB** permission or a sysadmin can create a database.
-  **CREATE DATABASE** cannot be executed inside a transaction block.
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

-  **database_name**

   Indicates the database name.

   Value range: a string that complies with the naming convention, which includes uppercase and lowercase letters, digits, underscores (_), and dollar signs ($). However, it cannot begin with a digit or dollar sign ($).

-  **OWNER [ = ] user_name**

   Indicates the owner of the new database. By default, the owner of the database is the current user.

   Value range: an existing user name

-  **TEMPLATE [ = ] template**

   Indicates the template name, that is, the name of the template to be used to create the database. GaussDB(DWS) creates a database by coping a database template. GaussDB(DWS) has two default template databases **template0** and **template1** and a default user database **gaussdb**.

   Value range: An existing database name. If this it is not specified, the system copies **template1** by default. Its value cannot be **gaussdb**.

   .. important::

      Currently, database templates cannot contain sequences. If a database template contains sequences, database creation using this template will fail.

-  **ENCODING [ = ] encoding**

   Specifies the encoding format used by the new database. The value can be a string (for example, **SQL_ASCII**) or an integer.

   By default, the encoding format of the template database is used. The encoding formats of the template databases **template0** and **template1** vary based on OS environments by default. The **template1** database does not allow encoding customization. To specify encoding for a database when creating it, use **template0**. To specify encoding, set **template** to **template0**.

   Common values: **GBK**, **UTF8**, and **Latin1**

   .. important::

      -  To view the character encoding of the current database, run the **show server_encoding;** command.
      -  To make your database compatible with most characters, you are advised to use the UTF8 encoding when creating a database.

      -  The character set encoding of the new database must be compatible with the local settings (**LC_COLLATE** and **LC_CTYPE**).
      -  When the specified character encoding set is **GBK**, some uncommon Chinese characters cannot be directly used as object names. This is because when the encoding range of the second byte of GBK is between 0x40 and 0x7E, the byte encoding overlaps with the ASCII character @A-Z[\\]^_`a-z{|}. ``@[\]^_?{|}`` is an operator in the database. If it is directly used as an object name, a syntax error will be reported. For example, the GBK hexadecimal code is **0x8240**, and the second byte is **0x40**, which is the same as the ASCII character @. Therefore, the character cannot be used as an object name. If you really want to use it, you can avoid this problem by adding double quotation marks when creating and accessing objects.
      -  In the current version, the GBK character set supports the character **€**, which is represented as **0x80** in hexadecimal code. You can use the **€** character in the GBK library, and the GBK character set of GaussDB(DWS) is compatible with the CP936 character set. Note that the GBK character set is approximately equal to the CP936 character set, but the GBK character set does not contain the definition of the character **€**.

-  **LC_COLLATE [ = ] lc_collate**

   Specifies the collation order to use in the new database. For example, this parameter can be set using lc_collate = 'zh_CN.gbk'.

   The use of this parameter affects the sort order applied to strings, for example, in queries with **ORDER BY**, as well as the order used in indexes on text columns. The default is to use the collation order of the template database.

   Value range: A valid order type.

-  **LC_CTYPE [ = ] lc_ctype**

   Specifies the character classification to use in the new database. For example, this parameter can be set using lc_ctype = 'zh_CN.gbk'. The use of this parameter affects the categorization of characters, for example, lower, upper and digit. The default is to use the character classification of the template database.

   Value range: A valid character type.

-  **DBCOMPATIBILITY [ = ] compatibility_type**

   Specifies the compatible database type.

   Value range: **ORA**, **TD**, and **MySQL**, representing the Oracle-, Teradata-, and MySQL-compatible modes, respectively. If this parameter is not specified, the default value **ORA** is used.

-  **DBCOMPATIBILITY_BEHAVOIR [ = ] opt_compat\_behavior**

   Specifies the compatibility behavior of the database. This parameter is supported only by clusters of version 9.1.0 or later. If this parameter is not specified, the default value **NO_BEHAVIOR** is used, indicating no special behavior.

   The value can be **td_rtrim** or **pg_char**.

   **td_rtrim**: removes the trailing part of a variable-length character string in Teradata compatibility mode.

   **pg_char**: converts the varchar type to the nvarchar2 type in PostgreSQL compatibility mode.

-  **CONNECTION LIMIT [ = ] connlimit**

   Number of concurrent connections to a database.

   Value range: An integer greater than or equal to **-1**. The default value **-1** means no limit.

   .. important::

      -  This limit does not apply to sysadmin.
      -  To ensure the proper running of a cluster, the minimum value of **CONNECTION LIMIT** is the number of CNs in the cluster, because when a cluster runs ANALYZE on a CN, other CNs will connect with the running CN for metadata synchronization. For example, if there are three CNs in the cluster, set **CONNECTION LIMIT** to **3** or a larger value.

The following are limitations on character encoding:

-  If the locale is **C** (or equivalently **POSIX**), then all encoding modes are allowed, but for other locale settings only the encoding consistent with that of the locale will work properly.
-  The encoding and locale settings must match those of the template database, except when template0 is used as template. This is because other databases might contain data that does not match the specified encoding, or might contain indexes whose sort ordering is affected by **LC_COLLATE** and **LC_CTYPE**. Copying such data would result in a database that is corrupt according to the new settings. template0, however, is known to not contain any data or indexes that would be affected.
-  Supported encoding depends on the environment. If the message "invalid locale name" is displayed, run the **locale -a** command to check the encoding set supported by the environment.

Examples
--------

Create database **music** using GBK (the local encoding type is also GBK).

::

   CREATE DATABASE music ENCODING 'GBK' template = template0;

Create database **music2** and specify **jim** as its owner.

::

   CREATE DATABASE music2 OWNER jim;

Create database **music3** using template **template0** and specify **jim** as its owner.

::

   CREATE DATABASE music3 OWNER jim TEMPLATE template0;

Create a database compatible with Oracle.

::

   CREATE DATABASE ora_compatible_db DBCOMPATIBILITY 'ORA';

Helpful Links
-------------

:ref:`ALTER DATABASE <dws_06_0120>`, :ref:`DROP DATABASE <dws_06_0189>`
