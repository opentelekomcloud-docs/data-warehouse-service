:original_name: dws_06_0017.html

.. _dws_06_0017:

Bit String Types
================

Bit strings are strings of 1's and 0's. They can be used to store bit masks.

GaussDB(DWS) supports two SQL bit types: **bit(n)** and **bit varying(n)**, where **n** is a positive integer.

The **bit** type data must match the length **n** exactly. It is an error to attempt to store shorter or longer bit strings. The **bit varying** data is of variable length up to the maximum length **n**; longer strings will be rejected. Writing **bit** without a length is equivalent to **bit(1)**, while **bit varying** without a length specification means unlimited length.

.. note::

   If one explicitly casts a bit-string value to **bit(n)**, it will be truncated or zero-padded on the right to be exactly **n** bits, without raising an error.

   Similarly, if one explicitly casts a bit-string value to **bit varying(n)**, it will be truncated on the right if it is more than **n** bits.

::

   -- Create a table:
   CREATE TABLE bit_type_t1
   (
       BT_COL1 INTEGER,
       BT_COL2 BIT(3),
       BT_COL3 BIT VARYING(5)
   ) DISTRIBUTE BY REPLICATION;

   --Insert data:
   INSERT INTO bit_type_t1 VALUES(1, B'101', B'00');

   -- Specify the type length. An error is reported if an inserted string exceeds this length.
   INSERT INTO bit_type_t1 VALUES(2, B'10', B'101');
   ERROR:  bit string length 2 does not match type bit(3)
   CONTEXT:  referenced column: bt_col2

   -- Specify the type length. Data is converted if it exceeds this length.
   INSERT INTO bit_type_t1 VALUES(2, B'10'::bit(3), B'101');

   -- View data:
   SELECT * FROM bit_type_t1;
    bt_col1 | bt_col2 | bt_col3
   ---------+---------+---------
          1 | 101     | 00
          2 | 100     | 101
   (2 rows)

   -- Delete the tables:
   DROP TABLE bit_type_t1;
