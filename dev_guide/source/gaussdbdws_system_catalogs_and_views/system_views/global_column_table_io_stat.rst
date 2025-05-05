:original_name: dws_04_0964.html

.. _dws_04_0964:

GLOBAL_COLUMN_TABLE_IO_STAT
===========================

**GLOBAL_COLUMN_TABLE_IO_STAT** provides I/O statistics of all column-store tables in the current database. The names, types, and sequences of the columns in the view are the same as those in the **GS_COLUMN_TABLE_IO_STAT** view. For details about the columns, see :ref:`Table 1 <en-us_topic_0000001811491329__table39829710387>`. The value of each statistical column is the sum of the values of the corresponding columns of all nodes.

.. _en-us_topic_0000001811491329__table39829710387:

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
