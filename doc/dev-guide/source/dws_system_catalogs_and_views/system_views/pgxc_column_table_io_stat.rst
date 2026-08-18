:original_name: dws_04_0960.html

.. _dws_04_0960:

PGXC_COLUMN_TABLE_IO_STAT
=========================

**PGXC_COLUMN_TABLE_IO_STAT** provides I/O statistics of all column-store tables of the database on all CNs and DNs in the cluster. Except the **nodename** column of the name type added in front of each row, the names, types, and sequences of other columns are the same as those in the **GS_COLUMN_TABLE_IO_STAT** view. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001764650320__table39829710387>`.

.. _en-us_topic_0000001764650320__table39829710387:

.. table:: **Table 1** GS_COLUMN_TABLE_IO_STAT columns

   +------------+--------+----------------------------------------------------------+
   | Column     | Type   | Description                                              |
   +============+========+==========================================================+
   | schemaname | Name   | Namespace of a table                                     |
   +------------+--------+----------------------------------------------------------+
   | relname    | Name   | Table name                                               |
   +------------+--------+----------------------------------------------------------+
   | heap_read  | Bigint | Number of blocks logically read in the heap              |
   +------------+--------+----------------------------------------------------------+
   | heap_hit   | Bigint | Number of block hits in the heap                         |
   +------------+--------+----------------------------------------------------------+
   | idx_read   | Bigint | Number of blocks logically read in the index             |
   +------------+--------+----------------------------------------------------------+
   | idx_hit    | Bigint | Number of block hits in the index                        |
   +------------+--------+----------------------------------------------------------+
   | cu_read    | Bigint | Number of logical reads in the Compression Unit          |
   +------------+--------+----------------------------------------------------------+
   | cu_hit     | Bigint | Number of hits in the Compression Unit                   |
   +------------+--------+----------------------------------------------------------+
   | cidx_read  | Bigint | Number of indexes logically read in the Compression Unit |
   +------------+--------+----------------------------------------------------------+
   | cidx_hit   | Bigint | Number of index hits in the Compression Unit             |
   +------------+--------+----------------------------------------------------------+
