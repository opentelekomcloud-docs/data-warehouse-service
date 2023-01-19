:original_name: dws_06_0009.html

.. _dws_06_0009:

Numeric Types
=============

Numeric types consist of two-, four-, and eight-byte integers, four- and eight-byte floating-point numbers, and selectable-precision decimals.

For details about numeric operators and functions, see :ref:`Mathematical Functions and Operators <dws_06_0034>`.

GaussDB(DWS) supports integers, arbitrary precision numbers, floating point types, and serial integers.

Integer Types
-------------

The types TINYINT, SMALLINT, INTEGER, BINARY_INTEGER, and BIGINT store whole numbers, that is, numbers without fractional components, of various ranges. Saving a number with a decimal in any of the data types will result in errors.

The type INTEGER is the common choice. Generally, use the SMALLINT type only if you are sure that the value range is within the SMALLINT value range. The storage speed of INTEGER is much faster. BIGINT is used only when the range of INTEGER is not large enough.

.. table:: **Table 1** Integer types

   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+
   | Column         | Description                                  | Storage Space | Range                                                  |
   +================+==============================================+===============+========================================================+
   | TINYINT        | Tiny integer, also called INT1               | 1 byte        | 0 ~ 255                                                |
   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+
   | SMALLINT       | Small integer, also called INT2              | 2 bytes       | -32,768 ~ +32,767                                      |
   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+
   | INTEGER        | Typical choice for integer, also called INT4 | 4 bytes       | -2,147,483,648 ~ +2,147,483,647                        |
   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+
   | BINARY_INTEGER | INTEGER alias, compatible with Oracle        | 4 bytes       | -2,147,483,648 ~ +2,147,483,647                        |
   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+
   | BIGINT         | Big integer, also called INT8                | 8 bytes       | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807 |
   +----------------+----------------------------------------------+---------------+--------------------------------------------------------+

Examples:

Create a table containing TINYINT, INTEGER, and BIGINT data.

::

   CREATE TABLE int_type_t1
   (
       a TINYINT,
       b TINYINT,
       c INTEGER,
       d BIGINT
   );

Insert data.

::

   INSERT INTO int_type_t1 VALUES(100, 10, 1000, 10000);

View data.

::

   SELECT * FROM int_type_t1;
     a  | b  |  c   |   d
   -----+----+------+-------
    100 | 10 | 1000 | 10000
   (1 row)

Arbitrary Precision Types
-------------------------

The type NUMBER can store numbers with a very large number of digits. It is especially recommended for storing monetary amounts and other quantities where exactness is required. The arbitrary precision numbers require larger storage space and have lower storage efficiency, operation efficiency, and poorer compression ratio results than integer types.

The scale of a NUMBER value is the count of decimal digits in the fractional part, to the right of the decimal point. The precision of a NUMBER value is the total count of significant digits in the whole number, that is, the number of digits to both sides of the decimal point. So the number 23.5141 has a precision of 6 and a scale of 4. Integers can be considered to have a scale of zero.

To configure a numeric or decimal column, you are advised to specify both the maximum precision (p) and the maximum scale (s) of the column.

If the precision or scale of a value is greater than the declared scale of the column, the system will round the value to the specified number of fractional digits. Then, if the number of digits to the left of the decimal point exceeds the declared precision minus the declared scale, an error will be reported.

.. table:: **Table 2** Any-precision types

   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | Column            | Description                                                                                 | Storage Space                                                                                                                                  | Range                                                                                                                         |
   +===================+=============================================================================================+================================================================================================================================================+===============================================================================================================================+
   | NUMERIC[(p[,s])], | The value range of p (precision) is [1,1000], and the value range of s (standard) is [0,p]. | The precision is specified by users. Every four decimal digits occupy two bytes, and an extra eight-byte overhead is added to the entire data. | Up to 131,072 digits before the decimal point; and up to 16,383 digits after the decimal point when no precision is specified |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                               |
   | DECIMAL[(p[,s])]  |                                                                                             |                                                                                                                                                |                                                                                                                               |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------+
   | NUMBER[(p[,s])]   | Alias for type NUMERIC, compatible with Oracle                                              | The precision is specified by users. Every four decimal digits occupy two bytes, and an extra eight-byte overhead is added to the entire data. | Up to 131,072 digits before the decimal point; and up to 16,383 digits after the decimal point when no precision is specified |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------+

Examples:

Create a table with DECIMAL values.

::

   CREATE TABLE decimal_type_t1 (DT_COL1 DECIMAL(10,4));

Insert data.

::

   INSERT INTO decimal_type_t1 VALUES(123456.122331);

View data.

::

   SELECT * FROM decimal_type_t1;
      dt_col1
   -------------
    123456.1223
   (1 row)

Floating-Point Types
--------------------

The floating-point type is an inexact, variable-precision numeric type. This type is an implementation of IEEE Standard 754 for Binary Floating-Point Arithmetic (single and double precision, respectively), to the extent that the underlying processor, OS, and compiler support it.

