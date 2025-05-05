:original_name: dws_06_0177.html

.. _dws_06_0177:

CREATE TABLE
============

Function
--------

Creates a new empty table in the current database.

This table is owned by the user who executes the command. However, if the system administrator creates a table in the schema with the same name as a common user, the owner of the table is the user (not the system administrator).

Precautions
-----------

-  For details about the data types supported by column-store tables, see :ref:`Data Types Supported by Column-Store Tables <dws_06_0024>`.
-  It is recommended that the number of column-store and HDFS partitioned tables do not exceed 1000.
-  The primary key constraint and unique constraint in the table must contain a distribution column.
-  The data type of the distribution column in an existing table cannot be modified.
-  A system column cannot be designated as a primary key in a row-store REPLICATION distributed table.
-  If an error occurs during table creation, after it is fixed, the system may fail to delete the empty disk files created before the last automatic clearance. This problem seldom occurs.
-  Column-store tables support the **PARTIAL CLUSTER KEY** and table-level primary key and unique constraints, but do not support table-level foreign key constraints.
-  Only the NULL, NOT NULL, and DEFAULT constant values can be used as column-store table column constraints.
-  Whether column-store tables support a delta table is specified by the **enable_delta** parameter. The threshold for storing data into a delta table is specified by the **deltarow_threshold** parameter. Using column-store tables with delta tables is not recommended. This may cause disk bloat and performance deterioration due to delayed merge.
-  Multi-temperature tables support only partitioned column-store tables and depend on available OBS tablespaces.
-  Multi-temperature tables support only the default tablespace **default_obs_tbs**. If you need to add an OBS tablespace, contact technical support.
-  To create a column-store table, explicitly set **orientation** to **column**. When creating a local table in the storage-compute decoupling edition with all data stored on EVS disks, make sure to explicitly set **colversion** to **2.0**.
-  Once a table is created, it cannot be switched from a non-V3 table to a V3 table using the **ALTER TABLE** syntax. In other words, a non-V3 table with a colversion of 2.0 cannot be converted to a V3 table with a colversion of 3.0.
-  Delta tables and column-store level-2 partitions cannot be set for V3 tables (for example, **colversion = 3.0**, storage and compute separated tables).
-  V3 tables cannot be configured as H-Store tables, hot-and-cold-data-separated tables, or time sequence tables.
-  Global temporary tables and temporary tables cannot be created for V3 tables. Any created temporary tables are automatically converted to temporary tables with colversion 2.0.

.. warning::

   -  You are not advised to specify a user-defined tablespace when creating an ordinary table.
   -  Do not specify the **COMPRESS** compression attribute when creating a row-store table.
   -  When creating a hash-distributed table object, ensure that data is evenly distributed. (For a table with more than 10 GB data, the skew rate must be less than 10%.)
   -  When creating a table object for **REPLICATION** distribution, ensure that the number of rows in the table is less than 1 million.
   -  When creating an H-Store table, ensure that the database GUC parameter settings meet the following requirements:

      -  **autovacuum** is set to **on**.
      -  The value of **autovacuum_max_workers_hstore** is greater than **0**.
      -  The value of **autovacuum_max_workers** is greater than that of **autovacuum_max_workers_hstore**.

   -  For a large table (with more than 50 million rows of data) that contains the time field, the table must be designed as a partition table and the partition interval must be properly designed based on the query characteristics.
   -  For a table where a large amount of data needs to be added, deleted, or modified, it is recommended that the number of indexes be less than or equal to three. The maximum number of indexes is five.
   -  For more information about development and design specifications, see "GaussDB(DWS) Development and Design Proposal" in the *Data Warehouse Service (DWS) Developer Guide*.

Syntax
------

