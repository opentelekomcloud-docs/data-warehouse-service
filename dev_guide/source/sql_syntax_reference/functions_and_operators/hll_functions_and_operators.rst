:original_name: dws_06_0042.html

.. _dws_06_0042:

HLL Functions and Operators
===========================

Hash Functions
--------------

-  hll_hash_boolean(bool)

   Description: Hashes data of the bool type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_boolean(FALSE);
        hll_hash_boolean
      ---------------------
       5048724184180415669
      (1 row)

-  hll_hash_boolean(bool, int32)

   Description: Configures a hash seed (that is, change the hash policy) and hashes data of the bool type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_boolean(FALSE, 10);
        hll_hash_boolean
      --------------------
       391264977436098630
      (1 row)

-  hll_hash_smallint(smallint)

   Description: Hashes data of the smallint type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_smallint(100::smallint);
        hll_hash_smallint
      ---------------------
       4631120266694327276
      (1 row)

.. note::

   If parameters with the same numeric value are hashed using different data types, the data will differ, because hash functions select different calculation policies for each type.

-  hll_hash_smallint(smallint, int32)

   Description: Configures a hash seed (that is, change the hash policy) and hashes data of the smallint type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_smallint(100::smallint, 10);
        hll_hash_smallint
      ---------------------
       8349353095166695771
      (1 row)

-  hll_hash_integer(integer)

   Description: Hashes data of the integer type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_integer(0);
         hll_hash_integer
      ----------------------
       -3485513579396041028
      (1 row)

-  hll_hash_integer(integer, int32)

   Description: Hashes data of the integer type and configures a hash seed (that is, change the hash policy).

   Return type: hll_hashval

   For example:

   ::

       SELECT hll_hash_integer(0, 10);
        hll_hash_integer
      --------------------
       183371090322255134
      (1 row)

-  hll_hash_bigint(bigint)

   Description: Hashes data of the bigint type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_bigint(100::bigint);
         hll_hash_bigint
      ---------------------
       8349353095166695771
      (1 row)

-  hll_hash_bigint(bigint, int32)

   Description: Hashes data of the bigint type and configures a hash seed (that is, change the hash policy).

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_bigint(100::bigint, 10);
         hll_hash_bigint
      ---------------------
       4631120266694327276
      (1 row)

-  hll_hash_bytea(bytea)

   Description: Hashes data of the bytea type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_bytea(E'\\x');
       hll_hash_bytea
      ----------------
       0
      (1 row)

-  hll_hash_bytea(bytea, int32)

   Description: Hashes data of the bytea type and configures a hash seed (that is, change the hash policy).

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_bytea(E'\\x', 10);
         hll_hash_bytea
      ---------------------
       6574525721897061910
      (1 row)

-  hll_hash_text(text)

   Description: Hashes data of the text type.

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_text('AB');
          hll_hash_text
      ---------------------
       5365230931951287672
      (1 row)

-  hll_hash_text(text, int32)

   Description: Hashes data of the text type and configures a hash seed (that is, change the hash policy).

   Return type: hll_hashval

   For example:

   ::

      SELECT hll_hash_text('AB', 10);
      hll_hash_text
      ---------------------
      7680762839921155903
      (1 row)

-  hll_hash_any(anytype)

   Description: Hashes data of any type.

   Return type: hll_hashval

   For example:

   ::

      select hll_hash_any(1);
           hll_hash_any
      ----------------------
       -8604791237420463362
      (1 row)

      select hll_hash_any('08:00:2b:01:02:03'::macaddr);
           hll_hash_any
      ----------------------
       -4883882473551067169
      (1 row)

-  hll_hash_any(anytype, int32)

   Description: Hashes data of any type and configures a hash seed (that is, change the hash policy).

   Return type: hll_hashval

   For example:

   ::

      select hll_hash_any(1, 10);
           hll_hash_any
      ----------------------
       -1478847531811254870
      (1 row)

-  hll_hashval_eq(hll_hashval, hll_hashval)

   Description: Compares two pieces of data of the hll_hashval type to check whether they are the same.

   Return type: bool

   For example:

   ::

      select hll_hashval_eq(hll_hash_integer(1), hll_hash_integer(1));
       hll_hashval_eq
      ----------------
       t
      (1 row)

-  hll_hashval_ne(hll_hashval, hll_hashval)

   Description: Compares two pieces of data of the hll_hashval type to check whether they are different.

   Return type: bool

   For example:

   ::

      select hll_hashval_ne(hll_hash_integer(1), hll_hash_integer(1));
       hll_hashval_ne
      ----------------
       f
      (1 row)

Precision Functions
-------------------

