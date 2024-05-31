:original_name: dws_06_0330.html

.. _dws_06_0330:

Functional Functions
====================

hll_print(hll)
--------------

Description: Prints some debugging parameters of an HLL.

Return type: cstring

Example:

::

   SELECT hll_print(hll_empty());
                            hll_print
   -----------------------------------------------------------
    EMPTY, nregs=2048, nbits=5, expthresh=-1(160), sparseon=1gongne
   (1 row)

hll_empty()
-----------

Description: Creates an empty HLL.

Return type: hll

Example:

::

   SELECT hll_empty();
    hll_empty
   -----------
    \x118b7f
   (1 row)

hll_empty(int32 log2m)
----------------------

Description: Creates an empty HLL and sets the **log2m** parameter. The parameter value ranges from 10 to 16.

Return type: hll

Example:

::

   SELECT hll_empty(10);
    hll_empty
   -----------
    \x118a7f
   (1 row)

hll_empty(int32 log2m, int32 regwidth)
--------------------------------------

Description: Creates an empty HLL and sets the **log2m** and **regwidth** parameters in sequence. The value of **regwidth** ranges from 1 to 5.

Return type: hll

Example:

::

   SELECT hll_empty(10, 4);
    hll_empty
   -----------
    \x116a7f
   (1 row)

hll_empty(int32 log2m, int32 regwidth, int64 expthresh)
-------------------------------------------------------

Description: Creates an empty HLL and sets the **log2m**, **regwidth**, and **expthresh** parameters. The value of **expthresh** is an integer ranging from -1 to 7. This parameter specifies the threshold for switching from the **explicit** mode to the **sparse** mode. **-1** indicates the auto mode; **0** indicates that the **explicit** mode is skipped; a value from 1 to 7 indicates that the mode is switched when the number of distinct values reaches 2\ :sup:`expthresh`.

Return type: hll

Example:

::

   SELECT hll_empty(10, 4, 7);
    hll_empty
   -----------
    \x116a48
   (1 row)

hll_empty(int32 log2m, int32 regwidth, int64 expthresh, int32 sparseon)
-----------------------------------------------------------------------

Description: Creates an empty HLL and sets the **log2m**, **regwidth**, **expthresh**, and **sparseon** parameters. The value of **sparseon** is **0** or **1**.

Return type: hll

Example:

::

   SELECT hll_empty(10,4,7,0);
    hll_empty
   -----------
    \x116a08
   (1 row)

hll_add(hll, hll_hashval)
-------------------------

Description: Adds hll_hashval to an HLL.

Return type: hll

Example:

::

   SELECT hll_add(hll_empty(), hll_hash_integer(1));
            hll_add
   --------------------------
    \x128b7f8895a3f5af28cafe
   (1 row)

hll_add_rev(hll_hashval, hll)
-----------------------------

Description: Adds hll_hashval to an HLL. This function works the same as hll_add, except that the positions of parameters are switched.

Return type: hll

Example:

::

   SELECT hll_add_rev(hll_hash_integer(1), hll_empty());
          hll_add_rev
   --------------------------
    \x128b7f8895a3f5af28cafe
   (1 row)

hll_eq(hll, hll)
----------------

Description: Compares two HLLs to check whether they are the same.

Return type: bool

Example:

::

   SELECT hll_eq(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
    hll_eq
   --------
    f
   (1 row)

hll_ne(hll, hll)
----------------

Description: Compares two HLLs to check whether they are different.

Return type: bool

Example:

::

   SELECT hll_ne(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
    hll_ne
   --------
    t
   (1 row)

hll_cardinality(hll)
--------------------

Description: Calculates the number of distinct values of an HLL.

Return type: integer

Example:

::

   SELECT hll_cardinality(hll_empty() || hll_hash_integer(1));
    hll_cardinality
   -----------------
                  1
   (1 row)

hll_union(hll, hll)
-------------------

Description: Performs the **UNION** operation on two HLL data structures to obtain one HLL.

Return type: hll

Example:

::

   SELECT hll_union(hll_add(hll_empty(), hll_hash_integer(1)), hll_add(hll_empty(), hll_hash_integer(2)));
                   hll_union
   ------------------------------------------
    \x128b7f8895a3f5af28cafeda0ce907e4355b60
   (1 row)
