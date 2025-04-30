:original_name: dws_04_0956.html

.. _dws_04_0956:

GS_ROW_TABLE_IO_STAT
====================

**GS_ROW_TABLE_IO_STAT** displays the I/O of all row-store tables of the database on the current node. The value of each statistical column is the accumulated value since the instance was started.

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
