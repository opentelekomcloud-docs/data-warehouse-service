:original_name: dws_07_6815.html

.. _dws_07_6815:

Bitmap Index
============

There is a configuration parameter that is introduced for this feature named **BitmapIndexSupport** which default value is **comment**, then the sample input and output are as follows:

**Input - Bitmap index**

::

   CREATE BITMAP INDEX
   emp_bitmap_idx
   ON index_demo (gender);

**Output**

::

   /*CREATE BITMAP INDEX emp_bitmap_idx ON index_demo (gender);*/

However, if the configuration parameter is set to **BTREE**, then the output is as follows:

**Output**

::

   CREATE
        /*bitmap*/
        INDEX emp_bitmap_idx
             ON index_demo
             USING btree (gender) ;
