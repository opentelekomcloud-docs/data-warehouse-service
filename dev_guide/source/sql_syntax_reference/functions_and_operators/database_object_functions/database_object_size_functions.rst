:original_name: dws_06_0302.html

.. _dws_06_0302:

Database Object Size Functions
==============================

Database object size functions calculate the actual disk space used by database objects.

pg_column_size(any)
-------------------

Description: Specifies the number of bytes used to store a particular value (possibly compressed).

Return type: integer

Note: **pg_column_size** displays the space for storing an independent data value.

::

   postgres=#SELECT pg_column_size(1);
    pg_column_size
   ----------------
                 4
   (1 row)

pg_database_size(oid)
---------------------

Description: Specifies the disk space used by the database with the specified OID.

Return type: bigint

pg_database_size(name)
----------------------

Description: Specifies the disk space used by the database with the specified name.

Return type: bigint

Note: **pg_database_size** receives the OID or name of a database and returns the disk space used by the corresponding object.

Example:

::

   postgres=#SELECT pg_database_size('gaussdb');
    pg_database_size
   ------------------
            51590112
   (1 row)

pg_relation_size(oid)
---------------------

Description: Specifies the disk space used by the table with a specified OID or index.

Return type: bigint

get_db_source_datasize()
------------------------

Description: Estimates the total size of non-compressed data in the current database.

Return type: bigint

Note: (1) **ANALYZE** must be performed before this function is called. (2) Calculate the total size of non-compressed data by estimating the compression rate of column-store tables.

Example:

::

   postgres=#analyze;
   ANALYZE
   postgres=#SELECT get_db_source_datasize();
    get_db_source_datasize
   ------------------------
               35384925667
   (1 row)

pg_relation_size(text)
----------------------

Description: Specifies the disk space used by the table with a specified name or index. The table name can be schema-qualified.

Return type: bigint

pg_relation_size(relation regclass, fork text)
----------------------------------------------

Description: Specifies the disk space used by the specified bifurcating tree ('main', 'fsm', or 'vm') of a certain table or index.

Return type: bigint

pg_relation_size(relation regclass)
-----------------------------------

Description: Is an abbreviation of **pg_relation_size(..., 'main')**.

Return type: bigint

Note: **pg_relation_size** returns the size in bytes of the table, index, or compressed table with the specified OID or name.

pg_partition_size(oid,oid)
--------------------------

Description: Specifies the disk space used by the partition with a specified OID. The first **oid** is the OID of the table and the second **oid** is the OID of the partition.

Return type: bigint

pg_partition_size(text, text)
-----------------------------

Description: Specifies the disk space used by the partition with a specified name. The first **text** is the table name and the second **text** is the partition name.

Return type: bigint

pg_partition_indexes_size(oid,oid)
----------------------------------

Description: Specifies the disk space used by the index of the partition with a specified OID. The first **oid** is the OID of the table and the second **oid** is the OID of the partition.

Return type: bigint

pg_partition_indexes_size(text,text)
------------------------------------

Description: Specifies the disk space used by the index of the partition with a specified name. The first **text** is the table name and the second **text** is the partition name.

Return type: bigint

pg_indexes_size(regclass)
-------------------------

Description: Specifies the total disk space used by the index appended to the specified table.

Return type: bigint

pg_size_pretty(bigint)
----------------------

Description: Converts the calculated byte size into a size readable to human beings.

Return type: text

pg_size_pretty(numeric)
-----------------------

Description: Converts the calculated byte size indicated by a numeral into a size readable to human beings.

Return type: text

Note: **pg_size_pretty** formats the results of other functions into a human-readable format. KB/MB/GB/TB can be used.

pg_table_size(regclass)
-----------------------

Description: Specifies the disk space used by the specified table, excluding indexes (but including TOAST, free space mapping, and visibility mapping).

Return type: bigint

pg_total_relation_size(oid)
---------------------------

Description: Specifies the disk space used by the table with a specified OID, including the index and the compressed data.

Return type: bigint

pg_total_relation_size(regclass)
--------------------------------

Description: Specifies the total disk space used by the specified table, including all indexes and TOAST data.

Return type: bigint

pg_total_relation_size(text)
----------------------------

Description: This function specifies the disk space used by the table with the specified name, including the index and compressed data. The table name can be schema-qualified.

Return type: bigint

Note: **pg_total_relation_size** can specify the OID or name of a table or compressed table, and then return the data in bytes and the sizes of all related indexes and compressed tables.

pg_obs_file_size(regclass)
--------------------------

Description: This function obtains the CU file size, file name, and bucket ID stored on OBS by specifying the OID or name of a V3 column-store table. This function is supported only by clusters of version 9.1.0 or later.

Parameter: The input parameter can be the table OID or table name.

Return type: record

.. table:: **Table 1** Return fields

   ======== ======= ======================================
   Field    Type    Description
   ======== ======= ======================================
   bucketid integer ID of the bucket for storing CU files.
   filename text    CU file name.
   size     bigint  CU file size, in bytes.
   ======== ======= ======================================

Example:

::

   SELECT * FROM pg_obs_file_size('t3_col_part_dif_partition');
    bucketid |     filename     | size
   ----------+------------------+------
           9 | 23488119324673.0 |  512
          22 | 23488102760449.0 |  512
          21 | 23488119521281.0 |  512
          16 | 23488102662145.0 |  512
   (4 rows)

pg_obs_file_size(text, text)
----------------------------

Description: This function specifies a partition of a column-store V3 table (primary table name or partitioned table) and returns the size, file name, and bucket ID of the CU file stored on OBS. This function is supported only by clusters of version 9.1.0 or later.

Parameter: The input parameter can be the table OID or table name. The partition name is used as the partition name.

Return type: record

======== ======= ======================================
Field    Type    Description
======== ======= ======================================
bucketid integer ID of the bucket for storing CU files.
filename text    CU file name.
size     bigint  CU file size, in bytes.
======== ======= ======================================
