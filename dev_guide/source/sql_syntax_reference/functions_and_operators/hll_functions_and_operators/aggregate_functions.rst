:original_name: dws_06_0329.html

.. _dws_06_0329:

Aggregate Functions
===================

hll_add_agg(hll_hashval)
------------------------

Description: Groups hashed data into HLL.

Return type: hll

Example:

#. Prepare data.

   ::

      CREATE TABLE t_id(id int);
      INSERT INTO t_id VALUES(generate_series(1,500));
      CREATE TABLE t_data(a int, c text);
      INSERT INTO t_data SELECT mod(id,2), id FROM t_id;

#. Create another table and specify an HLL column:

   ::

      CREATE TABLE t_a_c_hll(a int, c hll);

#. Use **GROUP BY** on column **a** to group data, and insert the data to the HLL column:

   ::

      INSERT INTO t_a_c_hll SELECT a, hll_add_agg(hll_hash_text(c)) FROM t_data GROUP BY a;

#. Calculate the number of distinct values for each group in the HLL column:

   ::

      SELECT a, #c as cardinality FROM t_a_c_hll order by a;
       a |   cardinality
      ---+------------------
       0 | 250.741759091658
       1 | 250.741759091658
      (2 rows)

hll_add_agg(hll_hashval, int32 log2m)
-------------------------------------

Description: Groups hashed data into HLL and sets the **log2m** parameter. The parameter value ranges from 10 to 16.

Return type: hll

Example:

::

    SELECT hll_cardinality(hll_add_agg(hll_hash_text(c), 10)) FROM t_data;
    hll_cardinality
   ------------------
    503.932348927339
   (1 row)

hll_add_agg(hll_hashval, int32 log2m, int32 regwidth)
-----------------------------------------------------

Description: Groups hashed data into HLL. and sets the **log2m** and **regwidth** parameters in sequence. The value of **regwidth** ranges from 1 to 5.

Return type: hll

Example:

::

   SELECT hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1)) FROM t_data;
    hll_cardinality
   ------------------
    496.628982624022
   (1 row)

hll_add_agg(hll_hashval, int32 log2m, int32 regwidth, int64 expthresh)
----------------------------------------------------------------------

Description: Groups hashed data into HLL and sets the parameters **log2m**, **regwidth**, and **expthresh** in sequence. The value of **expthresh** is an integer ranging from -1 to 7. **expthresh** is used to specify the threshold for switching from the **explicit** mode to the **sparse** mode. **-1** indicates the auto mode; **0** indicates that the **explicit** mode is skipped; a value from 1 to 7 indicates that the mode is switched when the number of distinct values reaches 2\ :sup:`expthresh`.

Return type: hll

Example:

::

    SELECT hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1, 4)) FROM t_data;
    hll_cardinality
   ------------------
    496.628982624022
   (1 row)

hll_add_agg(hll_hashval, int32 log2m, int32 regwidth, int64 expthresh, int32 sparseon)
--------------------------------------------------------------------------------------

Description: Groups hashed data into HLL and sets the parameters **log2m**, **regwidth**, **expthresh**, and **sparseon** in sequence. The value of **sparseon** is 0 or 1.

Return type: hll

Example:

::

    SELECT hll_cardinality(hll_add_agg(hll_hash_text(c), NULL, 1, 4, 0)) FROM t_data;
    hll_cardinality
   ------------------
    496.628982624022
   (1 row)

hll_union_agg(hll)
------------------

Description: Perform the **UNION** operation on multiple pieces of data of the hll type to obtain one HLL.

Return type: hll

Example:

Perform the **UNION** operation on data of the HLL type in each group to obtain one HLL, and calculate the number of distinct values:

::

   SELECT #hll_union_agg(c) as cardinality FROM t_a_c_hll;
      cardinality
   ------------------
    496.628982624022
   (1 row)

.. note::

   To perform **UNION** on data in multiple HLLs, ensure that the HLLs have the same precision. Otherwise, **UNION** cannot be performed. This restriction also applies to the hll_union(hll, hll) function.
