:original_name: dws_06_0226.html

.. _dws_06_0226:

VACUUM
======

Function
--------

**VACUUM** reclaims storage space occupied by tables or B-tree indexes. In normal database operation, rows that have been deleted or obsoleted by an update are not physically removed from their table; they remain present until a **VACUUM** is done. Therefore, it is necessary to execute **VACUUM** periodically, especially on frequently-updated tables.

Precautions
-----------

-  With no table specified, **VACUUM** processes all the tables that the current user has permission to vacuum in the current database. With a table specified, **VACUUM** processes only that table.
-  To vacuum a table, you must ordinarily be the table's owner or the system administrator. However, database owners are allowed to **VACUUM** all tables in their databases, except shared catalogs. (The restriction for shared catalogs means that a true database-wide **VACUUM** can only be executed by the system administrator). **VACUUM** skips over any tables that the calling user does not have the permission to vacuum.
-  **VACUUM** cannot be executed inside a transaction block.
-  It is recommended that active production databases be vacuumed frequently (at least nightly), in order to remove dead rows. After adding or deleting a large number of rows, it might be a good idea to execute the **VACUUM ANALYZE** command for the affected table. This will update the system catalogs with the results of all recent changes, and allow the database query optimizer to make better choices in planning user queries.
-  **FULL** is recommended only in special scenarios. An example is when you have deleted or updated most of the rows in a table and would like the table to physically shrink to occupy less disk space. **VACUUM FULL** will usually shrink the table more than a plain **VACUUM** would. If the physical space usage does not decrease after you run the command, check whether there are other active transactions (that have started before you delete data transactions and not ended before you run **VACUUM FULL**). If there are such transactions, run this command again when the transactions quit.
-  **VACUUM** causes a substantial increase in I/O traffic, which might cause poor performance for other active sessions. Therefore, you are advised to do **VACUUM** to the database when services are not busy.
-  When **VERBOSE** is specified, **VACUUM** prints progress messages to indicate which table is currently being processed. Various statistics about the tables are printed as well. However, if the **VERBOSE** option is specified in **VACUUM** executed for column-store tables, no output will be displayed.
-  When the option list is surrounded by parentheses, the options can be written in any order. If there are no brackets, the options must be given in the order displayed in the syntax.
-  **VACUUM** and **VACUUM FULL** clear deleted tuples after the delay specified by **vacuum_defer_cleanup_age**.
-  **VACUUM ANALYZE** performs a **VACUUM** and then an **ANALYZE** for each selected table. This is a handy combination form for routine maintenance scripts.
-  Plain **VACUUM** (without **FULL**) recycles space and makes it available for reuse. This form of the command can operate in parallel with normal reading and writing of the table, as an exclusive lock is not obtained. **VACUUM FULL** executes wider processing, including moving rows across blocks to compress tables so they occupy minimum number of disk blocks. This form is much slower and requires an exclusive lock on each table while it is being processed.
-  When you do **VACUUM** to a column-store table, the following operations are internally performed: data in the delta table is migrated to the primary table, and the delta and desc tables of the primary table are vacuumed. **VACUUM** does not reclaim the storage space of the delta table. To reclaim it, do **VACUUM DELTAMERGE** to the column-store table.
-  If you perform VACUUM FULL when a long-running query accesses a system table, the long-running query may prevent VACUUM FULL from accessing the system table. As a result, the connection times out and an error is reported.

Syntax
------

-  Reclaim space and update statistics information, with no requirements for the order of keywords.

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

-  For HDFS and column-store tables, migrate data from the delta table to the primary table.

   ::

      VACUUM DELTAMERGE [ table_name ];

-  For HDFS tables, delete the empty value partition directory of HDFS table in HDFS storage.

   ::

      VACUUM HDFSDIRECTORY [ table_name ];

Parameter Description
---------------------

-  **FULL**

   Selects "FULL" vacuum, which can reclaim more space, but takes much longer and exclusively locks the table. This method also requires additional disk space, because it writes a new copy of the table and does not free the old copy until the operation is complete. Generally, this option is used only when a large amount of space needs to be reclaimed from a table.

   **FULL** options can also contain the **COMPACT** parameter, which is only used for the HDFS table. Specifying the **COMPACT** parameter improves **VACUUM FULL** operation performance.

   **COMPACT** and **PARTITION** cannot be used at the same time.

   .. note::

      Using **FULL** will cause statistics missing. To collect statistics, add the keyword **ANALYZE** to **VACUUM FULL**.

-  **FREEZE**

   Is equivalent to executing **VACUUM** with the **vacuum_freeze_min_age** parameter set to **zero**.

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

-  **partition_name**

   Indicates the partition name of a specific table to vacuum. Defaults are all partitions.

-  **DELTAMERGE**

   (For HDFS and column-store tables) Migrates data from the delta table to primary tables. If the data volume of the delta table is less than 60,000 rows, the data will not be migrated. Otherwise, the data will be migrated to HDFS, and the delta table will be cleared by **TRUNCATE**. For a column-store table, this operation always transfers all data in the delta table to the CU.

   .. note::

      The following DFX functions are provided to return the data storage in the delta table of a column-store table (for an HDFS table, it can be returned by **EXPLAIN ANALYZE**):

      -  pgxc_get_delta_info(TEXT): The input parameter is a column-store table name. The delta table information on each node is collected and displayed, including the number of active tuples, table size, and maximum block ID.
      -  get_delta_info(TEXT): The input parameter is a column-store table name. The system summarizes the results returned from pgxc_get_delta_info and returns the total number of active tuples, total table size, and maximum block ID in the delta table. When querying delta information about a temporary table, you need to specify the schema of the temporary table. Otherwise, an error is reported, indicating that the table cannot be found.

-  **HDFSDIRECTORY**

   Deletes the empty value partition directory of HDFS table in HDFS storage for HDFS table.

Examples
--------

Delete all tables in the current database.

::

   VACUUM;

Reclaim the space of partition **P2** of the **tpcds.web_returns_p1** table without updating statistics.

::

   VACUUM FULL tpcds.web_returns_p1 PARTITION(P2);

Reclaim the **tpcds.web_returns_p1** table and update statistics.

::

   VACUUM FULL ANALYZE tpcds.web_returns_p1;

Delete all tables in the current database and collect statistics about the query optimizer.

::

   VACUUM ANALYZE;

Delete only the **reason** table.

::

   VACUUM (VERBOSE, ANALYZE) reason;