.. table:: **Table 3** Floating point types

   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column            | Description                                                                                 | Storage Space                                                                                                                                  | Range                                                                                                                                                                               |
   +===================+=============================================================================================+================================================================================================================================================+=====================================================================================================================================================================================+
   | REAL,             | Single precision floating points, inexact                                                   | 4 bytes                                                                                                                                        | Six bytes of decimal digits                                                                                                                                                         |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   | FLOAT4            |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DOUBLE PRECISION, | Double precision floating points, inexact                                                   | 8 bytes                                                                                                                                        | 1E-307~1E+308,                                                                                                                                                                      |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   | FLOAT8            |                                                                                             |                                                                                                                                                | 15 bytes of decimal digits                                                                                                                                                          |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | FLOAT[(p)]        | Floating points, inexact. The value range of precision (p) is [1,53].                       | 4 or 8 bytes                                                                                                                                   | REAL or DOUBLE PRECISION is selected as an internal identifier based on different precision (p). If no precision is specified, DOUBLE PRECISION is used as the internal identifier. |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   |                   | .. note::                                                                                   |                                                                                                                                                |                                                                                                                                                                                     |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   |                   |    **p** is the precision, indicating the total decimal digits.                             |                                                                                                                                                |                                                                                                                                                                                     |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | BINARY_DOUBLE     | DOUBLE PRECISION alias, compatible with Oracle                                              | 8 bytes                                                                                                                                        | 1E-307~1E+308,                                                                                                                                                                      |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   |                   |                                                                                             |                                                                                                                                                | 15 bytes of decimal digits                                                                                                                                                          |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | DEC[(p[,s])]      | The value range of p (precision) is [1,1000], and the value range of s (standard) is [0,p]. | The precision is specified by users. Every four decimal digits occupy two bytes, and an extra eight-byte overhead is added to the entire data. | Up to 131,072 digits before the decimal point; and up to 16,383 digits after the decimal point when no precision is specified                                                       |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   |                   | .. note::                                                                                   |                                                                                                                                                |                                                                                                                                                                                     |
   |                   |                                                                                             |                                                                                                                                                |                                                                                                                                                                                     |
   |                   |    **p** indicates the total digits, and **s** indicates the decimal digit.                 |                                                                                                                                                |                                                                                                                                                                                     |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | INTEGER[(p[,s])]  | The value range of p (precision) is [1,1000], and the value range of s (standard) is [0,p]. | The precision is specified by users. Every four decimal digits occupy two bytes, and an extra eight-byte overhead is added to the entire data. | Up to 131,072 digits before the decimal point; and up to 16,383 digits after the decimal point when no precision is specified                                                       |
   +-------------------+---------------------------------------------------------------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Examples:

Create a table with floating-point values.

::

   CREATE TABLE float_type_t2
   (
       FT_COL1 INTEGER,
       FT_COL2 FLOAT4,
       FT_COL3 FLOAT8,
       FT_COL4 FLOAT(3),
       FT_COL5 BINARY_DOUBLE,
       FT_COL6 DECIMAL(10,4),
       FT_COL7 INTEGER(6,3)
   ) DISTRIBUTE BY HASH ( ft_col1);

Insert data.

::

   INSERT INTO float_type_t2 VALUES(10,10.365456,123456.1234,10.3214, 321.321, 123.123654, 123.123654);

View data.

::

   SELECT * FROM float_type_t2;
    ft_col1 | ft_col2 |   ft_col3   | ft_col4 | ft_col5 | ft_col6  | ft_col7
   ---------+---------+-------------+---------+---------+----------+---------
         10 | 10.3655 | 123456.1234 | 10.3214 | 321.321 | 123.1237 | 123.124
   (1 row)

Serial Integers
---------------

SMALLSERIAL, SERIAL, and BIGSERIAL are not true types, but merely a notational convenience for creating unique identifier columns. Therefore, an integer column is created and its default value plans to be read from a sequencer. A NOT NULL constraint is used to ensure NULL is not inserted. In most cases you would also want to attach a UNIQUE or PRIMARY KEY constraint to prevent duplicate values from being inserted unexpectedly. Lastly, the sequence is marked as "owned by" the column, so that it will be dropped if the column or table is dropped. Currently, the SERIAL column can be specified only when you create a table. You cannot add the SERIAL column in an existing table. In addition, SERIAL columns cannot be created in temporary tables. Because SERIAL is not a data type, columns cannot be converted to this type.

.. table:: **Table 4** Sequence integer

   +-------------+--------------------------------------+---------------+-------------------------------+
   | Column      | Description                          | Storage Space | Range                         |
   +=============+======================================+===============+===============================+
   | SMALLSERIAL | Two-byte auto-incrementing integer   | 2 bytes       | 1 ~ 32,767                    |
   +-------------+--------------------------------------+---------------+-------------------------------+
   | SERIAL      | Four-byte auto-incrementing integer  | 4 bytes       | 1 ~ 2,147,483,647             |
   +-------------+--------------------------------------+---------------+-------------------------------+
   | BIGSERIAL   | Eight-byte auto-incrementing integer | 8 bytes       | 1 ~ 9,223,372,036,854,775,807 |
   +-------------+--------------------------------------+---------------+-------------------------------+

Examples:

Create a table with serial values.

::

   CREATE TABLE smallserial_type_tab(a SMALLSERIAL);

Insert data.

::

   INSERT INTO smallserial_type_tab VALUES(default);

Insert data again.

::

   INSERT INTO smallserial_type_tab VALUES(default);

View data.

::

   SELECT * FROM smallserial_type_tab;
    a
   ---
    1
    2
   (2 rows)
