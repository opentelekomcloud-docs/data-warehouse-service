:original_name: dws_04_0690.html

.. _dws_04_0690:

GS_INSTR_UNIQUE_SQL
===================

Unique SQL Definition
---------------------

The database parses each received SQL text string and generates an internal parsing tree. The database traverses the parsing tree and ignores constant values in the parsing tree. In this case, an integer value is calculated using a certain algorithm. This integer is used as the Unique SQL ID to uniquely identify this type of SQL. SQL statements with the same Unique SQL ID are called Unique SQL statements.

.. _en-us_topic_0000001764650132__section1095316139818:

Examples
--------

Assume that the user enters the following SQL statements in sequence:

.. code-block::

   select * from t1 where id = 1;
   select * from t1 where id = 2;

The statistics of the two SQL statements are aggregated to the same Unique SQL statement.

.. code-block::

   select * from t1 where id = ?;

GS_INSTR_UNIQUE_SQL View
------------------------

The **GS_INSTR_UNIQUE_SQL** view displays the execution information about the Unique SQL statements collected by the current node, including:

-  Unique SQL ID and normalized SQL text string. The normalized SQL text is described in :ref:`Examples <en-us_topic_0000001764650132__section1095316139818>`. Generally, constant values are ignored during Unique SQL ID calculation in DML statements. However, constant values cannot be ignored in DDL, DCL, and parameter setting statements.
-  Number of execution times (number of successful execution times) and response time (SQL execution time in the database, including the maximum, minimum, and total time)
-  Cache or I/O information, including the number of physical reads and logical reads of blocks. Only information about successfully executed SQL statements on each DN is collected. The statistical value is related to factors such as the amount of data processed during query execution, used memory, whether the query is executed for multiple times, memory management policy, and whether there are other concurrent queries. The statistical value reflects the number of physical reads and logical reads of the buffer block in the entire query execution process. The statistical value may vary according to the execution time.
-  Row activities, such as the number of returned rows, updated rows, inserted rows, deleted rows, sequentially scanned rows, and randomly scanned rows in the result set of the **SELECT** statement. Except that the number of rows returned by the result set is the same as the number of rows in the result set of the **SELECT** statement and is recorded only on the CN, the activity information of other rows is recorded on the DN. The statistical value reflects the row activities during the entire query execution process, including scanning and modifying related system tables, metadata tables, and data tables. The value of this parameter is related to the data volume and related parameter settings. That is, the statistical value is greater than or equal to the scanning and modification times of actual data tables.
-  Time distribution, including DB_TIME/CPU_TIME/EXECUTION_TIME/PARSE_TIME/PLAN_TIME/REWRITE_TIME/PL_EXECUTION_TIME/PL_COMPILATION_TIME/NET_SEND_TIME/DATA_IO_TIME. For details, see :ref:`Table 1 <en-us_topic_0000001764650132__table1350052261017>`. The information is collected on both CNs and DNs and is displayed during view query.
-  Number of soft and hard parsing times, such as the number of soft parsing times (cache plan) and hard parsing times (generation plan). If the cache plan is executed this time, the number of soft parsing times increases by 1. If the generation plan is regenerated this time, the number of hard parsing times increases by 1. This number is counted on both CNs and DNs and is displayed during view query.

The Unique SQL statistics function has the following restrictions:

-  Detailed statistics are displayed only for successfully executed SQL statements. Otherwise, only query, node, and user information are recorded.
-  If the Unique SQL statistics collection function is enabled, the CN collects statistics on all received queries, including tool and user queries.
-  If an SQL statement contains multiple SQL statements or similar stored procedures, a Unique SQL statement is generated for the outermost SQL statement. The statistics of all sub-SQL statements are summarized to the Unique SQL record.
-  The response time statistics of Unique SQL does not include the time of the **NET_SEND_TIME** phase. Therefore, there is no comparison between **EXECUTION_TIME** and **elapse_time**.
-  **parse_time** of clauses cannot be calculated for **begin;...;commit** and similar transaction blocks.

When a common user accesses the **GS_INSTR_UNIQUE_SQL** view, only the Unique SQL information about the user is displayed. When an administrator accesses the **GS_INSTR_UNIQUE_SQL** view, all Unique SQL information about the current node is displayed. The **GS_INSTR_UNIQUE_SQL** view can be queried on both CNs and DNs. The DN displays the Unique SQL statistics of the local node, and the CN displays the complete Unique SQL statistics of the local node. That is, the CN collects the Unique SQL execution information of the CN from other CNs and DNs and displays the information. You can query the **GS_INSTR_UNIQUE_SQL** view to locate the Top SQL statements that consume different resources, providing a basis for cluster tuning and maintenance.