HLL supports **explicit**, **sparse**, and **full** modes. **explicit** and **sparse** excel when the data scale is small, and barely produce errors in calculation results. When the number of distinct values increases, **full** becomes more suitable, but produces some errors. The following functions are used to view precision parameters in HLLs.

-  hll_schema_version(hll)

   Description: Checks the schema version in the current HLL.

   For example:

   ::

      select hll_schema_version(hll_empty());
       hll_schema_version
      --------------------
                 1
      (1 row)

-  hll_type(hll)

   Description: Checks the type of the current HLL.

   For example:

   ::

      select hll_type(hll_empty());
       hll_type
      ----------
              1
      (1 row)

-  hll_log2m(hll)

   Description: Check the value of log2m of the current HLL. This value affects the error rate in calculating the number of distinct values by the HLL. The formula for calculating the error rate is as follows:

   |image1|

   For example:

   ::

      select hll_log2m(hll_empty());
       hll_log2m
      -----------
              11
      (1 row)

-  hll_regwidth(hll)

   Description: Checks the number of bits of buckets in a hll data structure.

   For example:

   ::

      select hll_regwidth(hll_empty());
       hll_regwidth
      --------------
              5
      (1 row)

-  hll_expthresh(hll)

   Description: Obtains the size of **expthresh** in the current HLL. An HLL usually switches from the **explicit** mode to the **sparse** mode and then to the **full** mode. This process is called the promotion hierarchy policy. You can change the value of **expthresh** to change the policy. For example, if **expthresh** is **0**, an HILL will skip the **explicit** mode and directly enter the **sparse** mode. If the value of **expthresh** is explicitly set to a value ranging from 1 to 7, this function returns 2\ :sup:`expthresh`.

   For example:

   ::

      select hll_expthresh(hll_empty());
       hll_expthresh
      ---------------
       (-1,160)
      (1 row)

      select hll_expthresh(hll_empty(11,5,3));
       hll_expthresh
      ---------------
       (8,8)
      (1 row)

-  hll_sparseon(hll)

   Description: Specifies whether to enable the **sparse** mode. **0** indicates **off** and **1** indicates **on**.

   For example:

   ::

      select hll_sparseon(hll_empty());
       hll_sparseon
      --------------
              1
      (1 row)

Aggregation Functions
---------------------

-  hll_add_agg(hll_hashval)

   Description: Groups hashed data into HLL.

   Return type: hll

   For example:

   ::

      -- Prepare data:
      create table t_id(id int);
      insert into t_id values(generate_series(1,500));
      create table t_data(a int, c text);
      insert into t_data select mod(id,2), id from t_id;

      -- Create another table and specify an HLL column:
      create table t_a_c_hll(a int, c hll);

      -- Use GROUP BY on column a to group data, and insert the data to the HLL:
      insert into t_a_c_hll select a, hll_add_agg(hll_hash_text(c)) from t_data group by a;

      -- Calculate the number of distinct values for each group in the HLL:
      select a, #c as cardinality from t_a_c_hll order by a;
       a |   cardinality
      ---+------------------
       0 | 250.741759091658
       1 | 250.741759091658
      (2 rows)

-  hll_add_agg(hll_hashval, int32 log2m)

   Description: Groups hashed data into HLL and sets the **log2m** parameter. The parameter value ranges from 10 to 16.

   Return type: hll

   For example:

   ::

       Select hll_cardinality(hll_add_agg(hll_hash_text(c), 10)) from t_data;
       hll_cardinality
      ------------------
       503.932348927339
      (1 row)

-  hll_add_agg(hll_hashval, int32 log2m, int32 regwidth)

   Description: Groups hashed data into HLL and sets the **log2m** and **regwidth** parameters in sequence. The value of **regwidth** ranges from 1 to 5.

   Return type: hll

   For example:

   ::

      Select hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1)) from t_data;
       hll_cardinality
      ------------------
       496.628982624022
      (1 row)

-  hll_add_agg(hll_hashval, int32 log2m, int32 regwidth, int64 expthresh)

   Description: Groups hashed data into HLL and sets the parameters **log2m**, **regwidth**, and **expthresh** in sequence. The value of **expthresh** is an integer ranging from -1 to 7. **expthresh** is used to specify the threshold for switching from the **explicit** mode to the **sparse** mode. **-1** indicates the auto mode; **0** indicates that the **explicit** mode is skipped; a value from 1 to 7 indicates that the mode is switched when the number of distinct values reaches 2\ :sup:`expthresh`.

   Return type: hll

   For example:

   ::

       Select hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1, 4)) from t_data;
       hll_cardinality
      ------------------
       496.628982624022
      (1 row)

