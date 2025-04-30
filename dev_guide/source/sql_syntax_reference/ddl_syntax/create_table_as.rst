:original_name: dws_06_0178.html

.. _dws_06_0178:

CREATE TABLE AS
===============

Function
--------

**CREATE TABLE AS** creates a table based on the results of a query.

It creates a table and fills it with data obtained using **SELECT**. The table columns have the names and data types associated with the output columns of the **SELECT**. Except that you can override the **SELECT** output column names by giving an explicit list of new column names.

**CREATE TABLE AS** queries once the source table and writes data in the new table. The query result view changes when the source table changes. In contrast, a view re-evaluates its defining **SELECT** statement whenever it is queried.

Precautions
-----------

-  This command cannot be used to create a partitioned table.
-  If an error occurs when you create a table, after the system is recovered, the system probably cannot automatically clear the created disk file whose size is not 0. This problem seldom occurs.

Syntax
------

::

   CREATE [ [ GLOBAL | LOCAL | VOLATILE ] { TEMPORARY | TEMP } | UNLOGGED ] TABLE table_name
       [ (column_name [, ...] ) ]
       [ WITH ( {storage_parameter = value} [, ... ] ) ]
       [ COMPRESS | NOCOMPRESS ]

       [ DISTRIBUTE BY { REPLICATION | ROUNDROBIN | { [HASH ] ( column_name ) } } ]

       [ COMMENT [=] 'text' ]
       AS query
       [ WITH [ NO ] DATA ];

Parameter Description
---------------------

-  [ GLOBAL \| LOCAL \| VOLATILE ] { TEMPORARY \| TEMP }

   Specifies the type of a temporary table. The value can be **GLOBAL**, **LOCAL**, or **VOLATILE**. For details, see :ref:`·GLOBAL | LOCAL | VOLATI... <en-us_topic_0000001764675138__l40601c13ccdb4b5d85be38edd4f99676>` in the section "CREATE TABLE".

-  **UNLOGGED**

   Specifies that the table is created as an unlogged table. Data written to unlogged tables is not written to the write-ahead log, which makes them considerably faster than ordinary tables. However, they are not crash-safe: an unlogged table is automatically truncated after a crash or unclean shutdown. The contents of an unlogged table are also not replicated to standby servers. Any indexes created on an unlogged table are automatically unlogged as well.

   -  Usage scenario: Unlogged tables do not ensure safe data. Users can back up data before using unlogged tables; for example, users should back up the data before a system upgrade.
   -  Troubleshooting: If data is missing in the indexes of unlogged tables due to some unexpected operations such as an unclean shutdown, users should re-create the indexes with errors.

      .. caution::

         #. The UNLOGGED table uses no primary/standby mechanism. In the case of system faults or abnormal breakpoints, data loss may occur. Therefore, the UNLOGGED table cannot be used to store basic data.
         #. Starting from version 9.1.0, UNLOGGED tables are automatically saved in the **pg_unlogged** tablespace and cannot be moved or assigned to other tablespaces.
         #. After an earlier version is upgraded to 9.1.0, the UNLOGGED table created in the earlier version is still stored in the original tablespace.
         #. If the instance restarts unexpectedly, the UNLOGGED table will be reset, which can impact the instance's recovery time objective (RTO). Version 9.1.0 has a script called **switch_unlogged_tablespace.py** that can move unlogged tables to optimize the recovery time objective (RTO). This script works together with the GUC parameter **enable_unlogged_tablespace_compat**.

-  **table_name**

   Specifies the name of the table to be created.

   Value range: a string. It must comply with the naming convention.

-  **column_name**

   Specifies the name of a column to be created in the new table.

   Value range: a string. It must comply with the naming convention.

