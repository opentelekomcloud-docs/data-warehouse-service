:original_name: dws_06_0012.html

.. _dws_06_0012:

Character Types
===============

:ref:`Table 1 <en-us_topic_0000001145830639__t0c62e4928aa34bdbb99c6b9fe3c6996b>` lists the character types that can be used in GaussDB(DWS). For string operators and related built-in functions, see :ref:`Character Processing Functions and Operators <dws_06_0030>`.

.. _en-us_topic_0000001145830639__t0c62e4928aa34bdbb99c6b9fe3c6996b:

.. table:: **Table 1** Character types

   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | Name                 | Description                                                                                      | Length                                                                                                                                        | Storage Space                                                |
   +======================+==================================================================================================+===============================================================================================================================================+==============================================================+
   | CHAR(n)              | Fixed-length character string. If the length is not reached, fill in spaces.                     | **n** indicates the string length. If it is not specified, the default precision **1** is used. The value of **n** is less than **10485761**. | The maximum size is 10 MB.                                   |
   |                      |                                                                                                  |                                                                                                                                               |                                                              |
   | CHARACTER(n)         |                                                                                                  |                                                                                                                                               |                                                              |
   |                      |                                                                                                  |                                                                                                                                               |                                                              |
   | NCHAR(n)             |                                                                                                  |                                                                                                                                               |                                                              |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | VARCHAR(n)           | Variable-length string.                                                                          | **n** indicates the byte length. The value of **n** is less than **10485761**.                                                                | The maximum size is 10 MB.                                   |
   |                      |                                                                                                  |                                                                                                                                               |                                                              |
   | CHARACTER VARYING(n) |                                                                                                  |                                                                                                                                               |                                                              |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | VARCHAR2(n)          | Variable-length string. It is an alias for VARCHAR(n) type, compatible with Oracle.              | **n** indicates the byte length. The value of **n** is less than **10485761**.                                                                | The maximum size is 10 MB.                                   |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | NVARCHAR2(n)         | Variable-length string.                                                                          | **n** indicates the string length. The value of **n** is less than **10485761**.                                                              | The maximum size is 10 MB.                                   |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | CLOB                 | Variable-length string. A big text object. It is an alias for TEXT type, compatible with Oracle. | ``-``                                                                                                                                         | The maximum size is 1,073,733,621 bytes (1 GB - 8203 bytes). |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+
   | TEXT                 | Variable-length string.                                                                          | ``-``                                                                                                                                         | The maximum size is 1,073,733,621 bytes (1 GB - 8203 bytes). |
   +----------------------+--------------------------------------------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------+

.. note::

   -  In addition to the size limitation on each column, the total size of each tuple is 1,073,733,621 bytes (1 GB - 8023 bytes).
   -  For string data, you are advised to use variable-length strings and specify the maximum length. To avoid truncation, ensure that the specified maximum length is greater than the maximum number of characters to be stored. You are not advised to use fixed-length character types such as CHAR(n), NCHAR(n), and CHARACTER(n) unless you know that the data type is a fixed-length character string.

GaussDB(DWS) has two other fixed-length character types, as listed in :ref:`Table 2 <en-us_topic_0000001145830639__ta7a9f8927f4b419e9d1c0a01d2dac911>`.

The name type is used only in the internal system catalog as the storage identifier. The length of this type is 64 bytes (63 characters plus the terminator). This data type is not recommended for common users. When the name type is aligned with other data types (for example, in multiple branches of **case when**, one branch returns the name type and other branches return the text type), the name type may be aligned but characters may be truncated. If you do not want to have 64-bit truncated characters, you need to forcibly convert the name type to the text type.

The type **"char"** only uses one byte of storage. It is internally used in the system catalogs as a simplistic enumeration type.

.. _en-us_topic_0000001145830639__ta7a9f8927f4b419e9d1c0a01d2dac911:

.. table:: **Table 2** Special character types

   ====== ============================== =============
   Name   Description                    Storage Space
   ====== ============================== =============
   name   Internal type for object names 64 bytes
   "char" Single-byte internal type      1 byte
   ====== ============================== =============