-  hll_add_agg(hll_hashval, int32 log2m, int32 regwidth, int64 expthresh, int32 sparseon)

   Description: Groups hashed data into HLL and sets the **log2m**, **regwidth**, **expthresh**, and **sparseon** parameters in sequence. The value of **sparseon** is **0** or **1**.

   Return type: hll

   For example:

   ::

       Select hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1, 4, 0)) from t_data;
       hll_cardinality
      ------------------
       496.628982624022
      (1 row)

-  hll_union_agg(hll)

   Description: Perform the **UNION** operation on multiple pieces of data of the hll type to obtain one HLL.

   Return type: hll

   For example:

   ::

      -- Perform the UNION operation on data of the hll type in each group to obtain one HLL, and calculate the number of distinct values:
      select #hll_union_agg(c) as cardinality from t_a_c_hll;
         cardinality
      ------------------
       496.628982624022
      (1 row)

   .. note::

      To perform **UNION** on data in multiple HLLs, ensure that the HLLs have the same precision. Otherwise, **UNION** cannot be performed. This restriction also applies to the hll_union(hll, hll) function.

Functional Functions
--------------------

-  hll_print(hll)

   Description: Prints some debugging parameters of an HLL.

   For example:

   ::

      select hll_print(hll_empty());
                               hll_print
      -----------------------------------------------------------
       EMPTY, nregs=2048, nbits=5, expthresh=-1(160), sparseon=1gongne
      (1 row)

-  hll_empty()

   Description: Creates an empty HLL.

   Return type: hll

   For example:

   ::

      select hll_empty();
       hll_empty
      -----------
       \x118b7f
      (1 row)

-  hll_empty(int32 log2m)

   Description: Creates an empty HLL and sets the **log2m** parameter. The parameter value ranges from 10 to 16.

   Return type: hll

   For example:

   ::

       select hll_empty(10);
       hll_empty
      -----------
       \x118a7f
      (1 row)

-  hll_empty(int32 log2m, int32 regwidth)

   Description: Creates an empty HLL and sets the **log2m** and **regwidth** parameters in sequence. The value of **regwidth** ranges from 1 to 5.

   Return type: hll

   For example:

   ::

      select hll_empty(10, 4);
       hll_empty
      -----------
       \x116a7f
      (1 row)

-  hll_empty(int32 log2m, int32 regwidth, int64 expthresh)

   Description: Creates an empty HLL and sets the **log2m**, **regwidth**, and **expthresh** parameters. The value of **expthresh** is an integer ranging from -1 to 7. This parameter specifies the threshold for switching from the **explicit** mode to the **sparse** mode. **-1** indicates the auto mode; **0** indicates that the **explicit** mode is skipped; a value from 1 to 7 indicates that the mode is switched when the number of distinct values reaches 2\ :sup:`expthresh`.

   Return type: hll

   For example:

   ::

       select hll_empty(10, 4, 7);
       hll_empty
      -----------
       \x116a48
      (1 row)

-  hll_empty(int32 log2m, int32 regwidth, int64 expthresh, int32 sparseon)

   Description: Creates an empty HLL and sets the **log2m**, **regwidth**, **expthresh**, and **sparseon** parameters. The value of **sparseon** is **0** or **1**.

   Return type: hll

   For example:

   ::

       select hll_empty(10,4,7,0);
       hll_empty
      -----------
       \x116a08
      (1 row)

-  hll_add(hll, hll_hashval)

   Description: Adds hll_hashval to an HLL.

   Return type: hll

   For example:

   ::

      select hll_add(hll_empty(), hll_hash_integer(1));
               hll_add
      --------------------------
       \x128b7f8895a3f5af28cafe
      (1 row)

-  hll_add_rev(hll_hashval, hll)

   Description: Adds hll_hashval to an HLL. This function works the same as hll_add, except that the positions of parameters are switched.

   Return type: hll

   For example:

   ::

      select hll_add_rev(hll_hash_integer(1), hll_empty());
             hll_add_rev
      --------------------------
       \x128b7f8895a3f5af28cafe
      (1 row)

