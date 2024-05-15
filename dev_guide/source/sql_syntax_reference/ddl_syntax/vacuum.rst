:original_name: dws_06_0226.html

.. _dws_06_0226:

VACUUM
======

Function
--------

**VACUUM** reclaims storage space occupied by tables or B-tree indexes. In normal database operation, rows that have been deleted are not physically removed from their table; they remain present until a **VACUUM** is done. Therefore, it is necessary to execute **VACUUM** periodically, especially on frequently-updated tables.

Precautions
-----------

-  With no table specified, **VACUUM** processes all the tables that the current user has permission to vacuum in the current database. With a table specified, **VACUUM** processes only that table.
-  To perform **VACUUM** on a table, you must be the owner of the table, granted with the **VACUUM** permission on the table, or granted with the **gs_role_vacuum_any** role. By default, a system administrator has this permission. However, database owners are allowed to **VACUUM** all tables in their databases, except shared catalogs. (The restriction for shared catalogs means that a true database-wide **VACUUM** can only be executed by the system administrator). **VACUUM** skips over any tables that the calling user does not have the permission to vacuum.
-  **VACUUM** cannot be executed inside a transaction block.
-  It is recommended that active production databases be vacuumed frequently (at least nightly), in order to remove dead rows. After adding or deleting a large number of rows, it might be a good idea to execute the **VACUUM ANALYZE** command for the affected table. This will update the system catalogs with the results of all recent changes, and allow the query planner to make better choices in planning queries.
-  **FULL** is recommended only in special scenarios. For example, you wish to physically narrow the table to decrease the occupied disk space after deleting most rows of a table. **VACUUM FULL** usually shrinks more table size than **VACUUM**. If the physical space usage does not decrease after you run the command, check whether there are other active transactions (that have started before you delete data transactions and not ended before you run **VACUUM FULL**). If there are such transactions, run this command again when the transactions quit.
-  **VACUUM** causes a substantial increase in I/O traffic, which might cause poor performance for other active sessions. Therefore, it is sometimes advisable to use the cost-based VACUUM delay feature.
-  When **VERBOSE** is specified, **VACUUM** prints progress messages to indicate which table is currently being processed. Various statistics about the tables are printed as well.
-  When the option list is surrounded by parentheses, the options can be written in any order. If there are no brackets, the options must be given in the order displayed in the syntax.
-  **VACUUM** and **VACUUM FULL** clear deleted tuples after the delay specified by **vacuum_defer_cleanup_age**.
-  **VACUUM ANALYZE** executes a VACUUM operation and then an ANALYZE operation for each selected table. This is a handy combination form for routine maintenance scripts.
-  Plain **VACUUM** (without **FULL**) recycles space and makes it available for reuse. This form of the command can operate in parallel with normal reading and writing of the table, as an exclusive lock is not obtained. **VACUUM FULL** executes wider processing, including moving rows across blocks to compress tables so they occupy minimum number of disk blocks. This form is much slower and requires an exclusive lock on each table while it is being processed.
-  When you do **VACUUM** to a column-store table, the following operations are internally performed: data in the delta table is migrated to the primary table, and the delta and desc tables of the primary table are vacuumed. **VACUUM** does not reclaim the storage space of the delta table. To reclaim it, do **VACUUM DELTAMERGE** to the column-store table.
-  If you perform VACUUM FULL when a long-running query accesses a system table, the long-running query may prevent VACUUM FULL from accessing the system table. As a result, the connection times out and an error is reported.
-  Running **VACUUM FULL** on a column-store partitioned table locks the table and its partitions.
-  Concurrent VACUUM FULL operations on system catalogs may cause local deadlocks.
-  When a **VACUUM FULL** operation is performed on a table, it triggers table rebuilding. During this rebuilding process, data is dumped into a new data file. Once the rebuilding is complete, the original file is deleted. However, it is important to note that if the table is large, the rebuilding process can consume a significant amount of disk space. When the disk space is insufficient, exercise caution when performing the **VACUUM FULL** operation on large tables to prevent the cluster from being read-only.

Syntax
------

-  Reclaim space and update statistics. The keyword sequence must be given in the order in which the syntax is displayed.

   ::

      VACUUM [ ( { FULL | FREEZE | VERBOSE | {ANALYZE | ANALYSE }} [,...] ) ]
          [ table_name [ (column_name [, ...] ) ] ] [ PARTITION ( partition_name ) ];