Length
------

If a field is defined as **char(n)** or **varchar(n)**. **n** indicates the maximum length. Regardless of the type, the length can not exceed 10485760 bytes (10 MB).

When the data length exceeds the specified length **n**, the error "value too long" is reported. Of course, you can also specify to automatically truncate the data that exceeds the length.

Example:

#. Create table **t1** and specify the character type of its columns.

   ::

      CREATE TABLE t1 (a char(5),b varchar(5));

#. An error is reported when the length of data inserted into the table **t1** exceeds the specified length.

   ::

      INSERT INTO t1 VALUES('bookstore','123');
      ERROR:  value too long for type character(5)
      CONTEXT:  referenced column: a

#. Insert data into table **t1** and specify that the data is automatically truncated when the length exceeds the specified bytes.

   ::

      INSERT INTO t1 VALUES('bookstore'::char(5),'12345678'::varchar(5));
      INSERT 0 1

      SELECT a,b FROM t1;
         a   |   b
      -------+-------
       books | 12345
      (1 row)

Fixed Length and Variable Length
--------------------------------

All character types can be classified into fixed-length strings and variable-length strings.

-  For a fixed-length string, the length must be specified. If the length is not specified, the default length **1** is used. If the data length does not reach the specified length, spaces are automatically added to the end of the string. However, the added spaces are meaningless and will be ignored in actual use, such as comparison, sorting, and type conversion.
-  For a variable-length string, if the length is specified, the specified length indicates the maximum length of the data that can be stored. If the length is not specified, it means any length is available.

Example:

#. Create table **t2** and specify the character type of its columns.

   ::

      CREATE TABLE t2 (a char(5),b varchar(5));

#. Insert data into table **t2** and query the byte length of column **a**. During table creation, the character type of column **a** is specified as **char(5)** and fixed-length. If the data length does not reach 5 bytes, spaces are added. Therefore, the queried data length is **5**.

   ::

      INSERT INTO t2 VALUES('abc','abc');
      INSERT 0 1

      SELECT a,lengthb(a),b FROM t2;
         a   | lengthb |  b
      -------+---------+-----
       abc   |       5 | abc
      (1 row)

#. After the conversion by using the function, the actual queried length of the column **a** is **3** bytes.

   ::

      SELECT a = b from t2;
       ?column?
      ----------
       t
      (1 row)

      SELECT cast(a as text) as val,lengthb(val) FROM t2;
       val | lengthb
      -----+---------
       abc |       3
      (1 row)

Bytes and Characters
--------------------

**n** means differently in **VARCHAR2(n)** and **NVARCHAR2(n)**.

-  In **VARCHAR2(n)** **n** indicates the number of bytes.
-  In **NVARCHAR2(n)**, **n** indicates the number of characters.

   .. note::

      Take an UTF8-encoded database as an example. A letter occupies one byte, and a Chinese character occupies three bytes. **VARCHAR2(6)** allows for six letters or two Chinese characters, and **NVARCHAR2(6)** allows for six letters or six Chinese characters.

Empty Strings and NULL
----------------------

In Oracle compatibility mode, empty strings and NULL are not distinguished. When a statement is executed to query or import data, empty strings are processed as NULL.

As such, **= "** cannot be used as the query condition, and so does **is ''**. Otherwise, no result set is returned. The correct usage is **is null**, or **is not null**.

Example:

#. Create table **t4** and specify the character type of its columns.

   ::

      CREATE TABLE t4 (a text);

#. Insert data into table **t4**. The inserted value contains an empty string and NULL.

   ::

      INSERT INTO t4 VALUES('abc'),(''),(null);
      INSERT 0 3

#. Check whether **t4** contains null values.

   ::

      SELECT a,a isnull FROM t4;
        a  | ?column?
      -----+----------
           | t
           | t
       abc | f
      (3 rows)

      SELECT a,a isnull FROM t4 WHERE a is null;
       a | ?column?
      ---+----------
         | t
         | t
      (2 rows)