-  hll_eq(hll, hll)

   Description: Compares two HLLs to check whether they are the same.

   Return type: bool

   For example:

   ::

      select hll_eq(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
       hll_eq
      --------
       f
      (1 row)

-  hll_ne(hll, hll)

   Description: Compares two HLLs to check whether they are different.

   Return type: bool

   For example:

   ::

      select hll_ne(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
       hll_ne
      --------
       t
      (1 row)

-  hll_cardinality(hll)

   Description: Calculates the number of distinct values of an HLL.

   Return type: int

   For example:

   ::

      select hll_cardinality(hll_empty() || hll_hash_integer(1));
       hll_cardinality
      -----------------
                     1
      (1 row)

-  hll_union(hll, hll)

   Description: Performs the **UNION** operation on two HLL data structures to obtain one HLL.

   Return type: hll

   For example:

   ::

      select hll_union(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
                      hll_union
      ------------------------------------------
       \x128b7f8895a3f5af28cafeda0ce907e4355b60
      (1 row)

Built-in Functions
------------------

HLL has a series of built-in functions for internal data processing. Generally, users do not need to know how to use these functions. For details, see :ref:`Table 1 <en-us_topic_0000001145830495__table47911935194712>`.

.. _en-us_topic_0000001145830495__table47911935194712:

.. table:: **Table 1** Built-in functions

   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | Function          | Description                                                                                                                                   |
   +===================+===============================================================================================================================================+
   | hll_in            | Receives hll data in string format.                                                                                                           |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_out           | Sends hll data in string format.                                                                                                              |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_recv          | Receives hll data in bytea format.                                                                                                            |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_send          | Sends hll data in bytea format.                                                                                                               |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_trans_in      | Receives hll_trans_type data in string format.                                                                                                |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_trans_out     | Sends hll_trans_type data in string format.                                                                                                   |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_trans_recv    | Receives hll_trans_type data in bytea format.                                                                                                 |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_trans_send    | Sends hll_trans_type data in bytea format.                                                                                                    |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_typmod_in     | Receives typmod data.                                                                                                                         |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_typmod_out    | Sends typmod data.                                                                                                                            |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_hashval_in    | Receives hll_hashval data.                                                                                                                    |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_hashval_out   | Sends hll_hashval data.                                                                                                                       |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_add_trans0    | Works similar to hll_add, and is used on the first phase of DNs in distributed aggregation operations.                                        |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_union_trans   | Works similar to hll_union, and is used on the first phase of DNs in distributed aggregation operations.                                      |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_union_collect | Works similar to hll_union, and is used on the second phase of CNs in distributed aggregation operations to summarize the results of each DN. |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_pack          | Is used on the third phase of CNs in distributed aggregation operations to convert a user-defined type hll_trans_type to the hll type.        |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll               | Converts a hll type to another hll type. Input parameters can be specified.                                                                   |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_hashval       | Converts the bigint type to the hll_hashval type.                                                                                             |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+
   | hll_hashval_int4  | Converts the int4 type to the hll_hashval type.                                                                                               |
   +-------------------+-----------------------------------------------------------------------------------------------------------------------------------------------+

Operators
---------

-  =

   Description: Compares the values of hll and hll_hashval types to check whether they are the same.

   Return type: bool

   For example:

   ::

      --hll
      select (hll_empty() || hll_hash_integer(1)) = (hll_empty() || hll_hash_integer(1));
      column
      ----------
       t
      (1 row)

      --hll_hashval
      select hll_hash_integer(1) = hll_hash_integer(1);
       ?column?
      ----------
       t
      (1 row)

-  <> or !=

   Description: Compares the values of hll and hll_hashval types to check whether they are different.

   Return type: bool

   For example:

   ::

      --hll
      select (hll_empty() || hll_hash_integer(1)) <> (hll_empty() || hll_hash_integer(2));
       ?column?
      ----------
       t
      (1 row)

      --hll_hashval
      select hll_hash_integer(1) <> hll_hash_integer(2);
       ?column?
      ----------
       t
      (1 row)

-  \|\|

   Description: Represents the functions of hll_add, hll_union, and hll_add_rev.

   Return type: hll

   For example:

   ::

      --hll_add
      select hll_empty() || hll_hash_integer(1);
               ?column?
      --------------------------
       \x128b7f8895a3f5af28cafe
      (1 row)

      --hll_add_rev
      select hll_hash_integer(1) || hll_empty();
               ?column?
      --------------------------
       \x128b7f8895a3f5af28cafe
      (1 row)

      --hll_union
      select (hll_empty() || hll_hash_integer(1)) || (hll_empty() || hll_hash_integer(2));
                       ?column?
      ------------------------------------------
       \x128b7f8895a3f5af28cafeda0ce907e4355b60
      (1 row)

-  #

   Description: Calculates the number of distinct values of an HLL. It works the same as the hll_cardinality function.

   Return type: int

   For example:

   ::

      select #(hll_empty() || hll_hash_integer(1));
       ?column?
      ----------
              1
      (1 row)

.. |image1| image:: /_static/images/en-us_image_0000001145830887.png
