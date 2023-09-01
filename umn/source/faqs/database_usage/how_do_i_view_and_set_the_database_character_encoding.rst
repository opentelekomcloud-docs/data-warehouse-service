:original_name: dws_03_0085.html

.. _dws_03_0085:

How Do I View and Set the Database Character Encoding?
======================================================

Viewing the Database Character Encoding
---------------------------------------

Use the **server_encoding** parameter to check the character set encoding of the current database. For example, the character encoding of database **music** is UTF8.

::

   music=> show server_encoding;
    server_encoding
   -----------------
    UTF8
   (1 row)

Setting the Database Character Encoding
---------------------------------------

.. note::

   GaussDB(DWS) does not support the modification of the character encoding format of a created database.

If you need to specify the character encoding format of a database, use **template0** and the **CREATE DATABASE** syntax to create a database. To make your database compatible with most characters, you are advised to use the UTF8 encoding when creating a database.

CREATE DATABASE syntax
----------------------

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

   Indicates the template name, that is, the name of the template to be used to create the database. GaussDB(DWS) creates a database by coping a database template. GaussDB(DWS) has two initial template databases **template0** and **template1** and a default user database **postgres**.

   Value range: an existing database name. If this is not specified, the system copies **template1** by default. Its value cannot be **postgres**.

   .. important::

      Currently, database templates cannot contain sequences. If sequences exist in the template library, database creation will fail.

-  **ENCODING [ = ] encoding**

   Character encoding used by the database. The value can be a character string (for example, **SQL_ASCII'**) or an integer number.

   If this parameter is not specified, the encoding of the template database is used by default. The encoding of template databases **template0** and **template1** depends on the OS by default. The character encoding of **template1** cannot be changed. To change the encoding, use **template0** to create a database.

   Value range: **GBK**, **UTF8**, and **Latin1**

   .. important::

      The character set encoding of the new database must be compatible with the local settings (**LC_COLLATE** and **LC_CTYPE**).

Examples
--------

Create database **music** using UTF8 (the local encoding type is also UTF8).

::

   CREATE DATABASE music ENCODING 'UTF8' template = template0;
