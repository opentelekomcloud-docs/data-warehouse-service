:original_name: dws_03_0085.html

.. _dws_03_0085:

How Can I Change the Character Encoding Format of a Database?
=============================================================

In GaussDB(DWS), the encoding format of a database cannot be changed. You need to create another database in the required format. For globalization purposes, set the encoding to UTF8 when creating a database.

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
                  [ CONNECTION LIMIT [=] connlimit ]}[...] ];

-  **TEMPLATE [ = ] template**

   Name of the template from which the database is created. GaussDB(DWS) creates a database by copying a template database. Initially, GaussDB(DWS) contains two template databases **template0** and **template1**, and a default user database **gaussdb**.

   Value range: an existing database name. If this is not specified, the system copies **template1** by default. Its value cannot be **gaussdb**.

   .. important::

      Currently, database templates cannot contain sequences. If sequences exist in the template library, database creation will fail.

-  **ENCODING [ = ] encoding**

   Character encoding used by the database. The value can be a character string (for example, **SQL_ASCII'**) or an integer number.

   If this parameter is not specified, the encoding of the template database is used by default. The encoding of template databases **template0** and **template1** depends on the OS by default. The character encoding of **template1** cannot be changed. To change the encoding, use **template0** to create a database.

   Value range: **GBK**, **UTF8**, and **Latin1**

   .. important::

      The character set encoding of the new database must be compatible with the local settings (**LC_COLLATE** and **LC_CTYPE**).

Example
-------

To create a UTF8 database **music** that can be modified later. (The encoding of the local environment must also be UTF8.)

.. code-block::

   CREATE DATABASE music ENCODING 'UTF8' template = template0;