::

   CREATE [ [ GLOBAL | LOCAL | VOLATILE ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE [ IF NOT EXISTS ] table_name
       { ({ column_name data_type [ compress_mode ] [ COLLATE collation ] [ column_constraint [ ... ] ]
           | table_constraint
           | LIKE source_table [ like_option [...] ] }
           [, ... ])|
           LIKE source_table [ like_option [...] ] }
       [ WITH ( {storage_parameter = value} [, ... ] ) ]
       [ ON COMMIT { PRESERVE ROWS | DELETE ROWS } ]
       [ COMPRESS | NOCOMPRESS ]
       [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { HASH ( column_name [,...] ) } } ]
       [ TO { GROUP groupname | NODE ( nodename [, ... ] ) } ]
       [ COMMENT [=] 'text' ];

-  **column_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { NOT NULL |
        NULL |
        CHECK ( expression ) |
        DEFAULT default_expr |
        ON UPDATE on_update_expr |
        COMMENT 'text' |
        UNIQUE [ NULLS [NOT] DISTINCT | NULLS IGNORE ] index_parameters |
        PRIMARY KEY index_parameters |
        REFERENCES reftable [ ( refcolumn ) ] }
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **compress_mode** of a column is as follows:

   ::

      { DELTA | PREFIX | DICTIONARY | NUMSTR | NOCOMPRESS }

-  **table_constraint** is as follows:

   ::

      [ CONSTRAINT constraint_name ]
      { CHECK ( expression ) |
        UNIQUE [ NULLS [NOT] DISTINCT | NULLS IGNORE ] ( column_name [, ... ] ) index_parameters |
        PRIMARY KEY ( column_name [, ... ] ) index_parameters |
        PARTIAL CLUSTER KEY ( column_name [, ... ] ) }
      [ DEFERRABLE | NOT DEFERRABLE | INITIALLY DEFERRED | INITIALLY IMMEDIATE ]

-  **like_option** is as follows:

   ::

      { INCLUDING | EXCLUDING } { DEFAULTS | CONSTRAINTS | INDEXES | STORAGE | COMMENTS | PARTITION | RELOPTIONS | DISTRIBUTION | DROPCOLUMNS | ALL }

-  **index_parameters** is as follows:

   ::

      [ WITH ( {storage_parameter = value} [, ... ] ) ]

Table Design Reference
----------------------

GaussDB(DWS) is compatible with the PostgreSQL ecosystem. Row storage and its B-tree index are similar to those of PostgreSQL. Column storage and its index are self-developed. When creating a table, it is crucial to choose the right storage method, distribution column, partition key, and index. This ensures efficient data access during SQL execution, reducing I/O consumption. The following figure illustrates the process from SQL statement initiation to data acquisition, helping you understand the function of each technical method for performance optimization.

|image1|

#. When the SQL statement is executed, the partition table is optimized using the Partition Column to pinpoint the specific partition.
#. The Distribute Column is used in a distributed hash table to quickly identify the data shard where the data resides. The data shard is located on a DN in a storage-compute coupled architecture, while in a storage-compute decoupled architecture, it is located on a bucket.
#. In row-store mode, B-tree is used to quickly locate the data page. In column-store mode, the **min-max** index is used to quickly locate the CU data block that may contain relevant data. This index is particularly effective when filtering on the PCK column.
#. The system automatically maintains the **min-max** index for all columns in the column-store mode. There is no need for manual index definition. The **min-max** index is used for coarse filtering. CU data blocks meeting the min-max condition may not contain data rows that meet the filter condition. If a bitmap column is defined, the bitmap index can quickly locate the row number of data that meets the filter condition in the CU. For ordered CUs, binary search is also used to quickly locate the row number of data.
#. Column storage supports B-tree and GIN indexes, which can quickly locate the CU and row number of data that meets the conditions. However, due to high index maintenance costs, it is advised to use bitmap indexes instead unless there are high performance requirements for point queries.

The following table lists the existing optimization methods of GaussDB(DWS).

.. table:: **Table 1** Optimization methods

   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | No.         | Method                     | Usage                                                                                                                                                                                                                                                                                   | Example SQL                                                   | Modifiable After Creation                                                                                             |
   +=============+============================+=========================================================================================================================================================================================================================================================================================+===============================================================+=======================================================================================================================+
   | 1           | String                     | #. The string type has slower performance compared to the fixed-length type, so it is not recommended for scenarios where the fixed-length type is more suitable.                                                                                                                       | ``-``                                                         | Yes (The existing data can be rewritten.)                                                                             |
   |             |                            | #. If the specified length is less than 16, performance will be significantly improved.                                                                                                                                                                                                 |                                                               |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 2           | Numeric                    | Specifying precision for the numeric type is essential for improving performance. It is not advisable to use the numeric type without specifying precision.                                                                                                                             | ``-``                                                         | Yes (The existing data can be rewritten.)                                                                             |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 3           | Partition by Column        | #. This requires user-defined settings and is designed for partitioned tables. Pruning is possible using partition keys and partition-wise joins are supported. This method is suitable for equality and range queries.                                                                 | ::                                                            | No (You need to create a new table to make modifications.)                                                            |
   |             |                            | #. Having more than 1000 partitions is not recommended, and it is advisable to limit the number of partition columns to two.                                                                                                                                                            |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE t1.c1='p1';                         |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 4           | secondary_part_column      | #. This requires user-defined settings and is applicable only to column-store tables and equality queries.                                                                                                                                                                              | ::                                                            | No (You need to create a new table to make modifications.)                                                            |
   |             |                            | #. Specify a level-2 partition on the most commonly used equivalent filter.                                                                                                                                                                                                             |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE t1.c1='p1';                         |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 5           | Distribute by Column       | This requires user-defined settings and is suitable for join fields that require frequent **GROUP BY** or multi-table joins. It reduces data shuffling through local joins and is ideal for equality queries.                                                                           | ::                                                            | No (You need to create a new table to make modifications.)                                                            |
   |             |                            |                                                                                                                                                                                                                                                                                         |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 join t2 on  t1.c3 = t2.c1;                |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 6           | Bitmap column              | Define the bitmap index (cardinality <= 32) or bloom filter (cardinality > 32) based on the repeated values in the CU. This method is applicable to equivalent queries of varchar or text type columns. It is advised to create indexes on columns involved in the **WHERE** condition. | ::                                                            | Yes (Modification does not rewrite existing data. Only the new data is affected.)                                     |
   |             |                            |                                                                                                                                                                                                                                                                                         |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE t1.c4='hello';                      |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 7           | **min-max** index          | #. The **min-max** index is automatically generated and can be used for both equality and range queries.                                                                                                                                                                                | ::                                                            | Yes (The PCK columns can be modified. Modification does not rewrite existing data and only the new data is affected.) |
   |             |                            | #. The **min-max** filtering effect depends on the data order. Specifying the PCK column enhances the filtering effect.                                                                                                                                                                 |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE c3 > 100 and c3 < 200;              |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 8           | Primary key (B-tree index) | #. UPSERT data import strongly depends on the primary key and needs to be customized. It is applicable to equality and range queries. We suggest limiting the number of columns to five or fewer.                                                                                       | ::                                                            | Yes (The index can be modified and re-created.)                                                                       |
   |             |                            | #. If service requirements are met, it is better to use fixed-length type columns. During definition, place columns with more distinct values at the beginning.                                                                                                                         |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE c3 > 100 and c3 < 200;              |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 9           | GIN index                  | #. This requires user-defined settings and is suitable for multi-condition equality queries. Avoid using columns with more than 1 million distinct values.                                                                                                                              | ::                                                            | Yes (The index can be modified and re-created.)                                                                       |
   |             |                            | #. It is recommended when the data volume after filtering is less than 1000. If the data volume remains large after filtering, it is not recommended.                                                                                                                                   |                                                               |                                                                                                                       |
   |             |                            |                                                                                                                                                                                                                                                                                         |    SELECT * FROM t1 WHERE c1 = 100 and c3 = 200 and c2 = 105; |                                                                                                                       |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+
   | 10          | Orientation=column/row     | This method specifies whether a table is stored in rows or columns. Row-store tables cannot be compressed and are best suited for point queries and frequent updates. Column-store tables can be compressed and are ideal for analysis purposes.                                        | ``-``                                                         | No (You need to create a new table to make modifications.)                                                            |
   +-------------+----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+---------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------+

Parameters
----------

-  **UNLOGGED**

   If this key word is specified, the created table is not a log table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, an unlogged table is automatically truncated after a crash or unclean shutdown, incurring data loss risks. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are not automatically logged as well.

   Usage scenario: Unlogged tables do not ensure safe data. You can back up data before using unlogged tables; for example, you should back up the data before a system upgrade.

   Troubleshooting: If data is missing in the indexes of unlogged tables due to some unexpected operations such as an unclean shutdown, you should re-create the indexes with errors.

   .. important::

      #. The UNLOGGED table uses no primary/standby mechanism. In the case of system faults or abnormal breakpoints, data loss may occur. Therefore, the UNLOGGED table cannot be used to store basic data.
      #. Starting from version 9.1.0, UNLOGGED tables are automatically saved in the **pg_unlogged** tablespace and cannot be moved or assigned to other tablespaces.
      #. After an earlier version is upgraded to 9.1.0, the UNLOGGED table created in the earlier version is still stored in the original tablespace.
      #. If the instance restarts unexpectedly, the UNLOGGED table will be reset, which can impact the instance's recovery time objective (RTO). Version 9.1.0 has a script called **switch_unlogged_tablespace.py** that can move unlogged tables to optimize the recovery time objective (RTO). This script works together with the GUC parameter **enable_unlogged_tablespace_compat**.

-  .. _en-us_topic_0000001764675138__l40601c13ccdb4b5d85be38edd4f99676:

   **GLOBAL \| LOCAL** \| **VOLATILE**

   Specify the keywords **GLOBAL**, **LOCAL**, and **VOLATILE** before **TEMP** or **TEMPORARY** to create temporary tables with different attributes. Global temporary tables are supported only by 8.2.1.220 and later cluster versions.

   -  If **LOCAL** is specified, a local temporary table is created.

   -  If keyword **GLOBAL** is specified, the attributes of the temporary table depend on the GUC parameter **enable_global_temp_table**. The default value of this parameter is **ON**.

      If **enable_global_temp_tabl** is set to **on**, a global temporary table **GLOBAL** is created.

      If **enable_global_temp_tabl** is set to **off,** a local temporary table **LOCAL** is created. You can also specify keyword **LOCAL** to reach the same effect.

   -  If **VOLATILE** is specified, a temporary volatile table is created.

   -  If **default_temptable_type** is set to **local**, temporary tables created without keywords are local temporary tables. If **default_temptable_type** is set to **volatile**, temporary tables created without keywords are volatile temporary tables.

-  **TEMPORARY \| TEMP**

   If **TEMP** or **TEMPORARY** is specified, the created table is a temporary table. Temporary tables are automatically dropped at the end of a session, or optionally at the end of the current transaction. Therefore, apart from CN and other CN errors connected by the current session, you can still create and use temporary table in the current session. Temporary tables are created only in the current session. If a DDL statement involves operations on temporary tables, a DDL error will be generated. Therefore, you are not advised to perform operations on temporary tables in DDL statements. **TEMP** is equivalent to **TEMPORARY**.

   .. important::

      -  Local or volatile temporary tables are visible to the current session through schema of the **pg_temp** start. Users should not delete schema started with **pg_temp**, **pg_toast_temp**.
      -  If **TEMPORARY** or **TEMP** is not specified when you create a table and the schema of the specified table starts with **pg_temp\_**, the table is created as a temporary table.
      -  Similar to common tables, all metadata of local temporary tables is stored in system catalogs. Volatile temporary tables store table structure metadata in memory, except the schema metadata. Compared with local temporary tables, volatile temporary tables have the following constraints:

         -  After a CN or DN is restarted, data in its memory will be lost, and accordingly, volatile temporary tables on it will become invalid.
         -  Currently, volatile temporary tables do not support table structure modification, such as **ALTER** and **GRANT**.
         -  Volatile temporary tables and local temporary tables share temporary schemas. Therefore, in the same session, the VOLATILE temporary table and local temporary table cannot have the same name.
         -  Volatile temporary table information is not stored in system catalogs. Therefore, Volatile metadata cannot be queried by running DML statements in system catalogs.
         -  Volatile temporary tables support only common row-store and column-store tables. Delta tables, time series tables, and cold and hot tables are not supported.
         -  Views cannot be created based on volatile temporary tables.
         -  A tablespace cannot be specified when a temporary table is created. The default tablespace of a volatile temporary table is **pg_volatile**.
         -  The following constraints cannot be specified when a volatile temporary table is created: CHECK, UNIQUE, PRIMARY KEY, TRIGGER, EXCLUDE, and PARTIAL CLUSTER.

      -  Similar to common tables, all metadata in global temporary tables is stored in system catalogs.

         -  The difference between a global temporary table and a local temporary table is that in a global temporary table, the metadata is not deleted when a session exits, but the session data is deleted. Data of different sessions is independent but shares the metadata of the same global temporary table.
         -  A global temporary table has a schema that is similar to a regular table, unlike a local or volatile temporary table which has a schema starting with **pg_temp**. This means that a global temporary table can have the same name as a local or volatile temporary table.
         -  Global temporary tables support only common row-store and column-store tables. Delta tables, time series tables, and cold and hot tables are not supported.
         -  Operations cannot be performed on global temporary tables of other logical clusters.

-  **IF NOT EXISTS**

   If **IF NOT EXISTS** is specified, a table will be created if there is no table using the specified name. If there is already a table using the specified name, no error will be reported. A message will be displayed indicating that the table already exists, and the database will skip table creation.

-  **table_name**

   Specifies the name of the table to be created.

   The table name can contain a maximum of 63 characters, including letters, digits, underscores (_), dollar signs ($), and number signs (#). It must start with a letter or underscore (_).

   A table name enclosed in double quotation marks can contain spaces and special characters. However, you are not advised to use these characters in a table name because they may make it difficult to reference and use. In addition, they may be processed differently under different database compatibility modes.

-  **column_name**

   Specifies the name of a column to be created in the new table.

   The column name can contain a maximum of 63 characters, including letters, digits, underscores (_), dollar signs ($), and number signs (#). It must start with a letter or underscore (_).

-  **data_type**

   Specifies the data type of the column.

   .. note::

      In a database compatible with Teradata or MySQL syntax, if the data type of a column is set to DATE, the DATE type is returned. Otherwise, the TIMESTAMP type is returned.

-  **compress_mode**

   Specifies the compress option of the table, only available for row-store table. The option specifies the algorithm preferentially used by table columns.

   Value range: DELTA, PREFIX, DICTIONARY, NUMSTR, NOCOMPRESS

-  **COLLATE collation**

   Assigns a collation to the column (which must be of a collatable data type). If no collation is specified, the default collation is used.

-  **LIKE source_table [ like_option ... ]**

   Specifies a table from which the new table automatically copies all column names, their data types, and their not-null constraints.

   The new table and the source table are decoupled after creation is complete. Changes to the source table will not be applied to the new table, and it is not possible to include data of the new table in scans of the source table.

   Columns and constraints copied by **LIKE** are not merged with the same name. If the same name is specified explicitly or in another **LIKE** clause, an error is reported.

   -  The default expressions or the **ON UPDATE** expressions are copied from the source table to the new table only if **INCLUDING DEFAULTS** is specified. The default behavior is to exclude default expressions, resulting in the copied columns in the new table having default values **NULL**.
   -  The **CHECK** constraints are copied from the source table to the new table only when **INCLUDING CONSTRAINTS** is specified. Other types of constraints are never copied to the new table. **NOT NULL** constraints are always copied to the new table. These rules also apply to column constraints and table constraints.
   -  Any indexes on the source table will not be created on the new table, unless the **INCLUDING INDEXES** clause is specified.
   -  STORAGE settings for the copied column definitions are copied only if **INCLUDING STORAGE** is specified. The default behavior is to exclude **STORAGE** settings.
   -  If **INCLUDING COMMENTS** is specified, comments for the copied columns, constraints, and indexes are copied. The default behavior is to exclude comments.
   -  If **INCLUDING PARTITION** is specified, the partition definitions of the source table are copied to the new table, and the new table no longer uses the **PARTITION BY** clause. The default behavior is to exclude partition definition of the source table.
   -  If **INCLUDING RELOPTIONS** is specified, the storage parameter (**WITH** clause of the source table) of the source table is copied to the new table. The default behavior is to exclude partition definition of the storage parameter of the source table.

      .. note::

         **PERIOD** and **TTL** in the **WITH** clause are partition-related parameters. **LIKE INCLUDING RELOPTIONS** will not be copied to the new table. To copy **LIKE INCLUDING RELOPTIONS**, use **INCLUDING PARTITION**.

   -  If **INCLUDING DISTRIBUTION** is specified, the distribution information of the source table is copied to the new table, including distribution type and column, and the new table no longer use the **DISTRIBUTE BY** clause. The default behavior is to exclude distribution information of the source table.
   -  If **INCLUDING DROPCOLUMNS** is specified, the deleted column information in the source table is copied to the new table. By default, the deleted column information of the source table is not copied.
   -  **INCLUDING ALL** contains the meaning of **INCLUDING DEFAULTS**, **INCLUDING CONSTRAINTS**, **INCLUDING INDEXES**, **INCLUDING STORAGE**, **INCLUDING COMMENTS**, **INCLUDING PARTITION**, **INCLUDING RELOPTIONS**, **INCLUDING DISTRIBUTION**, and **INCLUDING DROPCOLUMNS**.
   -  If EXCLUDING is specified, the specified parameters are not included.
   -  For an OBS multi-temperature table, all partitions of the new table are local hot partitions after **INCLUDING PARTITION** is specified.

   .. important::

      -  If the source table contains a sequence with the SERIAL, BIGSERIAL, or SMALLSERIAL data type, or a column in the source table is a sequence by default and the sequence is created for this table by using **CREATE SEQUENCE...** **OWNED BY**, these sequences will not be copied to the new table, and another sequence specific to the new table will be created. This is different from earlier versions. To share a sequence between the source table and new table, create a shared sequence (do not use **OWNED BY**) and set a column in the source table to this sequence.
      -  You are not advised to set a column in the source table to the sequence specific to another table especially when the table is distributed in specific Node Groups, because doing so may result in **CREATE TABLE ... LIKE** execution failures. In addition, doing so may cause the sequence to become invalid in the source sequence because the sequence will also be deleted from the source table when it is deleted from the table that the sequence is specific to. To share a sequence among multiple tables, you are advised to create a shared sequence for them.

-  **WITH ( { storage_parameter = value } [, ... ] )**

   Specifies an optional storage parameter for a table or an index.

   .. note::

      Using Numeric of any precision to define column, specifies precision p and scale s. When precision and scale are not specified, the input will be displayed.

   The description of parameters is as follows:

   -  FILLFACTOR

      The fillfactor of a table is a percentage between 10 and 100. 100 (complete packing) is the default value. When a smaller fillfactor is specified, **INSERT** operations pack table pages only to the indicated percentage. The remaining space on each page is reserved for updating rows on that page. This gives **UPDATE** a chance to place the updated copy of a row on the same page, which is more efficient than placing it on a different page. For a table whose records are never updated, setting the fillfactor to 100 (complete packing) is the appropriate choice, but in heavily updated tables smaller fillfactors are appropriate. The parameter has no meaning for column-store tables.

      Value range: 10 to 100

   -  ORIENTATION

      Specifies the storage mode (row-store, column-store) for table data. This parameter cannot be modified once it is set.

      Valid value:

      -  **ROW** indicates that table data is stored in rows.

         **ROW** applies to OLTP service, which has many interactive transactions. An interaction involves many columns in the table. Using ROW can improve the efficiency.

      -  **COLUMN** indicates that the data is stored in columns.

         **COLUMN** applies to the data warehouse service, which has a large amount of aggregation computing, and involves a few column operations.

      Default value: **ROW** (row-store)

   -  COMPRESSION

      Specifies the compression level of the table data. It determines the compression ratio and time. Generally, the higher the level of compression, the higher the ratio, the longer the time, and the lower the level of compression, the lower the ratio, the shorter the time. The actual compression ratio depends on the distribution characteristics of loading table data.

      Valid value:

      The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**. When this parameter is set to **YES**, the compression level is **LOW** by default.

      .. note::

         -  Currently, row-store table compression is not supported.
         -  To determine the size of a new GaussDB(DWS) cluster, consider the size of ORC data compressed and migrated to column-store tables in GaussDB(DWS). If the compression level is low, the size of a copy is about 1.5 to 2 times that of ORC. If the compression level is high, the size of a copy is basically the same as that of ORC.
         -  The middle compression of column-stores uses dictionary compression. For data not suitable for dictionary compression, the file size after middle compression may be greater than that of after low compression.

      GaussDB(DWS) provides the following compression algorithms:

      .. table:: **Table 2** Compression algorithms for column-based storage

         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | COMPRESSION | NUMERIC                                                | STRING                               | INT                                                     |
         +=============+========================================================+======================================+=========================================================+
         | LOW         | Delta compression + RLE compression                    | LZ4 compression                      | Delta compression (RLE is optional.)                    |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | MIDDLE      | Delta compression + RLE compression + LZ4 compression  | dict compression or LZ4 compression  | Delta compression or LZ4 compression (RLE is optional)  |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+
         | HIGH        | Delta compression + RLE compression + zlib compression | dict compression or zlib compression | Delta compression or zlib compression (RLE is optional) |
         +-------------+--------------------------------------------------------+--------------------------------------+---------------------------------------------------------+

   -  COMPRESSLEVEL

      Specifies the compression level of the table data. It determines the compression ratio and time. This divides a compression level into sublevels, providing you with more choices for compression rate and duration. As the value becomes greater, the compression rate becomes higher and duration longer at the same compression level. The parameter is only valid for column-store tables.

      Value range: 0-3.

      Default value: **0**

   -  TTL

      Schedules the partition deletion tasks in a partitioned table. By default, no partition deletion task is created.

      Value range: 1 hour-100 years

   -  PERIOD

      Schedules the partition creation tasks in a partitioned table. If **TTL** has been configured, **PERIOD** cannot be greater than **TTL**.

      Value range: 1 hour-100 years

      Default value: 1 day

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store tables.

      Value range: 10000 to 60000

      Default value: 60,000

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store tables.

      Value range: 600000 to 2147483647

      Default value: 4,200,000

   -  time_format

      You can use auto-increment and decrement partitions for **INT4**, **INT8**, **VARCHAR**, and **TEXT** columns, which are commonly used for storing time-related data. The **time_format** option is applicable only when the partition key is **INT4**, **INT8**, **VARCHAR**, or **TEXT** and a period is specified. This is supported only by clusters of version 9.1.0.200 or later.

      #. The value of **time_format** must comply with the PostgreSQL specifications, for example, **yyyymmdd**.

      #. Restrictions on **time_format** differ by partition key type.

         **VARCHAR**/**TEXT**:

         -  The precision can be accurate to seconds.
         -  The value cannot contain letters, for example, **month**, **am**, and **pm**.
         -  The time must be arranged in descending order, for example, year, month, day, hour, minute, and second.

         **INT4**/**INT8**:

         -  The precision can be accurate to hours.
         -  The value can contain only **Y**, **M**, **D**, and **HH24**.
         -  The time must be arranged in descending order, for example, year, month, day, and hour.

      #. **ALTER** restrictions:

         -  The set operation is not supported.
         -  When period is reset (indicating that automatic partitioning is disabled and a message is displayed), you can reset this option.

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables. If **COLVERSION** is set to **3.0**, **enable_delta** cannot be turned on because this parameter is not supported by V3 tables.

      Using column-store tables with delta tables is not recommended. This may cause disk bloat and performance deterioration due to delayed merge.

      Default value: **off**

   -  enable_hstore

      Specifies whether an H-Store table will be created (based on column-store tables). The parameter is only valid for column-store tables. This parameter is supported by version 8.2.0.100 or later clusters. If **COLVERSION** is set to **3.0**, **enable_delta** cannot be turned on because this parameter is not supported by V3 tables.

      Default value: **off**

      .. note::

         If this parameter is enabled, the following GUC parameters must be set to ensure that H-Store tables are cleared.

         Set **autovacuum** to **on**, **autovacuum_max_workers** to **6**, and **autovacuum_max_workers_hstore** to **3**.

   -  enable_disaster_cstore

      Specifies whether fine-grained DR will be enabled for column-store tables. This parameter only takes effect on column-store tables whose COLVERSION is 2.0 and cannot be set to **true** if **enable_hstore** is **true**. This parameter is supported by version 8.2.0.100 or later clusters.

   -  fine_disaster_table_role

      This parameter has been discarded in version 8.2.1 and is reserved for compatibility with earlier versions. This parameter is invalid in the current version.

      Specifies whether the fine-grained DR table will be set as a primary or secondary table. This parameter can be **true** only when the **enable_disaster_cstore** parameter has been set to **true**.

      Valid value:

      -  **primary**: Specifies the primary fine-grained DR table.
      -  **standby**: Specifies the standby fine-grained DR table.

   -  DELTAROW_THRESHOLD

      Specifies the upper limit of to-be-imported rows for triggering the data import to a delta table when data is to be imported to a column-store table. This parameter takes effect only if the **enable_delta** table parameter is set to **on**. The parameter is only valid for column-store tables.

      Value range: 0 to 60000

      **Default value**: **6000**

   -  COLVERSION

      Specifies the version of the column-store format. You can switch between different storage formats.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      **3.0**: Each column of a column-store table is stored in a file. The file is stored in the OBS file system and named **C1_fileid.0**.

      Default value: The default value for the storage-compute coupled version is 2.0, while for the storage-compute decoupling version, it is 3.0.

      The value of **COLVERSION** can only be set to **2.0** for OBS multi-temperature tables.

      .. note::

         -  For clusters of version 8.1.0, the default value of this parameter is **1.0**. For clusters of version 8.1.1 or later, the default value of this parameter is **2.0**. If the cluster version is upgraded from 8.1.0 to 8.1.1 or later, the default value of this parameter changes from **1.0** to **2.0**.
         -  When creating a column-store table, set **COLVERSION** to **2.0**. Compared with the **1.0** storage format, the performance is significantly improved:

            -  The time required for creating a column-store wide table is significantly reduced.
            -  In the Roach data backup scenario, the backup time is significantly reduced.
            -  The build and catch up time is greatly reduced.
            -  The occupied disk space decreases significantly.

         -  The storage-compute decoupling 3.0 version is compatible with all column-store versions. When creating a table, you need to explicitly specify the value of **colversion** (**1.0**, **2.0**, or **3.0**). If **colversion** is set to **3.0**, a table in decoupled storage and computing mode is created. If **colversion** is not explicitly specified, a column-store table of version 3.0 is created by default. When creating a table with decoupled storage and compute nodes, set **colversion** to **3.0** and set **orientation** to **column**.
         -  V3 storage-compute decoupling tables do not support colversion switching using **ALTER TABLE**, for example, from 2.0 to 3.0.

   -  analyze_mode

      Specifies the mode of table-level auto-analyze.

      Valid value:

      -  **frozen**: disables all **ANALYZE** operations (dynamic sampling can still be triggered when no statistics are collected).
      -  **backend**: allows only **ANALYZE** triggered by **AUTOVACUUM** polling.
      -  **runtime**: allows only runtime **ANALYZE** triggered by the optimizer.
      -  **all**: Both backend and runtime **AUTO-ANALYZE** can be triggered.

      Default value: **all**

   -  incremental_analyze

      Specifies whether to enable the incremental analyze mode for partitioned tables. This parameter is valid only for partitioned tables and cannot be set for replicated tables. This is supported only by clusters of version 9.1.0.100 or later.

      The default value is **false**.

   -  SKIP_FPI_HINT

      Indicates whether to skip the hint bits operation when the full-page writes (FPW) log needs to be written during sequential scanning.

      Default value: **false**

      .. note::

         If **SKIP_FPI_HINT** is set to **true** and the checkpoint operation is performed on a table, no Xlog will be generated when the table is sequentially scanned. This applies to intermediate tables that are queried less frequently, reducing the size of Xlogs and improving query performance.

   -  on_commit_preserve_rows

      It is similar to **ON COMMIT { PRESERVE ROWS \| DELETE ROWS }**. The two parameters cannot be specified at the same time. This parameter is used only for global temporary tables.

      Default value: **true**

   -  enable_column_autovacuum_garbage

      Determines whether to enable CU rewriting logic for column-store tables using AUTOVACUUM. This parameter is supported only by clusters of version 8.2.1.100 or later.

      There is a low probability that an error is reported when lightweight UPDATE and AUTOVACUUM are executed concurrently. You can set the table-level parameter to **off** to avoid this problem.

      Default value: **true**

   -  secondary_part_column

      Specifies the name of a level-2 partition column in a column-store table. Only one column can be specified as the level-2 partition column. This parameter applies only to H-Store column-store tables. This parameter is supported only by clusters of version 8.3.0 or later.

      .. note::

         -  The column specified as a level-2 partition column cannot be deleted or modified.
         -  The level-2 partition column can be specified only when a table is created. After a table is created, the level-2 partition column cannot be modified.
         -  You are not advised to specify a distribution column as a level-2 partition column.
         -  The level-2 partition column determines how the table is logically split into hash partitions on DNs, which enhances the query performance for that column.

   -  secondary_part_num

      Specifies the number of level-2 partitions in a column-store table. This parameter applies only to H-Store column-store tables. This parameter is supported only by clusters of version 8.3.0 or later.

      Value range: 1 to 32

      Default value: **8**

      .. note::

         -  This parameter can be specified only when **secondary_part_column** is specified.
         -  The number of level-2 partitions can be specified only when a table is created and cannot be modified after the table is created.
         -  You are not advised to change the default value, which may affect the import and query performance.

   -  enable_hstore_opt

      If the **enable_hstore_opt** table-level parameter is enabled, the **enable_hstore** table-level parameter is also enabled by default. This parameter is supported only in cluster 8.3.0 and later versions. This parameter supports V2 .

      Default value: **false**

   -  bitmap_columns

      **bitmap index** is only applicable to the new H-Store (**hstore_opt** table). To generate the **bitmap index** mapping, you need to enable the **enable_hstore_opt** table-level parameter and set **bitmap_columns** to **Specified Columns**. This parameter is supported only by clusters of version 8.3.0 or later.

   -  enable_turbo_store

      Determines whether to create a turbo table (column-store tables). The parameter is only valid for column-store tables.

      Default value: **off**

      .. note::

         Turbo tables enhance the storage efficiency of numeric and varchar data types using the integers, leading to accelerated processing speeds for these types.

         -  For enhanced performance, it is advised to define the numeric type with **precision** and **scale (p, s)**. The storage allocation is as follows: For p <= 9, 4-byte integers are used for storage. For p <= 18, 8-byte integers are used. For p <= 37, 16-byte integers are used. If p > 37 or remain unspecified, a variable-length format is used, which is less space-efficient and yields suboptimal performance.
         -  If the maximum length (n) of the varchar type is less than or equal to 2 bytes, 2-byte integers are used for storage. If the maximum length (n) of the varchar type is less than or equal to 4 bytes, 4-byte integers are used for storage. If the maximum length (n) of the varchar type is less than or equal to 16 bytes, 16-byte integers are used for storage. For varchar type columns involved in **GROUP BY** or **HashJoin** operations, it is advisable to limit the maximum byte length n to 16 or fewer to leverage integer storage for strings, thereby enhancing performance.
         -  At present, the turbo table does not have support for the timing table, delta table, and lightweight update.

   -  .. _en-us_topic_0000001764675138__en-us_topic_0000001342465185_li14679114513562:

      **cache_policy** (This parameter is supported only in 9.0.2 and later versions where the storage and compute nodes are decoupled.)

      Specifies the cache mode of tables or partitioned tables (disks). If one of the following values is specified in the cache policy, hot cache is used. Otherwise, cold cache is used. Hot cache occupies more space than cold cache and uses more complex replacement policies.

      Valid value:

      -  **ALL**: Hot cache is used for the entire table.
      -  **NONE**: Cold cache is used for the entire table.
      -  **HPN**: The first *N* partitions in a partitioned table use hot cache. The rest of the partitions use cold cache.
      -  **HPL:** *P1, P2, ...*. In a partitioned table, the specified partitions use hot cache. The rest of the partitions use cold cache.

      The default value is **ALL**.

      .. note::

         -  For foreign tables and non-partitioned tables, only the **ALL** and **NONE** cache policies are supported.
         -  Only range-partitioned and list-partitioned internal tables support HPN and HPL cache policies.

-  **ON COMMIT { PRESERVE ROWS \| DELETE ROWS }**

   **ON COMMIT** determines what to do when you commit a temporary table creation operation. Global temporary tables support only the **PRESERVE ROWS** option.

   -  **PRESERVE ROWS** (Default): No special action is taken at the ends of transactions. The temporary table and its table data are unchanged.
   -  **DELETE ROWS**: All rows in the temporary table will be deleted at the end of each transaction block.

-  **COMPRESS \| NOCOMPRESS**

   If you specify **COMPRESS** in the **CREATE TABLE** statement, the compression feature is triggered in the case of a bulk **INSERT** operation. If this feature is enabled, a scan is performed for all tuple data within the page to generate a dictionary and then the tuple data is compressed and stored. If **NOCOMPRESS** is specified, the table is not compressed.

   Default value: **NOCOMPRESS**, tuple data is not compressed before storage.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   Valid value:

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **ROUNDROBIN**: Each row in the table is sent to each DN in turn. Therefore, data is evenly distributed on each DN. This value is supported only in 8.1.2 or later.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

      .. note::

         -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
         -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.
         -  If **TO GROUP** is set to a replication table node group (supported in 8.1.2 or later), **DISTRIBUTE BY** must be set to **REPLICATION**. If **DISTRIBUTE BY** is not specified, the created table is automatically set as a replication table.
         -  The hybrid data warehouse (standalone) has only one DN. Therefore, the distribution rule is ignored and cannot be modified.

   Default value: determined by the GUC parameter **default_distribution_mode**

   -  When **default_distribution_mode** is set to **roundrobin**, the default value of **DISTRIBUTE BY** is selected according to the following rules:

      #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
      #. If the primary key or unique constraint is not included during table creation, round-robin distribution is selected.

   -  When **default_distribution_mode** is set to **hash**, the default value of **DISTRIBUTE BY** is selected according to the following rules:

      #. If the primary key or unique constraint is included during table creation, hash distribution is selected. The distribution column is the column corresponding to the primary key or unique constraint.
      #. If the primary key or unique constraint is not included during table creation but there are columns whose data types can be used as distribution columns, hash distribution is selected. The distribution column is the first column whose data type can be used as a distribution column.
      #. If the primary key or unique constraint is not included during table creation and no column whose data type can be used as a distribution column exists, round-robin distribution is selected.

   The following data types can be used as distribution columns:

   -  Integer types: **TINYINT**, **SMALLINT**, **INT**, **BIGINT**, and **NUMERIC/DECIMAL**
   -  Character types: **CHAR**, **BPCHAR**, **VARCHAR**, **VARCHAR2**, **NVARCHAR2**, and **TEXT**
   -  Date/time types: **DATE**, **TIME**, **TIMETZ**, **TIMESTAMP**, **TIMESTAMPTZ**, **INTERVAL**, and **SMALLDATETIME**

   .. note::

      When you create a table, the choices of distribution keys and partition keys have major impact on SQL query performance. Therefore, choosing proper distribution column and partition key with strategies.

      -  Selecting an Appropriate Distribution Column

         In the data distributed table using Hash, an appropriate distributed array should be used to distribute and store data on multiple DNs evenly, preventing data skew (uneven data distribution across several DNs). Determine the proper distribution column based on the following principles:

         #. Determine whether data is skewed.

            Connect to the database and run the following statements to check the number of tuples on each DN: Replace *tablename* with the actual name of the table to be analyzed.

            .. code-block::

               SELECT a.count,b.node_name FROM (SELECT count(*) AS count,xc_node_id FROM tablename GROUP BY xc_node_id) a, pgxc_node b WHERE a.xc_node_id=b.node_id ORDER BY a.count DESC;

            If tuple numbers vary greatly (several times or tenfold) in each DN, a data skew occurs. Change the data distribution key based on the following principles:

         #. Run the ALTER TABLE statement to adjust the distribution column. The rules for selecting a distribution column are as follows:

            The column value of the distribution column should be discrete so that data can be evenly distributed on each DN. For example, you are advised to select the primary key of a table as the distribution column, and the ID card number as the distribution column in a personnel information table.

            With the above principles met, you can select join conditions as distribution keys so that join tasks can be pushed down to DNs, reducing the amount of data transferred between the DNs.

         #. If a proper distribution column cannot be found to make data evenly distributed on each DN, you can use the **REPLICATION** or **ROUNDROBIN** data distribution mode. The **REPLICATION** data distribution mode stores complete data on each DN. Therefore, if a table is large and no proper distribution column can be found, the **ROUNDROBIN** data distribution mode is recommended. The **ROUNDROBIN** data distribution mode is supported in 8.1.2 or later.

      -  Selecting appropriate partition keys

         In range partitioning, the table is partitioned into ranges defined by a key column or set of columns, with no overlap between the ranges of values assigned to different partitions. Each range has a dedicated partition for data storage.

         Modify partition keys to make the query result stored in the same or least partitions (partition pruning). Obtaining consecutive I/O to improve the query performance.

         In actual services, time is used to filter query objects. Therefore, you can use time as a partition key, and change the key value based on the total data volume and single data query volume.

-  **TO { GROUP groupname \| NODE ( nodename [, ... ] ) }**

   **TO GROUP** specifies the Node Group in which the table is created. Currently, it cannot be used for HDFS tables. **TO NODE** is used for internal scale-out tools.

   In logical cluster mode, if **TO GROUP** is not specified, the table is created in the node group associated with the logical cluster user by default. If the user, such as the administrator or a common user, does not manage the logical cluster, by default the table is created in the first logical cluster, which is the logical cluster with the smallest **OID** in **pgxc_group**.

   If the node group specified by **TO GROUP** is a replication table node group, the table is created on all CNs and DNs, but the replication table data is distributed only on the DNs in the replication table node group.

   The Storage-compute decoupling 3.0 supports read-only logical clusters. If a user is not bound to any read-only logical clusters but sets **TO GROUP** to a logical cluster in a table creation statement, an error will be reported during table creation. If a user bound to a read-only logical cluster creates a table, the table will be created in the logical cluster specified by the GUC parameter **default_storage_nodegroup**. If **default_storage_nodegroup** is set to **installation**, tables will be created in the first logical cluster.

-  **COMMENT [=] 'text'**

   The **COMMENT** clause can specify table comments during table creation.

-  **CONSTRAINT constraint_name**

   Specifies a name for a column or table constraint. The optional constraint clauses specify constraints that new or updated rows must satisfy for an insert or update operation to succeed.

   There are two ways to define constraints:

   -  A column constraint is defined as part of a column definition, and it is bound to a particular column.
   -  A table constraint is not bound to any particular columns but can apply to more than one column.

-  **NOT NULL**

   Indicates that the column is not allowed to contain **NULL** values.

-  **NULL**

   The column is allowed to contain **NULL** values. This is the default setting.

   This clause is only provided for compatibility with non-standard SQL databases. You are advised not to use this clause.

-  **CHECK ( expression )**

   Specifies an expression producing a Boolean result which new or updated rows must satisfy for an insert or update operation to succeed. Expressions evaluating to **TRUE** or **UNKNOWN** succeed. If any row of an insert or update operation produces a FALSE result, an error exception is raised and the insert or update does not alter the database.

   A check constraint specified as a column constraint should reference only the column's values, while an expression appearing in a table constraint can reference multiple columns.

   .. note::

      **<>NULL** and **!=NULL** are invalid in an expression. Change them to **IS NOT NULL**.

-  **DEFAULT default_expr**

   Assigns a default data value for a column. The value can be any variable-free expressions (Subqueries and cross-references to other columns in the current table are not allowed). The data type of the default expression must match the data type of the column.

   The default expression will be used in any insert operation that does not specify a value for the column. If there is no default value for a column, then the default value is **NULL**.

-  **ON UPDATE on_update_expr**

   The **ON UPDATE** clause specifies a timestamp function for a column. Ensure that the data type of the column for which the **ON UPDATE** clause specifies a timestamp function is timestamp or timestamptz.

   When an SQL statement containing the **UPDATE** operation is executed, this column is automatically updated to the time specified by the timestamp function.

   .. note::

      The **on_update_expr** function supports only CURRENT_TIMESTAMP, CURRENT_TIME, CURRENT_DATE, LOCALTIME, LOCALTIMESTAMP.

-  **COMMENT** **'text'**

   The **COMMENT** clause can specify a comment for a column.

-  **UNIQUE [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] index_parameters**

   **UNIQUE [ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ] ( column_name [, ... ] ) index_parameters**

   Specifies that a group of one or more columns of a table can contain only unique values.

   The **[ NULLS [ NOT ] DISTINCT \| NULLS IGNORE ]** field is used to specify how to process null values in the index column of the Unique index.

   Default value: This parameter is left empty by default. NULL values can be inserted repeatedly.

   When the inserted data is compared with the original data in the table, the NULL value can be processed in any of the following ways:

   -  NULLS DISTINCT: NULL values are unequal and can be inserted repeatedly.
   -  NULLS NOT DISTINCT: NULL values are equal. If all index columns are NULL, NULL values cannot be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.
   -  NULLS IGNORE: NULL values are skipped during the equivalent comparison. If all index columns are NULL, NULL values can be inserted repeatedly. If some index columns are NULL, data can be inserted only when non-null values are different.

   The following table lists the behaviors of the three processing modes.

   .. table:: **Table 3** Processing of NULL values in index columns in unique indexes

      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | Constraint         | All Index Columns Are NULL     | Some Index Columns Are NULL.                                                                               |
      +====================+================================+============================================================================================================+
      | NULLS DISTINCT     | Can be inserted repeatedly.    | Can be inserted repeatedly.                                                                                |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS NOT DISTINCT | Cannot be inserted repeatedly. | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+
      | NULLS IGNORE       | Can be inserted repeatedly.    | Cannot be inserted if the non-null values are equal. Can be inserted if the non-null values are not equal. |
      +--------------------+--------------------------------+------------------------------------------------------------------------------------------------------------+

   .. note::

      If **DISTRIBUTE BY REPLICATION** is not specified, the column table that contains only unique values must contain distribution columns.

-  **PRIMARY KEY index_parameters**

   **PRIMARY KEY ( column_name [, ... ] ) index_parameters**

   Specifies the primary key constraint specifies that a column or columns of a table can contain only unique (non-duplicate) and non-null values.

   Only one primary key can be specified for a table.

   .. note::

      If **DISTRIBUTE BY REPLICATION** is not specified, the column set with a primary key constraint must contain distributed columns.

-  **REFERENCES reftable [ ( refcolumn ) ]**

   The foreign key constraint requires that the group consisting of one or more columns in the new table should contain and match only the referenced column values in the referenced table. The referenced column should be the only column or primary key in the referenced table.

   .. note::

      GaussDB(DWS) does not check foreign key constraints. When using foreign key constraints, you need to use the **check_foreign_key_constraint** function to check whether the data in the foreign key table meets the foreign key constraints.

-  **DEFERRABLE \| NOT DEFERRABLE**

   Controls whether the constraint can be deferred. A constraint that is not deferrable will be checked immediately after every command. Checking of constraints that are deferrable can be postponed until the end of the transaction using the **SET CONSTRAINTS** command. **NOT DEFERRABLE** is the default value. Currently, only **UNIQUE** and **PRIMARY KEY** constraints of row-store tables accept this clause. All the other constraints are not deferrable.

-  **PARTIAL CLUSTER KEY**

   Specifies a partial cluster key for storage. When importing data to a column-store table, you can perform local data sorting by specified columns (single or multiple).

-  **INITIALLY IMMEDIATE \| INITIALLY DEFERRED**

   If a constraint is deferrable, this clause specifies the default time to check the constraint.

   -  If the constraint is **INITIALLY IMMEDIATE** (default value), it is checked after each statement.
   -  If the constraint is **INITIALLY DEFERRED**, it is checked only at the end of the transaction.

   The constraint check time can be altered using the **SET CONSTRAINTS** command.

Examples
--------

Create a V3 table with storage and compute decoupled (supported only in the storage-compute decoupling 3.0 version).

::

   CREATE TABLE  public.t1
   (
   id integer not null,
   data integer,
   age integer
   )
   WITH (ORIENTATION =COLUMN, COLVERSION =3.0)
   DISTRIBUTE BY ROUNDROBIN;

Specify the cache policy when creating a table (supported only in clusters of the storage-compute decoupling 3.0 version).

::

   CREATE TABLE Sports
   (
       N_NATIONKEY  INT NOT NULL
     , N_NAME       CHAR(25) NOT NULL
     , N_REGIONKEY  INT NOT NULL
     , N_COMMENT    VARCHAR(152)
   ) WITH (orientation = column, colversion = 3.0, cache_policy = 'HPL: Balls, Basketball')
   tablespace cu_obs_tbs
   DISTRIBUTE BY ROUNDROBIN
   partition by list(N_NAME)
   (
     partition Balls values ('Basketball', 'football', 'badminton'),
     partition Athletics values ('High jump', 'long jump', 'javelin'),
     partition Water_Sports values ('Surfing', 'diving', 'swimming'),
     partition Shooting values ('air guns', 'Rifles', 'archery'),
     partition rest values (DEFAULT)
   );

Define a unique column constraint for the table:

::

   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT          ,
       C_PHONE       CHAR(15)     ,
       C_ACCTBAL     DECIMAL(15,2)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Define a primary key table constraint for the table. You can define a primary key table constraint on one or more columns of a table:

::

   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT       ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT          ,
       C_PHONE       CHAR(15)     ,
       C_ACCTBAL     DECIMAL(15,2)   ,
       CONSTRAINT C_CUSTKEY_KEY PRIMARY KEY(C_CUSTKEY,C_NAME)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY,C_NAME);

Define the **CHECK** column constraint:

::

   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)  ,
       C_ADDRESS     VARCHAR(40)  ,
       C_NATIONKEY   INT NOT NULL  CHECK (C_NATIONKEY > 0)
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Define the **CHECK** table constraint:

.. code-block::

   CREATE TABLE CUSTOMER
   (
       C_CUSTKEY     BIGINT NOT NULL CONSTRAINT C_CUSTKEY_pk PRIMARY KEY  ,
       C_NAME        VARCHAR(25)      ,
       C_ADDRESS     VARCHAR(40)      ,
       C_NATIONKEY   INT              ,
       CONSTRAINT C_CUSTKEY_KEY2 CHECK(C_CUSTKEY > 0 AND C_NAME <> '')
   )
   DISTRIBUTE BY HASH(C_CUSTKEY);

Create a column-store table and specify the storage format and compression mode:

::

   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   WITH (ORIENTATION = COLUMN, COMPRESSION=HIGH,COLVERSION=2.0)
   DISTRIBUTE BY HASH (ca_address_sk);

Use **DEFAULT** to declare a default value for column **W_STATE**:

::

   CREATE TABLE warehouse_t
   (
       W_WAREHOUSE_SK            INTEGER                NOT NULL,
       W_WAREHOUSE_ID            CHAR(16)               NOT NULL,
       W_WAREHOUSE_NAME          VARCHAR(20)   UNIQUE DEFERRABLE,
       W_WAREHOUSE_SQ_FT         INTEGER                        ,
       W_COUNTY                  VARCHAR(30)                    ,
       W_STATE                   CHAR(2)            DEFAULT 'GA',
       W_ZIP                     CHAR(10)
   );

Create the **CUSTOMER_bk** table in LIKE mode:

::

   CREATE TABLE CUSTOMER_bk (LIKE CUSTOMER INCLUDING ALL);

Helpful Links
-------------

:ref:`ALTER TABLE <dws_06_0142>`, :ref:`12.101-RENAME TABLE <dws_06_0276>`, and :ref:`DROP TABLE <dws_06_0208>`

.. |image1| image:: /_static/images/en-us_image_0000002012629900.png