-  **WITH ( storage_parameter [= value] [, ... ] )**

   Specifies an optional storage parameter for a table or an index. See details of parameters below.

   -  FILLFACTOR

      The fillfactor of a table is a percentage between 10 and 100. 100 (complete packing) is the default value. When a smaller fillfactor is specified, **INSERT** operations pack table pages only to the indicated percentage. The remaining space on each page is reserved for updating rows on that page. This gives **UPDATE** a chance to place the updated copy of a row on the same page, which is more efficient than placing it on a different page. For a table whose records are never updated, setting the fillfactor to 100 (complete packing) is the appropriate choice, but in heavily updated tables smaller fillfactors are appropriate. The parameter is only valid for row-store tables.

      Value range: 10-100

   -  ORIENTATION

      Valid value:

      **COLUMN**: The data will be stored in columns.

      **ROW** (default value): The data will be stored in rows.

   -  COMPRESSION

      Specifies the compression level of the table data. It determines the compression ratio and time. Generally, the higher the level of compression, the higher the ratio, the longer the time, and the lower the level of compression, the lower the ratio, the shorter the time. The actual compression ratio depends on the distribution characteristics of loading table data.

      Valid value:

      The valid values for column-store tables are **YES**/**NO** and **LOW**/**MIDDLE**/**HIGH**, and the default is **LOW**.

      .. note::

         Currently, row-store table compression is not supported.

   -  MAX_BATCHROW

      Specifies the maximum of a storage unit during data loading process. The parameter is only valid for column-store tables.

      Value range: 10000 to 60000

      Default value: **60000**

   -  PARTIAL_CLUSTER_ROWS

      Specifies the number of records to be partial cluster stored during data loading process. The parameter is only valid for column-store tables.

      Value range: 600000 to 2147483647

      Default value: 4,200,000

   -  enable_delta

      Specifies whether to enable delta tables in column-store tables. The parameter is only valid for column-store tables.

      Using column-store tables with delta tables is not recommended. This may cause disk bloat and performance deterioration due to delayed merge.

      Default value: **off**

   -  COLVERSION

      Specifies the version of the column-store format. You can switch between different storage formats.

      Valid value:

      **1.0**: Each column in a column-store table is stored in a separate file. The file name is **relfilenode.C1.0**, **relfilenode.C2.0**, **relfilenode.C3.0**, or similar.

      **2.0**: All columns of a column-store table are combined and stored in a file. The file is named **relfilenode.C1.0**.

      Default value: **2.0**

      .. note::

         When creating a column-store table, set **COLVERSION** to **2.0**. Compared with the **1.0** storage format, the performance is significantly improved:

         #. The time required for creating a column-store wide table is significantly reduced.
         #. In the Roach data backup scenario, the backup time is significantly reduced.
         #. The build and catch up time is greatly reduced.
         #. The occupied disk space decreases significantly.

   -  SKIP_FPI_HINT

      Indicates whether to skip the hint bits operation when the full-page writes (FPW) log needs to be written during sequential scanning.

      Default value: **false**

      .. note::

         If **SKIP_FPI_HINT** is set to **true** and the checkpoint operation is performed on a table, no Xlog will be generated when the table is sequentially scanned. This applies to intermediate tables that are queried less frequently, reducing the size of Xlogs and improving query performance.

-  **COMPRESS / NOCOMPRESS**

   Specifies the keyword **COMPRESS** during the creation of a table, so that the compression feature is triggered in the case of a bulk **INSERT** operation. If this feature is enabled, a scan is performed for all tuple data within the page to generate a dictionary and then the tuple data is compressed and stored. If **NOCOMPRESS** is specified, the table is not compressed.

   Default value: **NOCOMPRESS**, tuple data is not compressed before storage.

-  **DISTRIBUTE BY**

   Specifies how the table is distributed or replicated between DNs.

   -  **REPLICATION**: Each row in the table exists on all DNs, that is, each DN has complete table data.
   -  **ROUNDROBIN**: Each row in the table is sent to each DN in sequence. This distribution policy prevents data skew. However, data distribution nodes are random. As a result, there is a higher probability that table redistribution is triggered during computing. This distribution policy is recommended for large tables with severe column skew. This value is supported only in 8.1.2 or later.
   -  **HASH (column_name)**: Each row of the table will be placed into all the DNs based on the hash value of the specified column.

   .. important::

      -  When **DISTRIBUTE BY HASH (column_name)** is specified, the primary key and its unique index must contain the **column_name** column.
      -  When **DISTRIBUTE BY HASH (column_name)** in a referenced table is specified, the foreign key of the reference table must contain the **column_name** column.

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

-  **COMMENT [=] 'text'**

   The **COMMENT** clause can specify table comments during table creation.

-  **AS query**

   Indicates a **SELECT** or **VALUES** command, or an **EXECUTE** command that runs a prepared **SELECT**, or **VALUES** query.

-  **[ WITH [ NO ] DATA ]**

   Specifies whether the data produced by the query should be copied into the new table. By default, the data is copied. If the **NO** parameter is used, the data is not copied.

Examples
--------

Create the **store_returns_t1** table and insert numbers that are greater than 4795 in the **sr_item_sk** column of the **store_returns** table:

::

   CREATE TABLE store_returns_t1 AS SELECT * FROM store_returns WHERE sr_item_sk > '4795';

Copy **store_returns** to create the **store_returns_t2** table:

::

   CREATE TABLE store_returns_t2 AS table store_returns;

Helpful Links
-------------

:ref:`CREATE TABLE <dws_06_0177>`, :ref:`SELECT <dws_06_0238>`
