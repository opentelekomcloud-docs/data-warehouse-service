:original_name: dws_04_0955.html

.. _dws_04_0955:

GS_COLUMN_TABLE_IO_STAT
=======================

**GS_COLUMN_TABLE_IO_STAT** displays the I/O of all column-store tables of the database on the current node. The value of each statistical column is the accumulated value since the instance was started.

.. table:: **Table 1** GS_COLUMN_TABLE_IO_STAT columns

   +------------+--------+----------------------------------------------------------+
   | Name       | Type   | Description                                              |
   +============+========+==========================================================+
   | schemaname | name   | Namespace of a table                                     |
   +------------+--------+----------------------------------------------------------+
   | relname    | name   | Table name                                               |
   +------------+--------+----------------------------------------------------------+
   | heap_read  | bigint | Number of blocks logically read in the heap              |
   +------------+--------+----------------------------------------------------------+
   | heap_hit   | bigint | Number of block hits in the heap                         |
   +------------+--------+----------------------------------------------------------+
   | idx_read   | bigint | Number of blocks logically read in the index             |
   +------------+--------+----------------------------------------------------------+
   | idx_hit    | bigint | Number of block hits in the index                        |
   +------------+--------+----------------------------------------------------------+
   | cu_read    | bigint | Number of logical reads in the Compression Unit          |
   +------------+--------+----------------------------------------------------------+
   | cu_hit     | bigint | Number of hits in the Compression Unit                   |
   +------------+--------+----------------------------------------------------------+
   | cidx_read  | bigint | Number of indexes logically read in the Compression Unit |
   +------------+--------+----------------------------------------------------------+
   | cidx_hit   | bigint | Number of index hits in the Compression Unit             |
   +------------+--------+----------------------------------------------------------+
