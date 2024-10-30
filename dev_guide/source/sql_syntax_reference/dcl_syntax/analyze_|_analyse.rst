:original_name: dws_06_0245.html

.. _dws_06_0245:

ANALYZE \| ANALYSE
==================

Function
--------

Collects statistics about table contents in databases, and stores the results in the **PG_STATISTIC** system catalog. The execution plan generator uses these statistics to determine which one is the most effective execution plan.

If no parameters are specified, **ANALYZE** analyzes each table and partitioned table in the database. You can also specify **table_name**, **column**, and **partition_name** to limit the analysis to a specified table, column, or partitioned table.

Users who can execute **ANALYZE** on a specific table include the owner of the table, owner of the database where the table is, users with the **ANALYZE** permission on the table, users who are granted the **gs_role_analyze_any** role, and users with the **SYSADMIN** attribute.

To collect statistics using percentage sampling, you must have the **ANALYZE** and **SELECT** permissions.

**ANALYZE** and **ANALYSE VERIFY** are used to check whether data files of common tables (row-store and column-store tables) in a database are damaged. Currently, this function does not support HDFS tables.

Precautions
-----------

-  Performs the **ANALYZE** operation on an unsharded table in an anonymous block, transaction block, function, or stored procedure. This function is supported only by 8.1.1 and later versions.
-  However, for analyzing an entire database, the **ANALYZE** operation of each table is in different transactions. Therefore, the current version does not support the **ANALYZE** execution for the entire database in anonymous blocks, transaction blocks, functions, or stored procedures.
-  Statistics updates of PG_CLASS related columns cannot be rolled back.
-  Most **ANALYZE VERIFY** operations are used for abnormal scenario detection, and require a release version. Remote read is not triggered in the **ANALYZE VERIFY** scenario. Therefore, the remote read parameter does not take effect. If the system detects that the page is damaged due to an error in the key system table, the system reports an error and stops the detection.

Syntax
------

-  Collect statistics information about a table.

   ::

      { ANALYZE | ANALYSE } [ { VERBOSE } ]
          [ table_name [ ( column_name [, ...] ) ] ];

-  Collect statistics about a partition.

   ::

      { ANALYZE | ANALYSE } [ { VERBOSE } ]
          [ table_name [ ( column_name [, ...] ) ] ]
          PARTITION ( partition_name ) ;

   .. note::

      -  Temporary sampling tables cannot be used to collect partition statistics.
      -  Multi-column statistics and expression statistics on partitions are not supported.

-  Collect statistics about a foreign table.

   ::

      { ANALYZE | ANALYSE } [ VERBOSE ]
          { foreign_table_name | FOREIGN TABLES };

-  Collect statistics about multiple columns.

   ::

      {ANALYZE | ANALYSE} [ VERBOSE ]
          table_name (( column_1_name, column_2_name [, ...] ));

   .. note::

      -  To sample data in percentage, set **default_statistics_target** to a negative number.
      -  The statistics about a maximum of 32 columns can be collected at a time.
      -  You are not allowed to collect statistics about multiple columns in system catalogs, or HDFS foreign tables.

-  Check the data files in the current database.

   ::

      {ANALYZE | ANALYSE} VERIFY {FAST|COMPLETE};

   .. note::

      -  All operations on the database are supported. Because many tables are involved, you are advised to save the result in redirection mode: **gsql -d database -p port -f "verify.sql"> verify_warning.txt 2>&1**.
      -  HDFS tables (internal and foreign tables), temporary tables, and unlog tables are not supported.
      -  Note: Only visible tables are checked. Internal table check involves foreign tables on which the internal tables depend and are not displayed or presented externally.
      -  This command can be used to process tolerant errors. The assert operation in a debug version may cause the core to fail to execute commands. Therefore, you are advised to perform this operation in a release version.
      -  If a key system table is damaged during a full database operation, an error is reported and the operation stops.

-  Check the data files of tables and indexes.

   ::

      {ANALYZE | ANALYSE} VERIFY {FAST|COMPLETE} table_name|index_name [CASCADE];

   .. note::

      -  You can perform operations on common tables and index tables, but cannot perform **CASCADE** operations on index tables. The reason is that **CASCADE** is used to process all index tables of the primary table. When the index table is checked separately, **CASCADE** is not required.
      -  HDFS tables (internal and foreign tables), temporary tables, and unlog tables are not supported.
      -  When the primary table is checked, the internal tables of the primary table, such as the **toast** table and **cudesc** table, are also checked.
      -  When the system displays a message indicating that the index table is damaged, you are advised to run the **reindex** command to recreate the index.

-  Check the data file of the table partition.

::

   {ANALYZE | ANALYSE} VERIFY {FAST|COMPLETE} table_name PARTITION {(partition_name)}[CASCADE];

.. note::

   -  You can detect a single partition of a table, but cannot perform the **CASCADE** operation on index tables.
   -  HDFS tables (internal and foreign tables), temporary tables, and unlog tables are not supported.

Parameter Description
---------------------

-  **VERBOSE**

   Enables the display of progress messages.

   .. note::

      If this parameter is specified, progress information is displayed by **ANALYZE** to indicate the table that is being processed, and statistics about the table are printed.

-  **table_name**

   Specifies the name (possibly schema-qualified) of a specific table to analyze. If omitted, all regular tables (but not foreign tables) in the current database are analyzed.

   Currently, you can use **ANALYZE** to collect statistics about row-store tables, column-store tables, HDFS tables, ORC- or CARBONDATA-formatted OBS foreign tables, and foreign tables for collaborative analysis.

   Value range: an existing table name

-  **column_name**, **column_1_name**, **column_2_name**

   Specifies the name of a specific column to analyze. All columns are analyzed by default.

   Value range: an existing column name

-  **partition_name**

   Assumes the table is a partitioned table. You can specify **partition_name** following the keyword **PARTITION** to analyze the statistics of this table. Currently the partitioned table supports the syntax of analyzing a partitioned table, but does not execute this syntax.

   Value range: a partition name in a table

-  **foreign_table_name**

   Specifies the name (possibly schema-qualified) of a specific table to analyze. The data of the table is stored in HDFS.

   Value range: an existing table name

-  **FOREIGN TABLES**

   Analyzes HDFS foreign tables stored in HDFS and accessible to the current user.

-  **index_name**

   Name of the index table to be analyzed. The name may contain the schema name.

   Value range: an existing table name

-  **FAST|COMPLETE**

   For row-store tables, the CRC and page header of row-store tables are verified in **FAST** mode. If the verification fails, an alarm is reported. In **COMPLETE** mode, parse and verify the pointers and tuples of row-store tables. For column-store tables, the CRC and magic of column-store tables are verified in **FAST** mode. If the verification fails, an alarm is reported. In **COMPLETE** mode, parse and verify CU of column-store tables.

-  **CASCADE**

   In **CASCADE** mode, all indexes of the current table are checked.

Examples
--------

-  Do **ANALYZE** to update statistics in the **customer_info** table:

   ::

      ANALYZE customer_info;

-  Do **ANALYZE VERBOSE** to update statistics and display table information in the **customer_info** table:

   ::

      ANALYZE VERBOSE customer_info;
      INFO:  analyzing "cstore.pg_delta_3394584009"(cn_5002 pid=53078)
      INFO:  analyzing "public.customer_info"(cn_5002 pid=53078)
      INFO:  analyzing "public.customer_info" inheritance tree(cn_5002 pid=53078)
      ANALYZE
