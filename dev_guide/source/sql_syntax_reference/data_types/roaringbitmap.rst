:original_name: dws_06_0279.html

.. _dws_06_0279:

RoaringBitmap
=============

In GaussDB(DWS) 8.1.3 and later, you can use the RoaringBitmap data type to store bitmap datasets.

The RoaringBitmap data type supports row-store and column-store tables.

.. table:: **Table 1** RoaringBitmap

   +---------------+--------------+--------------------------+------------------------------+
   | Name          | Storage Size | Description              | Range                        |
   +===============+==============+==========================+==============================+
   | RoaringBitmap | 32 bytes     | Stores bitmap data sets. | -2,147,483,648~2,147,483,647 |
   +---------------+--------------+--------------------------+------------------------------+

Example: Create a table with the RoaringBitmap data type.

::

   CREATE TABLE r_row (a int ,b text, c roaringbitmap);
   CREATE TABLE r_col (a int ,b text, c roaringbitmap) with (orientation=column);
