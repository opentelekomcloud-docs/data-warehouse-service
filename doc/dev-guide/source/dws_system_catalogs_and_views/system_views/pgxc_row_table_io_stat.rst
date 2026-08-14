:original_name: dws_04_0961.html

.. _dws_04_0961:

PGXC_ROW_TABLE_IO_STAT
======================

**PGXC_ROW_TABLE_IO_STAT** provides I/O statistics of all row-store tables of the database on all CNs and DNs in the cluster. Except the **nodename** column of the name type added in front of each row, the names, types, and sequences of other columns are the same as those in the **GS_ROW_TABLE_IO_STAT** view. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001764491824__table5381771391>`.

.. _en-us_topic_0000001764491824__table5381771391:

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
