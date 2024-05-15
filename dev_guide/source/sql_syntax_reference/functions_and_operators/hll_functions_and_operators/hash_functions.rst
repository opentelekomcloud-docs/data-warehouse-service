:original_name: dws_06_0327.html

.. _dws_06_0327:

Hash Functions
==============

hll_hash_boolean(bool)
----------------------

Description: Hashes data of the bool type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_boolean(FALSE);
     hll_hash_boolean
   ---------------------
    5048724184180415669
   (1 row)

hll_hash_boolean(bool, int32)
-----------------------------

Description: Configures a hash seed (that is, change the hash policy) and hashes data of the bool type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_boolean(FALSE, 10);
     hll_hash_boolean
   --------------------
    391264977436098630
   (1 row)

hll_hash_smallint(smallint)
---------------------------

Description: Hashes data of the smallint type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_smallint(100::smallint);
     hll_hash_smallint
   ---------------------
    4631120266694327276
   (1 row)

.. note::

   If parameters with the same numeric value are hashed using different data types, the data will differ, because hash functions select different calculation policies for each type.

hll_hash_smallint(smallint, int32)
----------------------------------

Description: Configures a hash seed (that is, change the hash policy) and hashes data of the smallint type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_smallint(100::smallint, 10);
     hll_hash_smallint
   ---------------------
    8349353095166695771
   (1 row)

hll_hash_integer(integer)
-------------------------

Description: Hashes data of the integer type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_integer(0);
      hll_hash_integer
   ----------------------
    -3485513579396041028
   (1 row)

hll_hash_integer(integer, int32)
--------------------------------

Description: Hashes data of the integer type and configures a hash seed (that is, change the hash policy).

Return type: hll_hashval

Example:

::

    SELECT hll_hash_integer(0, 10);
     hll_hash_integer
   --------------------
    183371090322255134
   (1 row)

hll_hash_bigint(bigint)
-----------------------

Description: Hashes data of the bigint type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_bigint(100::bigint);
      hll_hash_bigint
   ---------------------
    8349353095166695771
   (1 row)

hll_hash_bigint(bigint, int32)
------------------------------

Description: Hashes data of the bigint type and configures a hash seed (that is, change the hash policy).

Return type: hll_hashval

Example:

::

   SELECT hll_hash_bigint(100::bigint, 10);
      hll_hash_bigint
   ---------------------
    4631120266694327276
   (1 row)

hll_hash_bytea(bytea)
---------------------

Description: Hashes data of the bytea type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_bytea(E'\\x');
    hll_hash_bytea
   ----------------
    0
   (1 row)

hll_hash_bytea(bytea, int32)
----------------------------

Description: Hashes data of the bytea type and configures a hash seed (that is, change the hash policy).

Return type: hll_hashval

Example:

::

   SELECT hll_hash_bytea(E'\\x', 10);
      hll_hash_bytea
   ---------------------
    6574525721897061910
   (1 row)

hll_hash_text(text)
-------------------

Description: Hashes data of the text type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_text('AB');
       hll_hash_text
   ---------------------
    5365230931951287672
   (1 row)

hll_hash_text(text, int32)
--------------------------

Description: Hashes data of the text type and configures a hash seed (that is, change the hash policy).

Return type: hll_hashval

Example:

::

   SELECT hll_hash_text('AB', 10);
       hll_hash_text
   ---------------------
    7680762839921155903
   (1 row)

hll_hash_any(anytype)
---------------------

Description: Hashes data of any type.

Return type: hll_hashval

Example:

::

   SELECT hll_hash_any(1);
        hll_hash_any
   ----------------------
    -8604791237420463362
   (1 row)

   SELECT hll_hash_any('08:00:2b:01:02:03'::macaddr);
        hll_hash_any
   ----------------------
    -4883882473551067169
   (1 row)

hll_hash_any(anytype, int32)
----------------------------

Description: Hashes data of any type and configures a hash seed (that is, change the hash policy).

Return type: hll_hashval

Example:

::

   SELECT hll_hash_any(1, 10);
        hll_hash_any
   ----------------------
    -1478847531811254870
   (1 row)

hll_hashval_eq(hll_hashval, hll_hashval)
----------------------------------------

Description: Compares two pieces of data of the hll_hashval type to check whether they are the same.

Return type: bool

Example:

::

   SELECT hll_hashval_eq(hll_hash_integer(1), hll_hash_integer(1));
    hll_hashval_eq
   ----------------
    t
   (1 row)

hll_hashval_ne(hll_hashval, hll_hashval)
----------------------------------------

Description: Compares two pieces of data of the hll_hashval type to check whether they are different.

Return type: bool

Example:

::

   SELECT hll_hashval_ne(hll_hash_integer(1), hll_hash_integer(1));
    hll_hashval_ne
   ----------------
    f
   (1 row)