The background thread checks all Unique SQL statements every hour and deletes the Unique SQL statements whose **last_time** is **instr_unique_sql_timeout** hours ago.

.. _en-us_topic_0000001764650132__table1350052261017:

.. table:: **Table 1** GS_INSTR_UNIQUE_SQL columns

   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | Column              | Type                     | Description                                                                                                                                                                                                           |
   +=====================+==========================+=======================================================================================================================================================================================================================+
   | node_name           | Name                     | Name of the CN that receives SQL statements                                                                                                                                                                           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | node_id             | Integer                  | Node ID, which is the same as the value of **node_id** in the **pgxc_node** table                                                                                                                                     |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_name           | Name                     | Username                                                                                                                                                                                                              |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | user_id             | OID                      | User ID                                                                                                                                                                                                               |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | unique_sql_id       | Bigint                   | Normalized Unique SQL ID                                                                                                                                                                                              |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | query               | Text                     | Normalized SQL text                                                                                                                                                                                                   |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_calls             | Bigint                   | Number of successful execution times                                                                                                                                                                                  |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | min_elapse_time     | Bigint                   | Minimum running time of the SQL statement in the database (unit: μs)                                                                                                                                                  |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | max_elapse_time     | Bigint                   | Maximum running time of SQL statements in the database (unit: μs)                                                                                                                                                     |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | total_elapse_time   | Bigint                   | Total running time of SQL statements in the database (unit: μs)                                                                                                                                                       |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_returned_rows     | Bigint                   | Row activity - Number of rows in the result set returned by the **SELECT** statement                                                                                                                                  |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_tuples_fetched    | Bigint                   | Row activity - Randomly scan rows (column-store tables/foreign tables are not counted.)                                                                                                                               |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_tuples_returned   | Bigint                   | Row activity - Sequential scan rows (Column-store tables/foreign tables are not counted.)                                                                                                                             |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_tuples_inserted   | Bigint                   | Row activity - Inserted rows                                                                                                                                                                                          |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_tuples_updated    | Bigint                   | Row activity - Updated rows                                                                                                                                                                                           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_tuples_deleted    | Bigint                   | Row activity - Deleted rows                                                                                                                                                                                           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_blocks_fetched    | Bigint                   | Block access times of the buffer, that is, physical read/I/O                                                                                                                                                          |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_blocks_hit        | Bigint                   | Block hits of the buffer, that is, logical read/cache                                                                                                                                                                 |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_soft_parse        | Bigint                   | Number of soft parsing times (cache plan)                                                                                                                                                                             |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | n_hard_parse        | Bigint                   | Number of hard parsing times (generation plan)                                                                                                                                                                        |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | db_time             | Bigint                   | Valid DB execution time, including the waiting time and network sending time. If multiple threads are involved in query execution, the value of **DB_TIME** is the sum of **DB_TIME** of multiple threads (unit: μs). |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | cpu_time            | Bigint                   | CPU execution time, excluding the sleep time (unit: μs)                                                                                                                                                               |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | execution_time      | Bigint                   | SQL execution time in the query executor, DDL statements, and statements (such as Copy statements) that are not executed by the executor are not counted (unit: μs).                                                  |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | parse_time          | Bigint                   | SQL parsing time (unit: μs)                                                                                                                                                                                           |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | plan_time           | Bigint                   | SQL generation plan time (unit: μs)                                                                                                                                                                                   |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | rewrite_time        | Bigint                   | SQL rewriting time (unit: μs)                                                                                                                                                                                         |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pl_execution_time   | Bigint                   | Execution time of the plpgsql procedural language function (unit: μs)                                                                                                                                                 |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | pl_compilation_time | Bigint                   | Compilation time of the plpgsql procedural language function (unit: μs)                                                                                                                                               |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | net_send_time       | Bigint                   | Network time, including the time spent by the CN in sending data to the client and the time spent by the DN in sending data to the CN (unit: μs)                                                                      |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | data_io_time        | Bigint                   | File I/O time (unit: μs)                                                                                                                                                                                              |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | first_time          | Timestamp with time zone | Time of the first SQL statement execution                                                                                                                                                                             |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
   | last_time           | Timestamp with time zone | Time of the last SQL statement execution                                                                                                                                                                              |
   +---------------------+--------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