-  Reclaim space, without updating statistics information.

   ::

      VACUUM [ FULL [COMPACT] ] [ FREEZE ] [ VERBOSE ] [ table_name ] [ PARTITION ( partition_name ) ];

-  Reclaim space and update statistics information, with a specific order of keywords required.

   ::

      VACUUM [ FULL ] [ FREEZE ] [ VERBOSE ] { ANALYZE | ANALYSE } [ VERBOSE ]
          [ table_name [ (column_name [, ...] ) ] ] [ PARTITION ( partition_name ) ];

-  For HDFS tables, migrate data from the delta table to the primary table.

   ::

      VACUUM DELTAMERGE [ table_name ];

-  For HDFS tables, delete the empty value partition directory of HDFS table in HDFS storage.

   ::

      VACUUM HDFSDIRECTORY [ table_name ];

Parameter Description
---------------------

-  **FULL**

   Selects "FULL" vacuum, which can reclaim more space, but takes much longer and exclusively locks the table.

   **FULL** options can also contain the **COMPACT** parameter, which is only used for the HDFS table. Specifying the **COMPACT** parameter improves **VACUUM FULL** operation performance.

   **COMPACT** and **PARTITION** cannot be used at the same time.

   .. note::

      Using **FULL** will cause statistics missing. To collect statistics, add the keyword **ANALYZE** to **VACUUM FULL**.

-  **FREEZE**

   Specifying a **FREEZE** is equivalent to setting vacuum_freeze_min_age to **0** when VACUUM is executed.

-  **VERBOSE**

   Prints a detailed vacuum activity report for each table.

-  **ANALYZE \| ANALYSE**

   Updates statistics used by the planner to determine the most efficient way to execute a query.

-  **table_name**

   Indicates the name (optionally schema-qualified) of a specific table to vacuum.

   Value range: The name of a specific table to vacuum. Defaults are all tables in the current database.

-  **column_name**

   Indicates the name of a specific field to analyze.

   Value range: Indicates the name of a specific field to analyze. Defaults are all columns.

-  **PARTITION**

   HDFS table does not support **PARTITION**. **COMPACT** and **PARTITION** cannot be used at the same time.

   .. note::

      If the **PARTITION** and **COMPACT** parameters are used at the same time, the following error message is displayed: **COMPACT can not be used with PARTITION**.

-  **partition_name**

   Indicates the partition name of a specific table to vacuum. Defaults are all partitions.

-  **DELTAMERGE**

   (For HDFS tables) Migrates data from the delta table to primary tables. If the data volume of the delta table is less than 60,000 rows, the data will not be migrated. Otherwise, the data will be migrated to HDFS, and the delta table will be cleared by **TRUNCATE**.

-  **HDFSDIRECTORY**

   Deletes the empty value partition directory of HDFS table in HDFS storage for HDFS table.

Examples
--------

Create a partitioned table **customer_address**:

::

   DROP TABLE IF EXISTS customer_address;
   CREATE TABLE customer_address
   (
       ca_address_sk       INTEGER                  NOT NULL   ,
       ca_address_id       CHARACTER(16)            NOT NULL   ,
       ca_street_number    CHARACTER(10)                       ,
       ca_street_name      CHARACTER varying(60)               ,
       ca_street_type      CHARACTER(15)                       ,
       ca_suite_number     CHARACTER(10)
   )
   DISTRIBUTE BY HASH (ca_address_sk)
   PARTITION BY RANGE(ca_address_sk)
   (
           PARTITION P1 VALUES LESS THAN(2450815),
           PARTITION P2 VALUES LESS THAN(2451179),
           PARTITION P3 VALUES LESS THAN(2451544),
           PARTITION P4 VALUES LESS THAN(MAXVALUE)
   );

Delete all tables in the current database.

::

   VACUUM;

Reclaim the space of partition **P2** of the **customer_address** table without updating statistics.

::

   VACUUM FULL customer_address PARTITION(P2);

Reclaim the space of the **customer_address** table and update statistics.

::

   VACUUM FULL ANALYZE customer_address;

Delete all tables in the current database and collect statistics about the query optimizer.

::

   VACUUM ANALYZE;

Delete only the **reason** table.

::

   VACUUM (VERBOSE, ANALYZE) customer_address;
