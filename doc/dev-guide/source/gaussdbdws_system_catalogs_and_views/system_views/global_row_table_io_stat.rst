:original_name: dws_04_0965.html

.. _dws_04_0965:

GLOBAL_ROW_TABLE_IO_STAT
========================

**GLOBAL_ROW_TABLE_IO_STAT** provides I/O statistics of all row-store tables in the current database. The names, types, and sequences of the columns in the view are the same as those in the **GS_ROW_TABLE_IO_STAT** view. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001811490537__table5381771391>`. The value of each statistical column is the sum of the values of the corresponding columns of all nodes.

.. _en-us_topic_0000001811490537__table5381771391:

.. table:: **Table 1** GS_ROW_TABLE_IO_STAT columns

   +------------+--------+---------------------------------------------------------+
   | Column     | Type   | Description                                             |
   +============+========+=========================================================+
   | schemaname | Name   | Namespace of a table                                    |
   +------------+--------+---------------------------------------------------------+
   | relname    | Name   | Name of a table                                         |
   +------------+--------+---------------------------------------------------------+
   | heap_read  | Bigint | Number of blocks logically read in the heap             |
   +------------+--------+---------------------------------------------------------+
   | heap_hit   | Bigint | Number of block hits in the heap                        |
   +------------+--------+---------------------------------------------------------+
   | idx_read   | Bigint | Number of blocks logically read in the index            |
   +------------+--------+---------------------------------------------------------+
   | idx_hit    | Bigint | Number of block hits in the index                       |
   +------------+--------+---------------------------------------------------------+
   | toast_read | Bigint | Number of blocks logically read in the **TOAST** table  |
   +------------+--------+---------------------------------------------------------+
   | toast_hit  | Bigint | Number of block hits in the **TOAST** table             |
   +------------+--------+---------------------------------------------------------+
   | tidx_read  | Bigint | Number of indexes logically read in the **TOAST** table |
   +------------+--------+---------------------------------------------------------+
   | tidx_hit   | Bigint | Number of index hits in the **TOAST** table             |
   +------------+--------+---------------------------------------------------------+
