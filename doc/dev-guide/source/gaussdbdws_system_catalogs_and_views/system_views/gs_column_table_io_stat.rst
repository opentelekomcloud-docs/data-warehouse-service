:original_name: dws_04_0955.html

.. _dws_04_0955:

GS_COLUMN_TABLE_IO_STAT
=======================

**GS_COLUMN_TABLE_IO_STAT** displays the I/O of all column-store tables of the database on the current node. The value of each statistical column is the accumulated value since the instance was started.

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
