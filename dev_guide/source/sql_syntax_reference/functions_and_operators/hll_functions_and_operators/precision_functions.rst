:original_name: dws_06_0328.html

.. _dws_06_0328:

Precision Functions
===================

HLL supports **explicit**, **sparse**, and **full** modes. **explicit** and **sparse** excel when the data scale is small, and barely produce errors in calculation results. When the number of distinct values increases, **full** becomes more suitable, but produces some errors. The following functions are used to view precision parameters in HLLs.

hll_schema_version(hll)
-----------------------

Description: Checks the schema version in the current HLL.

Return type: integer

Example:

::

   SELECT hll_schema_version(hll_empty());
    hll_schema_version
   --------------------
              1
   (1 row)

hll_type(hll)
-------------

Description: Checks the type of the current HLL.

Return type: integer

Example:

::

   SELECT hll_type(hll_empty());
    hll_type
   ----------
           1
   (1 row)

hll_log2m(hll)
--------------

Description: Check the value of log2m of the current HLL. This value affects the error rate in calculating the number of distinct values by the HLL. The formula for calculating the error rate is as follows:

|image1|

Return type: integer

Example:

::

   SELECT hll_log2m(hll_empty());
    hll_log2m
   -----------
           11
   (1 row)

hll_regwidth(hll)
-----------------

Description: Checks the number of bits of buckets in a hll data structure.

Return type: integer

Example:

::

   SELECT hll_regwidth(hll_empty());
    hll_regwidth
   --------------
               5
   (1 row)

hll_expthresh(hll)
------------------

Description: Obtains the size of **expthresh** in the current HLL. An HLL usually switches from the **explicit** mode to the **sparse** mode and then to the **full** mode. This process is called the promotion hierarchy policy. You can change the value of **expthresh** to change the policy. For example, if **expthresh** is **0**, an HILL will skip the **explicit** mode and directly enter the **sparse** mode. If the value of **expthresh** is explicitly set to a value ranging from 1 to 7, this function returns 2\ :sup:`expthresh`.

Return type: record

Example:

::

   SELECT hll_expthresh(hll_empty());
    hll_expthresh
   ---------------
    (-1,160)
   (1 row)

   SELECT hll_expthresh(hll_empty(11,5,3));
    hll_expthresh
   ---------------
    (8,8)
   (1 row)

hll_sparseon(hll)
-----------------

Description: Specifies whether to enable the **sparse** mode. **0** indicates **off** and **1** indicates **on**.

Return type: integer

Example:

::

   SELECT hll_sparseon(hll_empty());
    hll_sparseon
   --------------
               1
   (1 row)

.. |image1| image:: /_static/images/en-us_image_0000001495204945.png
